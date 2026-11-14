---
name: gitops-workflow
description: ArgoCD と Flux で GitOps ワークフローを実装し、Kubernetes の自動化された宣言的デプロイと継続的リコンシリエーションを行う。GitOps 実践、Kubernetes デプロイ自動化、宣言的インフラ管理のセットアップ時に使用する。
---

# GitOps ワークフロー

ArgoCD と Flux による自動 Kubernetes デプロイのための GitOps 実装ガイド。

## 目的

ArgoCD または Flux CD を用い、OpenGitOps 原則に従った宣言的・Git ベースの継続的デリバリを実装する。

## このスキルを使うとき

- Kubernetes クラスタ向け GitOps をセットアップする
- Git からアプリケーションデプロイを自動化する
- プログレッシブデリバリ戦略を実装する
- マルチクラスタデプロイを管理する
- 自動 sync ポリシーを設定する
- GitOps でのシークレット管理を行う

## このスキルを使わないとき

- 一度きりの手動デプロイのみが必要
- クラスタアクセスやリポジトリ権限を管理できない
- Kubernetes へデプロイしない

## 手順

1. リポジトリレイアウトと desired-state の規約を定義する。
2. ArgoCD または Flux をインストールしクラスタを接続する。
3. sync ポリシー、環境、プロモーションフローを設定する。
4. ロールバックとシークレット扱いを検証する。

## 安全性

- 承認なしで本番へ auto-sync しない。
- シークレットを Git に置かず、Sealed Secrets または外部シークレットマネージャを使う。

## OpenGitOps 原則

1. **Declarative** — システム全体を宣言的に記述する
2. **Versioned and Immutable** — desired state を Git に保持する
3. **Pulled Automatically** — エージェントが desired state を pull する
4. **Continuously Reconciled** — 実際と desired の差分を継続的にリコンシルする

## ArgoCD セットアップ

### 1. インストール

```bash
# Create namespace
kubectl create namespace argocd

# Install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Get admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

**参照:** 詳細セットアップは `references/argocd-setup.md`

### 2. リポジトリ構造

```yaml
gitops-repo/
├── apps/
│   ├── production/
│   │   ├── app1/
│   │   │   ├── kustomization.yaml
│   │   │   └── deployment.yaml
│   │   └── app2/
│   └── staging/
├── infrastructure/
│   ├── ingress-nginx/
│   ├── cert-manager/
│   └── monitoring/
└── argocd/
    ├── applications/
    └── projects/
```

### 3. Application 作成

```yaml
# argocd/applications/my-app.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/org/gitops-repo
    targetRevision: main
    path: apps/production/my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

### 4. App of Apps パターン

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: applications
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/org/gitops-repo
    targetRevision: main
    path: argocd/applications
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
  syncPolicy:
    automated: {}
```

## Flux CD セットアップ

### 1. インストール

```bash
# Install Flux CLI
curl -s https://fluxcd.io/install.sh | sudo bash

# Bootstrap Flux
flux bootstrap github \
  --owner=org \
  --repository=gitops-repo \
  --branch=main \
  --path=clusters/production \
  --personal
```

### 2. GitRepository 作成

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: my-app
  namespace: flux-system
spec:
  interval: 1m
  url: https://github.com/org/my-app
  ref:
    branch: main
```

### 3. Kustomization 作成

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: my-app
  namespace: flux-system
spec:
  interval: 5m
  path: ./deploy
  prune: true
  sourceRef:
    kind: GitRepository
    name: my-app
```

## Sync ポリシー

### Auto-Sync 設定

**ArgoCD:**

```yaml
syncPolicy:
  automated:
    prune: true      # Delete resources not in Git
    selfHeal: true   # Reconcile manual changes
    allowEmpty: false
  retry:
    limit: 5
    backoff:
      duration: 5s
      factor: 2
      maxDuration: 3m
```

**Flux:**

```yaml
spec:
  interval: 1m
  prune: true
  wait: true
  timeout: 5m
```

**参照:** `references/sync-policies.md`

## プログレッシブデリバリ

### ArgoCD Rollouts による Canary デプロイ

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: my-app
spec:
  replicas: 5
  strategy:
    canary:
      steps:
      - setWeight: 20
      - pause: {duration: 1m}
      - setWeight: 50
      - pause: {duration: 2m}
      - setWeight: 100
```

### Blue-Green デプロイ

```yaml
strategy:
  blueGreen:
    activeService: my-app
    previewService: my-app-preview
    autoPromotionEnabled: false
```

## シークレット管理

### External Secrets Operator

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: db-credentials
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: aws-secrets-manager
    kind: SecretStore
  target:
    name: db-credentials
  data:
  - secretKey: password
    remoteRef:
      key: prod/db/password
```

### Sealed Secrets

```bash
# Encrypt secret
kubeseal --format yaml < secret.yaml > sealed-secret.yaml

# Commit sealed-secret.yaml to Git
```

## ベストプラクティス

1. 環境ごとに**別リポジトリまたはブランチ**を使う
2. Git リポジトリ向け **RBAC** を実装する
3. sync 失敗時の**通知**を有効化する
4. カスタムリソース向け**ヘルスチェック**を使う
5. 本番向け**承認ゲート**を実装する
6. **シークレットを Git に置かない**（External Secrets を使用）
7. 整理のため **App of Apps** パターンを使う
8. ロールバック容易化のため**リリースにタグ**を付ける
9. アラートで**sync 状態を監視**する
10. **ステージングで先に検証**する

## トラブルシューティング

**Sync 失敗:**

```bash
argocd app get my-app
argocd app sync my-app --prune
```

**Out of sync:**

```bash
argocd app diff my-app
argocd app sync my-app --force
```

## 関連スキル

- `k8s-manifest-generator` — マニフェスト作成
- `helm-chart-scaffolding` — アプリケーションのパッケージング

## アンチパターン

- 対象テストスイートで検証せずにコード変更をデプロイしない。
- 自明でないコードの前提・制約・副作用を文書化しないで進めない。

## 6) ナレッジのキャプチャ

## Memory Sync

主要な技術的所見、アーキテクチャ上の決定、コードのリファクタリング、リスク評価を完了したら、ローカルの memory capture を**必ず**起動すること。

1. 最終サマリーまたは成果物をプロジェクトディレクトリに Markdown ファイルとして保存する。
2. キャプチャスクリプトを実行する:
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. これにより、新しい知見・ポリシー・技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動ルーティングされる。