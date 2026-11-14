---
name: git-advanced-workflows
description: リベース、チェリーピック、bisect、worktree、reflogを含む高度なGitワークフローをマスターし、クリーンな履歴を維持してあらゆる状況から回復する。複雑なGit履歴の管理、フィーチャーブランチでのコラボレーション、またはリポジトリ問題のトラブルシューティング時に使用する。
---

# Git高度なワークフロー

クリーンな履歴を維持し、効果的にコラボレーションし、あらゆる状況から自信を持って回復するための高度なGitテクニックをマスターする。

## このスキルを使う場面

- マージ前のコミット履歴のクリーンアップ
- ブランチをまたいだ特定のコミットの適用
- バグを導入したコミットの特定
- 複数の機能の同時作業
- Gitのミスや失われたコミットからの回復
- 複雑なブランチワークフローの管理
- レビュー用のクリーンなPRの準備
- 分岐したブランチの同期

## コアコンセプト

### 1. インタラクティブリベース

インタラクティブリベースはGit履歴編集のスイスアーミーナイフ。

**主な操作:**

- `pick`: コミットをそのまま保持する
- `reword`: コミットメッセージを変更する
- `edit`: コミット内容を修正する
- `squash`: 前のコミットと結合する
- `fixup`: squashと同様だがメッセージを破棄する
- `drop`: コミットを完全に削除する

**基本的な使い方:**

```bash
# 最後の5コミットをリベース
git rebase -i HEAD~5

# 現在のブランチのすべてのコミットをリベース
git rebase -i $(git merge-base HEAD main)

# 特定のコミットへリベース
git rebase -i abc123
```

### 2. チェリーピック

ブランチ全体をマージせずに、特定のコミットを1つのブランチから別のブランチに適用する。

```bash
# 単一コミットのチェリーピック
git cherry-pick abc123

# コミット範囲のチェリーピック（排他的な開始点）
git cherry-pick abc123..def456

# コミットせずにチェリーピック（変更をステージングのみ）
git cherry-pick -n abc123

# コミットメッセージを編集してチェリーピック
git cherry-pick -e abc123
```

### 3. Git Bisect

コミット履歴の二分探索でバグを導入したコミットを特定する。

```bash
# bisectを開始する
git bisect start

# 現在のコミットを不良とマークする
git bisect bad

# 既知の良好なコミットをマークする
git bisect good v1.0.0

# GitはMiddleのコミットをチェックアウトする - テストする
# 次に良好または不良とマークする
git bisect good  # または: git bisect bad

# バグが見つかるまで続ける
# 完了したら
git bisect reset
```

**自動化されたBisect:**

```bash
# スクリプトを使って自動テスト
git bisect start HEAD v1.0.0
git bisect run ./test.sh

# test.shは良好なら0を、不良なら1-127（125を除く）を返すべき
```

   100|### 4. Worktree

スタッシュや切り替えなしに複数のブランチを同時に作業する。

```bash
# 既存のworktreeをリストアップ
git worktree list

# フィーチャーブランチの新しいworktreeを追加する
git worktree add ../project-feature feature/new-feature
   110|
# 新しいブランチを作成してworktreeを追加する
git worktree add -b bugfix/urgent ../project-hotfix main

# worktreeを削除する
git worktree remove ../project-feature

# 古くなったworktreeをプルーニングする
git worktree prune
```
   120|
### 5. Reflog

安全網 - 削除されたコミットを含むすべてのref移動を追跡する。

```bash
# reflogを表示する
git reflog

# 特定のブランチのreflogを表示する
   130|git reflog show feature/branch

# 削除されたコミットを復元する
git reflog
# コミットハッシュを見つける
git checkout abc123
git branch recovered-branch

# 削除されたブランチを復元する
git reflog
   140|git branch deleted-branch abc123
```

## 詳細なパターンと実例

詳細なパターンのドキュメントは `references/details.md` にあります。上記のナビゲーション階層では不十分な場合は、そのファイルを参照してください。

## ベストプラクティス

1. **常に --force-with-lease を使用する**: --forceより安全で、他者の作業を上書きすることを防ぐ
   150|2. **ローカルコミットのみリベースする**: プッシュして共有されたコミットはリベースしない
3. **説明的なコミットメッセージ**: 将来の自分が現在の自分に感謝する
4. **アトミックなコミット**: 各コミットは単一の論理的な変更であるべき
5. **フォースプッシュ前にテストする**: 履歴の書き換えが何も壊していないことを確認する
6. **Reflogを意識する**: Reflogは安全網だが、現在のtipから到達できないコミットのエントリはデフォルトで30日後に期限切れになる（`gc.reflogExpireUnreachable`）。到達可能なエントリは90日後（`gc.reflogExpire`）
7. **リスクのある操作の前にブランチを作成する**: 複雑なリベースの前にバックアップブランチを作成する

```bash
# 安全なフォースプッシュ
git push --force-with-lease origin feature/branch
   160|
# リスクのある操作の前にバックアップを作成する
git branch backup-branch
git rebase -i main
# 何か問題が起きた場合
git reset --hard backup-branch
```

## よくある落とし穴

   170|- **パブリックブランチのリベース**: コラボレーターに履歴の競合を引き起こす
- **leaseなしのフォースプッシュ**: チームメイトの作業を上書きする可能性がある
- **リベース中の作業紛失**: 競合を慎重に解決し、リベース後にテストする
- **Worktreeのクリーンアップ忘れ**: 孤立したworktreeはディスクスペースを消費する
- **実験前のバックアップなし**: 常に安全用のブランチを作成する
- **汚れたワーキングディレクトリでのBisect**: bisect前にコミットまたはスタッシュする

## 回復コマンド

```bash
   180|# 進行中の操作を中止する
git rebase --abort
git merge --abort
git cherry-pick --abort
git bisect reset

# 特定のコミットのバージョンにファイルを復元する
git restore --source=abc123 path/to/file

# 最後のコミットを元に戻すが変更は保持する
   190|git reset --soft HEAD^

# 最後のコミットを元に戻して変更を破棄する
# 警告: --hardはコミットされていない変更（ステージング済みと未ステージング）も削除する
# reflogはそれらを回復できない。事前に`git status`を確認してスタッシュすること
git reset --hard HEAD^

# 削除されたブランチをすぐに回復する。デフォルトでは、git gcは各エントリが記録されてから
# 30日後にHEADのreflogエントリをプルーニングできる。ブランチを削除するとその独自のreflogも削除される。
# HEADのreflogを検索する
   200|git reflog
git branch recovered-branch abc123
```

## 実践的ワークフロー

### ワークフロー 1: Clean Up Feature Branch Before PR

```bash
# Start with feature branch
git checkout feature/user-auth

# Interactive rebase to clean history
git rebase -i main

# Example rebase operations:
# - Squash "fix typo" commits
# - Reword commit messages for clarity
# - Reorder commits logically
# - Drop unnecessary commits

# Force push cleaned branch (safe if no one else is using it)
git push --force-with-lease origin feature/user-auth
```

### ワークフロー 2: Apply Hotfix to Multiple Releases

```bash
# Create fix on main
git checkout main
git commit -m "fix: critical security patch"

# Apply to release branches
git checkout release/2.0
git cherry-pick abc123

git checkout release/1.9
git cherry-pick abc123

# Handle conflicts if they arise
git cherry-pick --continue
# or
git cherry-pick --abort
```

### ワークフロー 3: Find Bug Introduction

```bash
# Start bisect
git bisect start
git bisect bad HEAD
git bisect good v2.1.0

# Git checks out middle commit - run tests
npm test

# If tests fail
git bisect bad

# If tests pass
git bisect good

# Git will automatically checkout next commit to test
# Repeat until bug found

# Automated version
git bisect start HEAD v2.1.0
git bisect run npm test
```

### ワークフロー 4: Multi-Branch Development

```bash
# Main project directory
cd ~/projects/myapp

# Create worktree for urgent bugfix
git worktree add ../myapp-hotfix hotfix/critical-bug

# Work on hotfix in separate directory
cd ../myapp-hotfix
# Make changes, commit
git commit -m "fix: resolve critical bug"
git push origin hotfix/critical-bug

# Return to main work without interruption
cd ~/projects/myapp
git fetch origin
git cherry-pick hotfix/critical-bug

# Clean up when done
git worktree remove ../myapp-hotfix
```

### ワークフロー 5: Recover from Mistakes

```bash
# Accidentally reset to wrong commit
git reset --hard HEAD~5  # Oh no!

# Use reflog to find lost commits
git reflog
# Output shows:
# abc123 HEAD@{0}: reset: moving to HEAD~5
# def456 HEAD@{1}: commit: my important changes

# Recover lost commits
git reset --hard def456

# Or create branch from lost commit
git branch recovery def456
```

## Advanced Techniques

### Rebase vs Merge Strategy

**When to Rebase:**
- Cleaning up local commits before pushing
- Keeping feature branch up-to-date with main
- Creating linear history for easier review

**When to Merge:**
- Integrating completed features into main
- Preserving exact history of collaboration
- Public branches used by others

```bash
# Update feature branch with main changes (rebase)
git checkout feature/my-feature
git fetch origin
git rebase origin/main

# Handle conflicts
git status
# Fix conflicts in files
git add .
git rebase --continue

# Or merge instead
git merge origin/main
```

### Autosquash Workflow

Automatically squash fixup commits during rebase.

```bash
# Make initial commit
git commit -m "feat: add user authentication"

# Later, fix something in that commit
git commit --fixup HEAD  # or specify commit hash

# Make more changes
git commit --fixup abc123

# Rebase with autosquash
git rebase -i --autosquash main

# Git automatically marks fixup commits
```

### Split Commit

Break one commit into multiple logical commits.

```bash
# Start interactive rebase
git rebase -i HEAD~3

# Mark commit to split with 'edit'
# Git will stop at that commit

# Reset commit but keep changes
git reset HEAD^

# Stage and commit in logical chunks
git add file1.py
git commit -m "feat: add validation"

git add file2.py
git commit -m "feat: add error handling"

# Continue rebase
git rebase --continue
```

### Partial Cherry-Pick

Cherry-pick only specific files from a commit.

```bash
# Show files in commit
git show --name-only abc123

# Checkout specific files from commit
git checkout abc123 -- path/to/file1.py path/to/file2.py

# Stage and commit
git commit -m "cherry-pick: apply specific changes from abc123"
```

## Best Practices

1. **Always Use --force-with-lease**: Safer than --force, prevents overwriting others' work
2. **Rebase Only Local Commits**: Don't rebase commits that have been pushed and shared
3. **Descriptive Commit Messages**: Future you will thank present you
4. **Atomic Commits**: Each commit should be a single logical change
5. **Test Before Force Push**: Ensure history rewrite didn't break anything
6. **Keep Reflog Aware**: Remember reflog is your safety net for 90 days
7. **Branch Before Risky Operations**: Create backup branch before complex rebases

```bash
# Safe force push
git push --force-with-lease origin feature/branch

# Create backup before risky operation
git branch backup-branch
git rebase -i main
# If something goes wrong
git reset --hard backup-branch
```

## Common Pitfalls

- **Rebasing Public Branches**: Causes history conflicts for collaborators
- **Force Pushing Without Lease**: Can overwrite teammate's work
- **Losing Work in Rebase**: Resolve conflicts carefully, test after rebase
- **Forgetting Worktree Cleanup**: Orphaned worktrees consume disk space
- **Not Backing Up Before Experiment**: Always create safety branch
- **Bisecting on Dirty Working Directory**: Commit or stash before bisecting

## Recovery Commands

```bash
# Abort operations in progress
git rebase --abort
git merge --abort
git cherry-pick --abort
git bisect reset

# Restore file to version from specific commit
git restore --source=abc123 path/to/file

# Undo last commit but keep changes
git reset --soft HEAD^

# Undo last commit and discard changes
git reset --hard HEAD^

# Recover deleted branch (within 90 days)
git reflog
git branch recovered-branch abc123
```

## リソース

- **references/git-rebase-guide.md**: Deep dive into interactive rebase
- **references/git-conflict-resolution.md**: Advanced conflict resolution strategies
- **references/git-history-rewriting.md**: Safely rewriting Git history
- **assets/git-workflow-checklist.md**: Pre-PR cleanup checklist
- **assets/git-aliases.md**: Useful Git aliases for advanced workflows
- **scripts/git-clean-branches.sh**: Clean up merged and stale branches

## アンチパターン

- 対象の test suite で検証せずにコード変更をデプロイしてはならない。
- 自明でないコードの前提、制約、副作用の文書化を省略してはならない。


## Memory Sync

主要な技術的知見、アーキテクチャ上の判断、コードのリファクタリング、リスク評価を完了したら、ローカルの memory capture を**必ず**起動する。

1. 最終サマリまたは成果物をプロジェクトディレクトリ内の Markdown ファイルとして保存する。
2. キャプチャスクリプトを実行する:
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. 新しい学び、ポリシー、技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動ルーティングされる。
