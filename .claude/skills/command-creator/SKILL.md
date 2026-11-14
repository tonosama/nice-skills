---

name: command-creator

description: |

  AI コーディングエージェントのスラッシュコマンドを作成するときに使う。ユーザーが「create a command」「make a slash command」「add a command」と依頼する、またはワークフローを再利用可能なコマンドとして文書化したいときに使用。適切な構造とベストプラクティスを備えた、エージェントが実行しやすいスラッシュコマンドの作成に不可欠。



---

# Command Creator

このスキルは AI コーディングエージェントのスラッシュコマンド作成を案内する。会話内で `/command-name` として呼び出せる再利用可能なワークフローである。

## About Slash Commands

スラッシュコマンドは `.claude/commands/`（プロジェクト）または `~/.claude/commands/`（グローバル／ユーザー）に置く markdown ファイルで、呼び出し時にプロンプトへ展開される。次に適している:

- 繰り返しワークフロー（code review、PR 提出、CI 修正）
- 一貫性が必要な複数ステップ処理
- エージェント委譲パターン
- プロジェクト固有の自動化

## このスキルを使うとき

ユーザーが次の場合にこのスキルを起動する:

- 「create a command」「make a slash command」と依頼したとき
- 繰り返しワークフローを自動化したいとき
- 再利用のため一貫した手順を文書化したいとき
- 「X を何度もやる、コマンドにできる？」と言ったとき
- プロジェクト固有またはグローバルコマンドを作りたいとき

## Bundled Resources

詳細ガイド用の参照ドキュメント:

- **references/patterns.md** - コマンドパターン（ワークフロー自動化、反復修正、エージェント委譲、単純実行）
- **references/examples.md** - 完全ソース付き実例（submit-stack、ensure-ci、create-implementation-plan）
- **references/best-practices.md** - 品質チェックリスト、よくある落とし穴、執筆ガイド、テンプレート構造

コマンド作成時に必要に応じて参照を読み、パターン・例・品質を確認する。

## Command Structure Overview

各スラッシュコマンドは次の形式の markdown ファイルである:

```markdown

---

**Description:** Brief description shown in /help (required)

argument-hint: <placeholder> (optional, if command takes arguments)

---



# Command Title



[Detailed instructions for the agent to execute autonomously]

```

## Command Creation Workflow

### Step 1: Determine Location

**適切な配置を自動検出:**

1. git リポジトリ状態を確認: `git rev-parse --is-inside-work-tree 2>/dev/null`
2. 既定の配置:
   - git リポジトリ内 → プロジェクト: `.claude/commands/`
   - リポジトリ外 → グローバル: `~/.claude/commands/`
3. ユーザーの上書きを許容:
   - 「global」「user-level」と明示 → `~/.claude/commands/`
   - 「project」「project-level」と明示 → `.claude/commands/`

続行前に選んだ配置をユーザーに報告する。

### Step 2: Show Command Patterns

コマンド種別を理解させる。**references/patterns.md** を読み利用可能なパターンを示す:

- **Workflow Automation** - Analyze → Act → Report（例: submit-stack）
- **Iterative Fixing** - Run → Parse → Fix → Repeat（例: ensure-ci）
- **Agent Delegation** - Context → Delegate → Iterate（例: create-implementation-plan）
- **Simple Execution** - 引数付きコマンド実行（例: codex-review）

ユーザーに「どのパターンが近いか？」と尋ね、会話の枠組みを作る。

### Step 3: Gather Command Information

主要情報をユーザーに尋ねる:

#### A. Command Name and Purpose

- 「コマンド名は？」（ファイル名用）
- 「何をするコマンドか？」（description 用）

ガイドライン:

- コマンド名は **kebab-case 必須**（ハイフン。アンダースコア不可）
  - ✅ 正: `submit-stack`, `ensure-ci`, `create-from-plan`
  - ❌ 誤: `submit_stack`, `ensure_ci`, `create_from_plan`
- ファイル名＝コマンド名: `my-command.md` → `/my-command`
- description は簡潔で動作指向（`/help` に表示）

#### B. Arguments

- 引数はあるか？
- 必須か任意か？
- 引数は何を表すか？

引数がある場合:

- frontmatter に `argument-hint: <placeholder>` を追加
- 必須は `<angle-brackets>`
- 任意は `[square-brackets]`

#### C. Workflow Steps

- 従う具体的ステップは？
- 順序は？
- 使うツールやコマンドは？

次を収集:

- 最初の分析・チェック
- 主なアクション
- 結果の扱い
- 成功基準
- エラーハンドリング方針

#### D. Tool Restrictions and Guidance

- 特定のエージェントやツールを使うべきか？
- 避けるツールや操作は？
- コンテキスト用に読むファイルは？

### Step 4: Generate Optimized Command

エージェント向けに最適化した指示でコマンドファイルを作成。**references/best-practices.md** でテンプレート構造・実行ベストプラクティス・文体・品質チェックリストを確認。

原則:

- 命令形／不定詞（動詞先行）
- 明示的で具体
- 期待される成果を含める
- 具体例を示す
- 明確なエラーハンドリング

### Step 5: Create the Command File

1. 完全パスを決める:
   - プロジェクト: `.claude/commands/[command-name].md`
   - グローバル: `~/.claude/commands/[command-name].md`
2. ディレクトリを用意:

   ```bash
   mkdir -p [directory-path]
   ```

3. Write ツールでファイルを書く
4. ユーザーに確認:
   - ファイルの場所
   - コマンドの概要
   - 使い方: `/command-name [arguments]`

### Step 6: Test and Iterate (Optional)

テストする場合:

1. `You can test this command by running: /command-name [arguments]` を提案
2. フィードバックに応じて反復
3. 必要に応じてファイルを更新

## References & Loading Triggers

- **必須**: プロンプト詳細収集やレイアウト計画の前に [patterns.md](references/patterns.md) と [best-practices.md](references/best-practices.md) を**全文**読み、書式とロジックを揃える。
- 構造テンプレートを写すときは [examples.md](references/examples.md) を読む。
- 自明な標準ワークフローの場合は**外部参照を読み込まない**。

## Anti-Patterns

| 避けること | 理由 | 代わりに |
|-------|-----|---------|
| すべてを root に置く | 肥大化・保守困難 | リンクで分割 |
| カテゴリ過多 | 断片化 | 関連トピックを統合 |
| 曖昧な指示 | トークン浪費・価値低 | 具体化するか削除 |
| 既定の重複 | エージェントは既知 | 必要なときだけ上書き |
| 深いネスト | ナビゲーション困難 | フラット＋リンク |
| System Rules の重複 | コンテキスト肥大・競合 | カスタムワークフローのみ定義 |
| underscore_names | 一貫性破壊 | 常に kebab-case（`my-command.md`）|

## Summary

コマンド作成時:

1. **配置を検出**（プロジェクト vs グローバル）
2. **パターンを示す**
3. **情報収集**（名前、目的、引数、手順、ツール）
4. **最適化コマンドを生成**
5. **ファイル作成**
6. **確認と反復**

明確なステップ、明示的なツール利用、適切なエラーハンドリングで、エージェントが自律実行できるコマンドを作ることに集中する。