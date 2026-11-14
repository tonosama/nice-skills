---
name: github-actions-templates
description: 自動テスト・ビルド・デプロイ向けの本番運用可能な GitHub Actions ワークフローを作成する。GitHub Actions で CI/CD を構築する、開発ワークフローを自動化する、再利用可能なワークフローテンプレートを作る際に使用する。
---

# GitHub Actions Templates

テスト・ビルド・デプロイ向けの本番運用可能な GitHub Actions ワークフローパターン。

## このスキルを使わないとき

- タスクが github actions templates と無関係な場合
- 本スコープ外の別ドメインやツールが必要な場合

## 手順

- 目的、制約、必要な入力を明確にする。
- 関連するベストプラクティスを適用し、成果を検証する。
- 実行可能なステップと検証方法を提供する。

## 目的

多様な技術スタック向けに、継続的インテグレーションとデプロイのための効率的で安全な GitHub Actions ワークフローを作る。

## このスキルを使うとき

- テストとデプロイを自動化する
- Docker イメージをビルドしてレジストリへ push する
- Kubernetes クラスターへデプロイする
- セキュリティスキャンを実行する
- 複数環境向け matrix ビルドを実装する

## 一般的なワークフローパターン

### パターン 1: テストワークフロー

```yaml
name: Test

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
    - uses: actions/checkout@v4

    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Run linter
      run: npm run lint

    - name: Run tests
      run: npm test

    - name: Upload coverage
      uses: codecov/codecov-action@v3
      with:
        files: ./coverage/lcov.info
```


### パターン 2: Docker イメージのビルドと push

```yaml
name: Build and Push

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
    - uses: actions/checkout@v4

    - name: Log in to Container Registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
        tags: |
          type=ref,event=branch
          type=ref,event=pr
          type=semver,pattern={{version}}
          type=semver,pattern={{major}}.{{minor}}

    - name: Build and push
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
```


### パターン 3: Kubernetes へのデプロイ

```yaml
name: Deploy to Kubernetes

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2

    - name: Update kubeconfig
      run: |
        aws eks update-kubeconfig --name production-cluster --region us-west-2

    - name: Deploy to Kubernetes
      run: |
        kubectl apply -f k8s/
        kubectl rollout status deployment/my-app -n production
        kubectl get services -n production

    - name: Verify deployment
      run: |
        kubectl get pods -n production
        kubectl describe deployment my-app -n production
```

### パターン 4: Matrix ビルド

```yaml
name: Matrix Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        python-version: ['3.9', '3.10', '3.11', '3.12']

    steps:
    - uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Run tests
      run: pytest
```

## ワークフローのベストプラクティス

1. **action のバージョンを固定する**（@latest ではなく @v4）
2. **依存関係をキャッシュ**してビルドを高速化
3. **secrets** で機密データを扱う
4. PR に**ステータスチェック**を実装
5. 複数バージョンテストに **matrix ビルド**を使う
6. **適切な permissions** を設定
7. 共通パターンに**再利用ワークフロー**を使う
8. 本番に**承認ゲート**を実装
9. 失敗時の**通知ステップ**を追加
10. 機密ワークロードに**セルフホスト runner**を使う

## 再利用ワークフロー

```yaml
# .github/workflows/reusable-test.yml
name: Reusable Test Workflow

on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
    secrets:
      NPM_TOKEN:
        required: true

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}
    - run: npm ci
    - run: npm test
```

**再利用ワークフローの呼び出し：**

```yaml
jobs:
  call-test:
    uses: ./.github/workflows/reusable-test.yml
    with:
      node-version: '20.x'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## セキュリティスキャン

```yaml
name: Security Scan

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4

    - name: Run Trivy vulnerability scanner
      uses: aquasecurity/trivy-action@master
      with:
        scan-type: 'fs'
        scan-ref: '.'
        format: 'sarif'
        output: 'trivy-results.sarif'

    - name: Upload Trivy results to GitHub Security
      uses: github/codeql-action/upload-sarif@v2
      with:
        sarif_file: 'trivy-results.sarif'

    - name: Run Snyk Security Scan
      uses: snyk/actions/node@master
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
```

## 承認付きデプロイ

```yaml
name: Deploy to Production

on:
  push:
    tags: [ 'v*' ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://app.example.com

    steps:
    - uses: actions/checkout@v4

    - name: Deploy application
      run: |
        echo "Deploying to production..."
        # Deployment commands here
        
    - name: Notify Slack
      if: success()
      uses: slackapi/slack-github-action@v1
      with:
        webhook-url: ${{ secrets.SLACK_WEBHOOK }}
        payload: |
          {
            "text": "Deployment to production completed successfully!"
          }
```

## 関連スキル

- `gitlab-ci-patterns` — GitLab CI ワークフロー向け
- `deployment-pipeline-design` — パイプラインアーキテクチャ向け
- `secrets-management` — Secret 管理向け

## アンチパターン

- 対象のテストスイートで検証せずにコード変更をデプロイしてはならない。
- 自明でないコードの前提・制約・副作用のドキュメントを省略してはならない。


## Memory Sync

重要な技術的所見、アーキテクチャ上の決定、コードリファクタリング、リスク評価を完了したあと、ローカルの memory capture を**必ず**起動する。

1. 最終サマリーまたは成果物をプロジェクトディレクトリ内の Markdown ファイルとして保存する。
2. capture スクリプトを実行する：
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. これにより、新しい学び、ポリシー、技術スニペットが正しいローカルストレージ（OKF または ChromaDB）へ自動的にルーティングされる。
