---
name: github-actions-node24
description: Node 20 非推奨警告の解消、Node 24 対応のための GitHub Actions メジャーバージョン更新、関連する input/cache エラーの解決のためのチートシート。
---

# GitHub Actions Node 24 移行ガイド

**重要な指示:** GitHub ワークフローファイル（例: `.github/workflows/` 内）が他のリポジトリ変更と一緒に作成または変更されるたびに、非推奨警告を待たず、下記の Node 24 対応バージョンに合わせて影響を受ける action のメジャーバージョンを**必ず**確認し上げること。

GitHub Actions ランナーが Node 20 非推奨（`Node.js 20 is deprecated... forced to run on Node.js 24`）を報告した場合、サードパーティ action は `setup-node` を入れるだけでは直らない。影響を受ける action の**メジャーバージョンを上げる必要がある**。

## Action バージョンチートシート（Node 24 対応）
> [!IMPORTANT]
> **セキュリティピン留めの制約:** これらの Node 24 対応メジャーへ上げる際、mutable なタグ／ブランチ名（`@v7`、`@v4` など）ではなく、**40 文字のコミット SHA**（例: `uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1`）で action ステップをピン留めしなければならない。mutable タグは自動 SAST 警告（Semgrep など）を起こし、サプライチェーン攻撃に晒される。
>
> タグの最新コミット SHA は `git ls-remote https://github.com/org/repo.git refs/tags/vX*` で確認できる。

Node 20 非推奨警告の解消には次のメジャーバージョンへ上げる:
- `actions/checkout` -> `@v7` (例: `v7.0.1` は `3d3c42e5aac5ba805825da76410c181273ba90b1`)
- `actions/setup-python` -> `@v7` (例: `v7.0.0` は `5fda3b95a4ea91299a34e894583c3862153e4b97`)
- `actions/setup-node` -> `@v7` (例: `v7.1.0` は `949feb2413d6458794dcd2491c4babbbce0c15c1`) または `@v6`
- `actions/setup-java` -> `@v6` (例: `v6.0.1` は `de7274f081f381c8f8158605e0321c36c376e2e6`)
- `actions/cache/restore` -> `@v5`
- `actions/cache/save` -> `@v5`
- `docker/login-action` -> `@v4`
- `docker/setup-qemu-action` -> `@v4`
- `docker/setup-buildx-action` -> `@v4`
- `docker/build-push-action` -> `@v7`
- `docker/metadata-action` -> `@v6` *(注: v7 はまだ存在しない)*
- `gitleaks/gitleaks-action` -> `@v3`
- `softprops/action-gh-release` -> `@v3`

## 未パッチの action（既知）
- `mathieudutour/github-tag-action@v6.1` は現時点でネイティブ Node 24 非対応。この action 関連の警告は害がなく、メンテナが新メジャーを出すまで無視してよい。

## よくあるエラーと注意点

### Gitleaks Action (`Unexpected input(s) 'args'`)
- **エラー:** `Unexpected input(s) 'args', valid inputs are ['']`
- **原因:** GitHub Actions の厳格な入力検証が `gitleaks/gitleaks-action@v3` の `args` を拒否する。
- **解決:** `with: args: ...` ブロックを完全に削除する。action はデフォルトの `detect` を実行しレポートを自動出力する。

### QEMU キャッシュロック (`Unable to reserve cache with key`)
- **エラー:** `Failed to save: Unable to reserve cache with key docker.io--tonistiigi--binfmt-latest-linux-x64, another job may be creating this cache.`
- **原因:** 並行ジョブ（同時 push と PR トリガーなど）が同一 QEMU キャッシュの保存を競合するレースコンディション。
- **解決:** 害のない警告。ビルド失敗やコンテナイメージには影響しない。安全に無視してよい。

## アンチパターン

- 対象のテストスイートで検証せずにコード変更をデプロイしてはならない。
- 自明でないコードの前提・制約・副作用のドキュメント化を省略してはならない。


## Memory Sync

重要な技術的発見、アーキテクチャ上の決定、コードリファクタリング、リスク評価を完了した後は、ローカルのメモリキャプチャを**必ず**起動すること。

1. 最終サマリーまたは成果物をプロジェクトディレクトリに Markdown ファイルとして保存する。
2. キャプチャスクリプトを実行する:
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. これにより、新しい知見・ポリシー・技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動的にルーティングされる。