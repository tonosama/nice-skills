---

name: plugin-forge

description: |

  マーケットプレイス向け AI コーディングエージェント plugin の作成と管理（適切な構造、manifest、マーケットプレイス連携を含む）。マーケットプレイス用 plugin の作成、plugin コンポーネント（commands、agents、hooks）の追加、plugin バージョンの bump、`plugin.json` / `marketplace.json` manifest の編集時に使用する。



  マーケットプレイス向け AI コーディングエージェント plugin の作成と管理（適切な構造、manifest、マーケットプレイス連携を含む）。マーケットプレイス用 plugin の作成、plugin コンポーネント（commands、agents、hooks）の追加、plugin バージョンの bump、`plugin.json` / `marketplace.json` manifest の編集時に使用する。



---



# CC Plugin Forge



## 目的



正しい構造、manifest、マーケットプレイス連携を備えた AI コーディングエージェント plugin の構築と管理。ワークフロー、自動化スクリプト、参照ドキュメントを含む。



## 使用タイミング



- マーケットプレイス向けの新規 plugin 作成

- plugin コンポーネント（commands、skills、agents、hooks）の追加・変更

- plugin バージョンの更新

- plugin または marketplace の manifest 作業

- ローカル plugin テストのセットアップ

- plugin の公開



## はじめに



### 新規 plugin の作成



`create_plugin.py` で plugin 構造を生成する:



```bash

python scripts/create_plugin.py plugin-name \

  --marketplace-root /path/to/marketplace \

  --author-name "Your Name" \

  --author-email "your.email@example.com" \

  --description "Plugin description" \

  --keywords "keyword1,keyword2" \

  --category "productivity"

```



これにより自動的に:



- plugin ディレクトリ構造の作成

- `plugin.json` manifest の生成

- README テンプレートの作成

- `marketplace.json` の更新



### バージョンの bump



`bump_version.py` で両方の manifest のバージョンを更新する:



```bash

python scripts/bump_version.py plugin-name major|minor|patch \

  --marketplace-root /path/to/marketplace

```



セマンティックバージョニング:



- **major**: 破壊的変更 (1.0.0 → 2.0.0)

- **minor**: 新機能、リファクタリング (1.0.0 → 1.1.0)

- **patch**: バグ修正、ドキュメント (1.0.0 → 1.0.1)



## 開発ワークフロー



### 1. 構造の作成



手動アプローチ（スクリプトを使わない場合）:



```bash

mkdir -p plugins/plugin-name/.claude-plugin

mkdir -p plugins/plugin-name/commands

mkdir -p plugins/plugin-name/skills

```



### 2. Plugin manifest



ファイル: `plugins/plugin-name/.claude-plugin/plugin.json`



```json

{

  "name": "plugin-name",

  "version": "0.1.0",

  "description": "Plugin description",

  "author": {

    "name": "Your Name",

    "email": "your.email@example.com"

  },

  "keywords": ["keyword1", "keyword2"]

}

```



### 3. マーケットプレイスへの登録



`.claude-plugin/marketplace.json` を更新する:



```json

{

  "name": "plugin-name",

  "source": "./plugins/plugin-name",

  "description": "Plugin description",

  "version": "0.1.0",

  "keywords": ["keyword1", "keyword2"],

  "category": "productivity"

}

```



### 4. コンポーネントの追加



各ディレクトリに作成する:



| Component | Location | Format |
|-----------|----------|--------|
| Commands | `commands/` | Markdown with frontmatter |
| Skills | `skills/<name>/` | Directory with `SKILL.md` |
| Agents | `agents/` | Markdown definitions |
| Hooks | `hooks/hooks.json` | Event handlers |
| MCP Servers | `.mcp.json` | External integrations |



### 5. ローカルテスト



```bash

# Add marketplace

/plugin marketplace add /path/to/marketplace-root



# Install plugin

/plugin install plugin-name@marketplace-name



# After changes: reinstall

/plugin uninstall plugin-name@marketplace-name

/plugin install plugin-name@marketplace-name

```



## Plugin パターン



### Framework plugin



フレームワーク固有のガイダンス（React、Vue など）向け:



```

plugins/framework-name/

├── .claude-plugin/plugin.json

├── skills/

│   └── framework-name/

│       ├── SKILL.md

│       └── references/

├── commands/

│   └── prime/

│       ├── components.md

│       └── framework.md

└── README.md

```



### Utility plugin



ツールと commands 向け:



```

plugins/utility-name/

├── .claude-plugin/plugin.json

├── commands/

│   ├── action1.md

│   └── action2.md

└── README.md

```



### Domain plugin



ドメイン固有の知識向け:



```

plugins/domain-name/

├── .claude-plugin/plugin.json

├── skills/

│   └── domain-name/

│       ├── SKILL.md

│       ├── references/

│       └── scripts/

└── README.md

```



## Command の命名



`:` 区切りによるサブディレクトリベースの名前空間:



- `commands/namespace/command.md` → `/namespace:command`

- `commands/simple.md` → `/simple`



例:



- `commands/prime/vue.md` → `/prime:vue`

- `commands/docs/generate.md` → `/docs:generate`



## バージョン管理



**重要:** 次の両方の場所でバージョンを更新すること:



1. `plugins/<name>/.claude-plugin/plugin.json`

2. `.claude-plugin/marketplace.json`



自動化には `bump_version.py` を使用する。



## Git commit



Conventional Commits を使用する:



```bash

git commit -m "feat: add new plugin"

git commit -m "fix: correct plugin manifest"

git commit -m "docs: update plugin README"

git commit -m "feat!: breaking change"

```



## 参照ドキュメント



同梱の詳細ドキュメント:



| Reference | Content |
|-----------|---------|
| `references/plugin-structure.md` | ディレクトリ構造、manifest スキーマ、コンポーネント |
| `references/marketplace-schema.md` | マーケットプレイス形式、plugin エントリ、配布 |
| `references/workflows.md` | ステップバイステップのワークフロー、パターン、公開 |



### スクリプト



| Script | Purpose |
|--------|---------|
| `scripts/create_plugin.py` | 新規 plugin のスキャフォールド |
| `scripts/bump_version.py` | バージョンの更新 |



## アンチパターン



| 避けること | 理由 | 代わりに |
|-------|-----|---------|
| 名前空間フォルダの大文字 | CLI ルーティングパラメータを壊す | 小文字と `:` デリミタを使用 |
| チェックなしの raw commit | 構文エラーがローカル install ワークフローを阻害する | commit 前に `plugin.json` スキーマの検証を実行 |



## 参照と読み込みトリガー



**必須 — 読み込みトリガー**:

- マーケットプレイスプロパティを変更する前に、[marketplace-schema.md](references/marketplace-schema.md) を**必ず**最初から最後まで読むこと。

- ディレクトリを再構成する、または hooks を追加する前に、[plugin-structure.md](references/plugin-structure.md) を**必ず**読むこと。

- 単純なバージョン bump のみの場合は workflows 参照ガイドを**読み込まない**こと。