---
name: git-pr-workflows-git-workflow
description: "コードレビューから PR 作成までの包括的な git ワークフローを編成し、品質保証・テスト・デプロイ準備の専門エージェントを活用する。Conventional Commits、自動テスト、構造化 PR 作成など現代的 git ベストプラクティスを実装する。"
---

# マルチエージェント編成による完全 Git ワークフロー

コードレビューから PR 作成までの包括的な git ワークフローを編成し、品質保証・テスト・デプロイ準備の専門エージェントを活用する。Conventional Commits、自動テスト、構造化 PR 作成など現代的 git ベストプラクティスを実装する。

[拡張思考: 本ワークフローは複数の専門エージェントを調整し、コミット前にコード品質を担保する。code-reviewer が初期品質チェック、test-automator が全テスト通過、deployment-engineer が本番準備を検証する。コンテキストを引き継ぎながら順次エージェントを実行することで、破損コードの混入を防ぎつつ高い開発速度を維持する。trunk-based および feature-branch の両戦略に対応し、チーム需要に応じてオプションを設定できる。]

## このスキルを使うとき

- マルチエージェント編成による完全 git ワークフローのタスクや作業を行うとき
- 完全 git ワークフローに関するガイダンス、ベストプラクティス、チェックリストが必要なとき

## このスキルを使わないとき

- 完全 git ワークフローと無関係なタスクのとき
- 本スコープ外の別ドメインやツールが必要なとき

## 手順

- 目標、制約、必要な入力を明確化する。
- 関連するベストプラクティスを適用し、成果を検証する。
- 実行可能なステップと検証方法を提示する。

## 設定

**ターゲットブランチ**: $ARGUMENTS（未指定時は `main`）

**サポートフラグ**:
- `--skip-tests`: 自動テスト実行をスキップ（慎重に使用）
- `--draft-pr`: 作業中 PR を draft として作成
- `--no-push`: 全チェックは行うがリモートへ push しない
- `--squash`: push 前にコミットを squash
- `--conventional`: Conventional Commits 形式を厳格に強制
- `--trunk-based`: trunk-based 開発ワークフローを使用
- `--feature-branch`: feature branch ワークフローを使用（デフォルト）

## フェーズ 1: コミット前レビューと分析

### 0. リポジトリ Origin と Fork ガードレール事前確認
- 事前メモリ取得と fork の origin 検証を実行:
  ```bash
  python3 ~/memory_system/inspect_memory.py --preflight .
  ```
- **アクティブ Fork ゲート**: 対象リポジトリが fork か検証する。
  - アクティブ fork の場合: **fork のデフォルトブランチへ一方的な PR を出さない。** upstream から同期（`gh repo sync`）するか、pull request のターゲットを upstream 親リポジトリに設定する。
  - スタンドアロンまたは放棄済み採用 fork と確認済みの場合: 標準ワークフローに進む。

### 1. コード品質評価
- Task ツールで subagent_type="code-reviewer"
- プロンプト: "Review all uncommitted changes for code quality issues. Check for: 1) Code style violations, 2) Security vulnerabilities, 3) Performance concerns, 4) Missing error handling, 5) Incomplete implementations. Generate a detailed report with severity levels (critical/high/medium/low) and provide specific line-by-line feedback. Output format: JSON with {issues: [], summary: {critical: 0, high: 0, medium: 0, low: 0}, recommendations: []}"
- 期待出力: 次フェーズ用の構造化コードレビューレポート

### 2. 依存関係と破壊的変更の分析
- Task ツールで subagent_type="code-reviewer"
- プロンプト: "Analyze the changes for: 1) New dependencies or version changes, 2) Breaking API changes, 3) Database schema modifications, 4) Configuration changes, 5) Backward compatibility issues. Context from previous review: [insert issues summary]. Identify any changes that require migration scripts or documentation updates."
- 前フェーズのコンテキスト: 破壊的変更を示しうるコード品質問題
- 期待出力: 破壊的変更評価と移行要件

## フェーズ 2: テストと検証

### 1. テスト実行とカバレッジ
- Task ツールで subagent_type="unit-testing::test-automator"
- プロンプト: "Execute all test suites for the modified code. Run: 1) Unit tests, 2) Integration tests, 3) End-to-end tests if applicable. Generate coverage report and identify any untested code paths. Based on review issues: [insert critical/high issues], ensure tests cover the problem areas. Provide test results in format: {passed: [], failed: [], skipped: [], coverage: {statements: %, branches: %, functions: %, lines: %}, untested_critical_paths: []}"
- 前フェーズのコンテキスト: テストでカバーすべき重大コードレビュー問題
- 期待出力: 完全なテスト結果とカバレッジ指標

### 2. テスト推奨とギャップ分析
- Task ツールで subagent_type="unit-testing::test-automator"
- プロンプト: "Based on test results [insert summary] and code changes, identify: 1) Missing test scenarios, 2) Edge cases not covered, 3) Integration points needing verification, 4) Performance benchmarks needed. Generate test implementation recommendations prioritized by risk. Consider the breaking changes identified: [insert breaking changes]."
- 前フェーズのコンテキスト: テスト結果、破壊的変更、未テストパス
- 期待出力: 優先順位付きの追加テスト一覧

## フェーズ 3: コミットメッセージ生成

### 1. 変更分析と分類
- Task ツールで subagent_type="code-reviewer"
- プロンプト: "Analyze all changes and categorize them according to Conventional Commits specification. Identify the primary change type (feat/fix/docs/style/refactor/perf/test/build/ci/chore/revert) and scope. For changes: [insert file list and summary], determine if this should be a single commit or multiple atomic commits. Consider test results: [insert test summary]."
- 前フェーズのコンテキスト: テスト結果、コードレビュー要約
- 期待出力: コミット構造の推奨

### 2. Conventional Commit メッセージ作成
- Task ツールで subagent_type="llm-application-dev::prompt-engineer"
- プロンプト: "Create Conventional Commits format message(s) based on categorization: [insert categorization]. Format: <type>(<scope>): <subject> with blank line then <body> explaining what and why (not how), then <footer> with BREAKING CHANGE: if applicable. Include: 1) Clear subject line (50 chars max), 2) Detailed body explaining rationale, 3) References to issues/tickets, 4) Co-authors if applicable. Consider the impact: [insert breaking changes if any]."
- 前フェーズのコンテキスト: 変更分類、破壊的変更
- 期待出力: 適切な形式のコミットメッセージ

## フェーズ 4: ブランチ戦略と push 準備

### 1. ブランチ管理
- Task ツールで subagent_type="cicd-automation::deployment-engineer"
- プロンプト: "Based on workflow type [--trunk-based or --feature-branch], prepare branch strategy. For feature branch: ensure branch name follows pattern (feature|bugfix|hotfix)/<ticket-id>-<description>. For trunk-based: prepare for direct main push with feature flag strategy if needed. Current branch: [insert branch], target: [insert target branch]. Verify no conflicts with target branch."
- 期待出力: ブランチ準備コマンドとコンフリクト状態

### 2. push 前検証
- Task ツールで subagent_type="cicd-automation::deployment-engineer"
- プロンプト: "Perform final pre-push checks: 1) Verify all CI checks will pass, 2) Confirm no sensitive data in commits, 3) Validate commit signatures if required, 4) Check branch protection rules, 5) Ensure all review comments addressed. Test summary: [insert test results]. Review status: [insert review summary]."
- 前フェーズのコンテキスト: 全検証結果
- 期待出力: push 準備完了またはブロック要因

## フェーズ 5: Pull Request 作成

### 1. PR 説明の生成
- Task ツールで subagent_type="documentation-generation::docs-architect"
- プロンプト: "Create comprehensive PR description including: 1) Summary of changes (what and why), 2) Type of change checklist, 3) Testing performed summary from [insert test results], 4) Screenshots/recordings if UI changes, 5) Deployment notes from [insert deployment considerations], 6) Related issues/tickets, 7) Breaking changes section if applicable: [insert breaking changes], 8) Reviewer checklist. Format as GitHub-flavored Markdown."
- 前フェーズのコンテキスト: 全検証結果、テスト結果、破壊的変更
- 期待出力: Markdown 形式の完全な PR 説明

### 2. PR メタデータと自動化セットアップ
- Task ツールで subagent_type="cicd-automation::deployment-engineer"
- プロンプト: "Configure PR metadata: 1) Assign appropriate reviewers based on CODEOWNERS, 2) Add labels (type, priority, component), 3) Link related issues, 4) Set milestone if applicable, 5) Configure merge strategy (squash/merge/rebase), 6) Set up auto-merge if all checks pass. Consider draft status: [--draft-pr flag]. Include test status: [insert test summary]."
- 前フェーズのコンテキスト: PR 説明、テスト結果、レビュー状態
- 期待出力: PR 設定コマンドと自動化ルール

## 成功基準

- ✅ critical および high のコード問題がすべて解消
- ✅ テストカバレッジが維持または改善（目標: >80%）
- ✅ 全テスト通過（unit、integration、e2e）
- ✅ コミットメッセージが Conventional Commits 形式に準拠
- ✅ ターゲットブランチとのマージコンフリクトなし
- ✅ PR 説明に必須セクションがすべて含まれる
- ✅ ブランチ保護ルールを満たす
- ✅ セキュリティスキャン完了、critical 脆弱性なし
- ✅ パフォーマンスベンチマークが許容閾値内
- ✅ API 変更がある場合はドキュメント更新済み

## ロールバック手順

マージ後に問題が発生した場合:

1. **即時 revert**: `git revert <commit-hash>` で revert PR を作成
2. **Feature Flag 無効化**: feature flag 使用時は即座に無効化
3. **Hotfix ブランチ**: 重大問題は main から hotfix ブランチを作成
4. **連絡**: 指定チャネルでチームに通知
5. **根本原因分析**: ポストモーテムテンプレートに事象を記録

## ベストプラクティス参照

- **コミット頻度**: 早く頻繁にコミットするが、各コミットは原子的であること
- **ブランチ命名**: `(feature|bugfix|hotfix|docs|chore)/<ticket-id>-<brief-description>`
- **PR サイズ**: 効果的なレビューのため 400 行未満を維持
- **レビュー対応**: 24 時間以内にレビューコメントに対応
- **マージ戦略**: feature branch は squash、release branch は merge
- **サインオフ**: main 変更には最低 2 件の承認を要求

### Dependabot と依存関係アラート管理

- **Dependabot 有効化**: `.github/dependabot.yml` で Dependabot のバージョン更新とセキュリティ更新を設定。各 package-ecosystem に必須の `cooldown: default-days: 7` を含める。コンパイルツールチェーンがアップグレードを制限する場合、コンテナベースイメージ（例: `eclipse-temurin` の `semver-major`）のメジャーバージョン更新は無視する。
- **アラート管理**: GitHub CLI でアクティブアラートを定期確認。解消または緩和済みアラートは次でプログラム的に dismiss:
  ```bash
  gh api -X PATCH repos/<owner>/<repo>/dependabot/alerts/<alert_number> \
    -f state="dismissed" \
    -f dismissed_reason="tolerable_risk" \
    -f dismissed_comment="Resolved by dependency updates/constraints in default branch"
  ```



## アンチパターン

- NEVER deploy code changes without validating them against target test suites.
- NEVER skip documenting non-obvious code assumptions, constraints, and side effects.


## Memory Sync

主要な技術的発見、アーキテクチャ決定、コードリファクタリング、リスク評価の完了後、**必ず**ローカルメモリキャプチャを起動する。

1. 最終サマリーまたは成果物をプロジェクトディレクトリに Markdown ファイルとして保存する。
2. キャプチャスクリプトを呼び出す:
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. これにより新しい学習内容、ポリシー、技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動ルーティングされる。