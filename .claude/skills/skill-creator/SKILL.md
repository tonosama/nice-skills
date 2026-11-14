---

name: skill-creator

description: |

  Claude skill の作成・更新・検証・パッケージ化のガイド。新規 skill の設計、skill ディレクトリの初期化、SKILL.md の作成、skill ユーティリティスクリプト（init_skill.py、package_skill.py、quick_validate.py）の実行、skill 形式の検証、ZIP へのパッケージ化を求められたときに起動。Keywords: SKILL.md, init_skill.py, package_skill.py, quick_validate.py, create skill, update skill, package skill, zip skill, skill metadata, progressive disclosure.



  Claude skill の作成・更新・検証・パッケージ化のガイド。新規 skill の設計、skill ディレクトリの初期化、SKILL.md の作成、skill ユーティリティスクリプト（init_skill.py、package_skill.py、quick_validate.py）の実行、skill 形式の検証、ZIP へのパッケージ化を求められたときに起動。Keywords: SKILL.md, init_skill.py, package_skill.py, quick_validate.py, create skill, update skill, package skill, zip skill, skill metadata, progressive disclosure.



license: Complete terms in LICENSE.txt

---



# Skill Creator - エキスパート向けワークフローとガイドライン



Claude の能力を拡張する skill を、作成・洗練・検証・パッケージ化するためのプロフェッショナル級ガイドラインとワークフローを提供する。



## Progressive Disclosure とリソース読み込みトリガー



本 skill は `scripts/` サブディレクトリのユーティリティスクリプトに依存する。スクリプトのソースを全面的に読むのではなく、次のコマンドライン実行でブラックボックスとして利用を起動する。



- **新規 skill の初期化**: `python scripts/init_skill.py --help` で設定引数を確認する。

- **クイック検証**: `python scripts/quick_validate.py <path/to/skill>` でパッケージ化せずローカル準拠を確認する。

- **検証とパッケージ化**: `python scripts/package_skill.py <path/to/skill> <output-dir>` でスキーマ検証とリソースバンドルを行う。



## 自由度の調整と制約



- **制約レベル: 中〜高**



  - **高い厳密性**: ディレクトリ構造、YAML frontmatter のキー（`name`、`description`）、スクリプト実行プロトコル、検証スキーマは厳密に従う。

  - **高い自由度**: 実際のコンテンツ設計、ドメイン固有の手順、ビジュアルスタイル、アセット／スクリプトの選択は対象ドメインに合わせてカスタマイズする。



## 決定木: リソース分離のトレードオフ



次の木で、特定コンテンツをどこに置くか決める。`SKILL.md` をスリムに保つ（最大約 5k 語）ことでコンテキスト効率が高まり、LLM の散漫を防ぐ。



```

Is the content a code/script, a static reference, or a template/asset?

 ├─ Executable Code or Tool CLI

 │   └─ Is it a short snippet or a reusable utility?

 │       ├─ Short snippet (<=10 lines) → In-line code block in SKILL.md

 │       └─ Reusable/complex utility → Save in scripts/ (e.g., scripts/rotate_pdf.py)

 ├─ Domain Information, Schemas, Policies, APIs, or Documentation

 │   └─ Is it core to the execution loop or auxiliary reference?

 │       ├─ Core execution loop & rules → Directly in SKILL.md

 │       └─ Auxiliary schemas/detailed docs → Save in references/ (e.g., references/api_docs.md)

 └─ Boilerplate, Images, Fonts, or Project Templates

     └─ Save in assets/ (e.g., assets/boilerplate-html/)

```



## プロフェッショナルなマインドセットと設計フレームワーク



skill を作成・変更するとき、このメンタルチェックリストを実行する。



1. **利用者は誰か？**（多くは別の Claude インスタンス）。指示は**命令形／不定詞形**で書く（例: 「検証を実行するには…」ではなく「To run validation, execute…」のような形）。

2. **必要最小のコンテキストは？** `SKILL.md` は 5,000 語未満に保つ。大きなスキーマ、データ表、長いリストは `references/` へ移す。

3. **脆い箇所はどこか？** Claude やユーザーが失敗しうる場所（パス解決、依存欠如、レート制限など）を明確にし、明示的なフォールバックを示す。

4. **テスト可能か？** 検証スクリプトがクリーンに走り、納品前に構造を検証できること。



## ステップバイステップの Skill エンジニアリング手順



### Step 1: 要件収集とスコープ



- 対象ドメインと特定のトリガーを定義する。

- 入力形式と出力要件を特定する。

- ツール依存と必要な Python／バイナリパッケージをマップする。



### Step 2: ディレクトリ初期化



- `python scripts/init_skill.py <skill-name> --path <output-directory>` を実行し、準拠したレイアウトを作る。

- 生成フォルダを確認する: `scripts/`、`references/`、`assets/`、`SKILL.md`。



### Step 3: リソースエンジニアリング



- **スクリプト作成**: Python／Bash を `scripts/` に置く。モジュール化し自己完結させる。

- **references の充填**: 大きなスキーマ、markdown 表、静的ドキュメントを `references/` に置く。

- **アセット収集**: ボイラープレート、テンプレート、メディアを `assets/` に置く。



### Step 4: SKILL.md 指示の記述



- 手順はすべて**命令形／不定詞形**（例: 「入力データを分析する」「package スクリプトを実行する」）。

- 堅牢な YAML frontmatter に `name` と `description`（明確な WHAT、WHEN、トリガー KEYWORDS）を含める。

- `NEVER` アンチパターンテーブル、決定木、エラー解決シナリオを追加する。



### Step 5: 検証と確認



- `python scripts/quick_validate.py <path/to/skill-folder>` を実行する。

- バリデータが指摘した問題を修正する（不正 YAML、壊れたファイル参照など）。



### Step 6: パッケージ化



- 検証済み skill をパッケージする:

  ```bash

  python scripts/package_skill.py <path/to/skill-folder> [optional/output/dir]

  ```

- 得られた `.zip` 成果物をユーザーに渡す。



## 重要なアンチパターン（NEVER リスト）



| アンチパターン | 説明 | 代替／解決策 |

| :--- | :--- | :--- |

| **NEVER** 情報の重複 | `SKILL.md` と reference ファイル間の重複は保守負荷と検証失敗の原因になる。 | 手順の単一の真実の源は `SKILL.md`；reference はデータ用。 |

| **NEVER** 二人称テキスト | 「You should do X」「We need to Y」は LLM のパース効率を下げる。 | 命令形: 「Execute command X」「Verify file exists」。 |

| **NEVER** 巨大なインライン | `SKILL.md` に 50 行超のコードブロックや DB スキーマを埋め込むとコンテキストが肥大化。 | コードは `scripts/`、スキーマ／表は `references/` へ。 |

| **NEVER** 絶対ローカルパス | `/home/user/...` のハードコードは環境間互換を壊す。 | skill ルートからの相対パスまたは環境変数。 |

| **NEVER** 検証エラー無視 | パッケージ時に検証失敗を無視すると下流で skill 読み込みが壊れる。 | スクリプトの警告と検証失敗はすべてブロッカーとして扱う。 |

| **NEVER** description なしでパッケージ | 空またはプレースホルダーの frontmatter では Claude がマッチ／トリガーできない。 | WHAT、WHEN、KEYWORDS を明示した堅牢な description を書く。 |



## よくあるエラーシナリオとフォールバック



### シナリオ 1: ターゲットディレクトリの権限または既存ディレクトリで `init_skill.py` が失敗



- **根本原因**: ディレクトリが既に存在する、または読み取り専用権限。

- **フォールバック**: 

  1. `--path` で別の出力パスを指定する。

  2. ディレクトリが存在し更新必須なら、手動でファイルを作るかターゲット名を変更する。



### シナリオ 2: メタデータスキーマで検証失敗



- **根本原因**: YAML frontmatter の形式不正（`name`/`description` 欠落、`---` 欠落、不正文字など）。

- **フォールバック**: ファイル先頭が次と完全一致することを確認する:

  ```yaml

  ---

  name: your-skill-name

  description: Robust text here.

  ---

  ```

  description にクォートなしのコロン `:` があると YAML パーサーを壊すので避ける。



### シナリオ 3: 「Unreferenced resource files」で検証失敗



- **根本原因**: `scripts/`、`references/`、`assets/` にファイルがあるが `SKILL.md` 本文で一切言及・リンクされていない。

- **フォールバック**: `SKILL.md` にそのリソースの使い方・タイミングを書くか、未使用ファイルを skill ディレクトリから削除する。