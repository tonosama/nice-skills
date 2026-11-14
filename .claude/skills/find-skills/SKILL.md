---
name: find-skills
description: ユーザーが「X のやり方は？」「X 用のスキルを探して」「…できるスキルはある？」のように尋ねたり、能力拡張に関心を示したときに、エージェントスキルの発見とインストールを支援する。インストール可能なスキルとして存在しそうな機能を探しているときに使う。
---

# Find Skills

このスキルは、オープンなエージェントスキルエコシステムからスキルを発見しインストールするのに役立つ。

## このスキルを使うとき

次のようなときに使う：

- 既存スキルがありそうな一般的タスクについて「X のやり方は？」と聞かれた
- 「X 用のスキルを探して」「X 用のスキルはある？」と言われた
- 専門的能力について「X できる？」と聞かれた
- エージェントの能力拡張に関心が示された
- ツール、テンプレート、ワークフローを検索したい
- 特定領域（デザイン、テスト、デプロイなど）の支援が欲しいと述べた

## Skills CLI とは

Skills CLI（`npx skills`）はオープンなエージェントスキルエコシステムのパッケージマネージャである。スキルは専門知識、ワークフロー、ツールでエージェント能力を拡張するモジュールパッケージである。

**主要コマンド：**

- `npx skills find [query] [--owner <owner>]` — 対話またはキーワードでスキルを検索。任意で GitHub owner にスコープ
- `npx skills add <package>` — GitHub などからスキルをインストール
- `npx skills update` — インストール済みスキルをすべて更新

**スキル閲覧：** https://skills.sh/

## ユーザーがスキルを見つける手助け

### ステップ 1: 必要なものを理解する

ユーザーが何かの手伝いを求めたら、次を特定する：

1. ドメイン（例：React、テスト、デザイン、デプロイ）
2. 具体的タスク（例：テスト作成、アニメーション、PR レビュー）
3. スキルが存在しそうなほど一般的なタスクかどうか

### ステップ 2: まずリーダーボードを確認

CLI 検索の前に [skills.sh リーダーボード](https://skills.sh/) で、そのドメイン用の有名スキルが既にないか確認する。リーダーボードは総インストール数でランク付けし、人気で実戦投入された選択肢を表に出す。

例：Web 開発のトップスキルには次がある：
- `vercel-labs/agent-skills` — React、Next.js、Web デザイン（各 100K+ インストール）
- `anthropics/skills` — フロントエンドデザイン、文書処理（100K+ インストール）

### ステップ 3: スキルを検索

リーダーボードがニーズをカバーしないなら find コマンドを実行：

```bash
npx skills find [query] [--owner <owner>]
```

例：

- 「React アプリを速くするには？」→ `npx skills find react performance`
- 「PR レビューを手伝って？」→ `npx skills find pr review`
- 「changelog を作りたい」→ `npx skills find changelog`

### ステップ 4: 推奨前に品質を検証

**検索結果だけを根拠にスキルを推奨しない。** 常に次を確認する：

1. **インストール数** — 1K+ を好む。100 未満は慎重に。
2. **ソースの信頼性** — 公式（`vercel-labs`、`anthropics`、`microsoft`）は未知の作者より信頼できる。
3. **GitHub スター** — ソースリポジトリを確認。100 スター未満のリポジトリのスキルは懐疑的に扱う。

### ステップ 5: 選択肢をユーザーに提示

関連スキルが見つかったら、次を含めて提示する：

1. スキル名と役割
2. インストール数とソース
3. 実行できるインストールコマンド
4. skills.sh で詳細を学ぶリンク

回答例：

```
I found a skill that might help! The "react-best-practices" skill provides
React and Next.js performance optimization guidelines from Vercel Engineering.
(185K installs)

To install it:
npx skills add vercel-labs/agent-skills@react-best-practices

Learn more: https://skills.sh/vercel-labs/agent-skills/react-best-practices
```

### ステップ 6: インストールを提案

ユーザーが進めたいなら、代わりにインストールできる：

```bash
npx skills add <owner/repo@skill> -g -y
```

`-g` はグローバル（ユーザー単位）、`-y` は確認プロンプトをスキップする。

## よくあるスキルカテゴリ

検索時に次のカテゴリを考慮する：

| Category        | Example Queries                          |
| --------------- | ---------------------------------------- |
| Web Development | react, nextjs, typescript, css, tailwind |
| Testing         | testing, jest, playwright, e2e           |
| DevOps          | deploy, docker, kubernetes, ci-cd        |
| Documentation   | docs, readme, changelog, api-docs        |
| Code Quality    | review, lint, refactor, best-practices   |
| Design          | ui, ux, design-system, accessibility     |
| Productivity    | workflow, automation, git                |

## 効果的な検索のヒント

1. **具体的キーワード**：「react testing」は「testing」だけよりよい
2. **別の用語を試す**：「deploy」が効かなければ「deployment」や「ci-cd」
3. **人気ソースを確認**：多くのスキルは `vercel-labs/agent-skills` や `ComposioHQ/awesome-claude-skills` から来る

## スキルが見つからないとき

関連スキルがなければ：

1. 既存スキルがなかったことを伝える
2. 一般能力で直接タスクを手伝えると申し出る
3. `npx skills init` で独自スキルを作れると提案する

例：

```
I searched for skills related to "xyz" but didn't find any matches.
I can still help you with this task directly! Would you like me to proceed?

If this is something you do often, you could create your own skill:
npx skills init my-xyz-skill
```