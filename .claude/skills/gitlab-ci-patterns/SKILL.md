---
name: gitlab-ci-patterns
description: スケーラブルな自動化のためのマルチステージワークフロー、キャッシュ、分散ランナーを含むGitLab CI/CDパイプラインを構築する。GitLab CI/CDの実装、パイプラインパフォーマンスの最適化、または自動テストとデプロイメントのセットアップ時に使用する。
---

# GitLab CI パターン

自動テスト、ビルド、デプロイメントのための包括的なGitLab CI/CDパイプラインパターン。

## 目的

適切なステージ構成、キャッシュ、デプロイメント戦略を持つ効率的なGitLab CIパイプラインを作成する。

## 使用タイミング

- GitLabベースのCI/CDの自動化
- マルチステージパイプラインの実装
- GitLab Runnersの設定
- GitLabからKubernetesへのデプロイ
- GitOpsワークフローの実装

## 基本パイプライン構造

```yaml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: "/certs"

build:
  stage: build
  image: node:20
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 hour
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/

test:
  stage: test
  image: node:20
  script:
    - npm ci
    - npm run lint
    - npm test
  coverage: '/Lines\s*:\s*(\d+\.\d+)%/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

deploy:
  stage: deploy
  image: bitnami/kubectl:1.31
  script:
    - kubectl apply -f k8s/
    - kubectl rollout status deployment/my-app
  only:
    - main
  environment:
    name: production
    url: https://app.example.com
```

## Dockerビルドとプッシュ

```yaml
build-docker:
  stage: build
  image: docker:24
  services:
    - docker:24-dind
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker build -t $CI_REGISTRY_IMAGE:latest .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker push $CI_REGISTRY_IMAGE:latest
  only:
    - main
    - tags
```

## マルチ環境デプロイメント

`KUBE_CA_CERT_FILE` にクラスターCA証明書を含むGitLabファイル変数として設定し、
保護されマスクされたCI変数を通じて `KUBE_TOKEN` を提供する。ファイル変数には
kubectlがAPIサーバー証明書を検証するために使用するパスが含まれる。デプロイメント
ジョブが保護されたトークンを読み取れるよう、`develop` と `main` の両方を保護する。

```yaml
.deploy_template: &deploy_template
  image: bitnami/kubectl:1.31
  before_script:
    - kubectl config set-cluster k8s --server="$KUBE_URL" --certificate-authority="$KUBE_CA_CERT_FILE" --embed-certs=true
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default

deploy:staging:
  <<: *deploy_template
  stage: deploy
  script:
    - kubectl apply -f k8s/ -n staging
    - kubectl rollout status deployment/my-app -n staging
  environment:
    name: staging
    url: https://staging.example.com
  only:
    - develop

deploy:production:
  <<: *deploy_template
  stage: deploy
  script:
    - kubectl apply -f k8s/ -n production
    - kubectl rollout status deployment/my-app -n production
  environment:
    name: production
    url: https://app.example.com
  when: manual
  only:
    - main
```

## Terraformパイプライン

```yaml
stages:
  - validate
  - plan
  - apply

variables:
  TF_ROOT: ${CI_PROJECT_DIR}/terraform
  TF_VERSION: "1.6.0"

before_script:
  - cd ${TF_ROOT}
  - terraform --version

validate:
  stage: validate
  image: hashicorp/terraform:${TF_VERSION}
  script:
    - terraform init -backend=false
    - terraform validate
    - terraform fmt -check

plan:
  stage: plan
  image: hashicorp/terraform:${TF_VERSION}
  script:
    - terraform init
    - terraform plan -out=tfplan
  artifacts:
    paths:
      - ${TF_ROOT}/tfplan
    expire_in: 1 day

apply:
  stage: apply
  image: hashicorp/terraform:${TF_VERSION}
  script:
    - terraform init
    - terraform apply -auto-approve tfplan
  dependencies:
    - plan
  when: manual
  only:
    - main
```

## セキュリティスキャン

```yaml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Container-Scanning.gitlab-ci.yml

trivy-scan:
  stage: test
  image: aquasec/trivy:0.58.0
  script:
    - trivy image --exit-code 1 --severity HIGH,CRITICAL $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  allow_failure: true
```

## キャッシュ戦略

```yaml
# node_modulesをキャッシュ
build:
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - node_modules/
    policy: pull-push

# グローバルキャッシュ
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - .cache/
    - vendor/

# ジョブ単位のキャッシュ
job1:
  cache:
    key: job1-cache
    paths:
      - build/

job2:
  cache:
    key: job2-cache
    paths:
      - dist/
```

## 動的チャイルドパイプライン

```yaml
generate-pipeline:
  stage: build
  script:
    - python generate_pipeline.py > child-pipeline.yml
  artifacts:
    paths:
      - child-pipeline.yml

trigger-child:
  stage: deploy
  trigger:
    include:
      - artifact: child-pipeline.yml
        job: generate-pipeline
    strategy: depend
```


## ベストプラクティス

1. **特定のイメージタグを使用する**（node:20、node:latestではなく）
2. **依存関係を適切にキャッシュする**
3. **ビルド出力にアーティファクトを使用する**
4. **本番環境に手動ゲートを実装する**
5. **デプロイメント追跡に環境を使用する**
6. **マージリクエストパイプラインを有効にする**
7. **定期的なジョブにパイプラインスケジュールを使用する**
8. **セキュリティスキャンを実装する**
9. **シークレットにCI/CD変数を使用する**
10. **パイプラインパフォーマンスを監視する**

## 関連スキル

- `github-actions-templates` - GitHub Actions
- `deployment-pipeline-design` - アーキテクチャ
- `secrets-management` - シークレット処理

## このスキルを使わないとき

- タスクが GitLab CI パターンと無関係なとき
- このスコープ外の別ドメインやツールが必要なとき

## 手順

- 目標、制約、必要な入力を明確にする。
- 関連するベストプラクティスを適用し、成果を検証する。
- 実行可能なステップと検証方法を提供する。

## アンチパターン

- 対象のテストスイートで検証せずにコード変更をデプロイしてはならない。
- 自明でないコードの前提、制約、副作用の文書化を省略してはならない。

## Memory Sync

主要な技術的所見、アーキテクチャ上の決定、コードリファクタリング、リスク評価を完了したら、ローカルのメモリキャプチャを**必ず**起動する。

1. 最終サマリーまたは成果物をプロジェクトディレクトリ内の Markdown ファイルとして保存する。
2. キャプチャスクリプトを起動する:
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. これにより、新しい学び、ポリシー、技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動的にルーティングされる。
