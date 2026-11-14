---

name: excalidraw

description: |

  `*.excalidraw` または `*.excalidraw.json` の作業、ユーザーが diagram／flowchart に言及、またはアーキテクチャ可視化を依頼するときに使用。冗長 JSON（単一ファイル 4k–22k トークン、read 上限超の場合あり）によるコンテキスト枯渇を防ぐため、すべての Excalidraw 操作を subagent に委譲する。

---

# Excalidraw Subagent Delegation

## Overview

**基本原則:** メインエージェントは Excalidraw ファイルを**直接読まない**。常に subagent に委譲しコンテキスト消費を隔離する。

Excalidraw ファイルはトークンコストが高く情報密度が低い JSON である。単一ファイルは 4k–22k トークン（最大は read 上限超）。複数 diagram を読むとコンテキストが急速に枯渇（7 ファイル＝67k トークン＝予算の約 33%）。

## The Problem

Excalidraw JSON の構造:

- 各 shape に 20 以上のプロパティ（x、y、width、height、strokeColor、seed、version など）
- 大半は視覚メタデータ（位置、スタイル、roughness）
- 実コンテンツはテキストラベルと要素関係（ファイルの 10% 未満）
- **シグナル対ノイズ比が極めて低い**

例: 14 要素 diagram＝596 行・16K・約 4k トークン。79 要素＝2,916 行・88K・約 22k トークン（read 上限超）。

## このスキルを使うとき

**次のいずれかで起動:**

- パスに `.excalidraw` または `.excalidraw.json` を含む
- 「diagram の説明／更新／作成」「architecture を見せる」「flow を可視化」などの依頼
- flowchart、アーキテクチャ diagram、Excalidraw ファイルへの言及
- 視覚成果物を伴うアーキテクチャ／設計ドキュメント作業

**次でも委譲:**

- 「小さい」ファイル（最小でも 4k トークン）
- 「さっと確認」（コンポーネント名確認でも JSON 全体を読む）
- 単一ファイル操作（隔離でコンテキスト汚染を防ぐ）
- 変更（メインコンテキストで形式理解は不要）

## Delegation Pattern

### Main Agent Responsibilities

**しないこと:**

- ❌ Read ツールで `*.excalidraw` を読む
- ❌ メインコンテキストで Excalidraw JSON をパースする
- ❌ 比較のため複数 diagram を読み込む
- ❌ 「形式理解」のためにファイルを inspect する

**常にすること:**

- ✅ すべての Excalidraw 操作を subagent に委譲
- ✅ subagent に明確なタスク説明を渡す
- ✅ テキストのみの要約を要求（生 JSON 不可）
- ✅ diagram 分析を本流作業から隔離

### Subagent Task Templates

#### Read/Understand Operation

```

Task: Extract and explain the components in [file.excalidraw.json]

Approach:
1. Read the Excalidraw JSON
2. Extract only text elements (ignore positioning/styling)
3. Identify relationships between components
4. Summarize architecture/flow

Return:
- List of components/services with descriptions
- Connection/dependency relationships
- Key insights about the architecture
- DO NOT return raw JSON or verbose element details

```

#### Modify Operation

```

Task: Add [component] to [file.excalidraw.json], connected to [existing-component]

Approach:
1. Read file to identify existing elements
2. Find [existing-component] and its position
3. Create new element JSON for [component]
4. Add arrow elements for connections
5. Write updated file

Return:
- Confirmation of changes made
- Position of new element
- IDs of created elements

```

#### Create Operation

```

Task: Create new Excalidraw diagram showing [description]

Approach:
1. Design layout for [number] components
2. Create rectangle elements with text labels
3. Add arrows showing relationships
4. Use consistent styling (colors, fonts)
5. Write to [file.excalidraw.json]

Return:
- Confirmation of file created
- Summary of components included
- File location

```

#### Compare Operation

```

Task: Compare architecture approaches in [file1] vs [file2]

Approach:
1. Read both files
2. Extract text labels from each
3. Identify structural differences
4. Compare component relationships

Return:
- Key differences in architecture
- Components unique to each approach
- Relationship/flow differences
- DO NOT return full element details from both files

```

## Common Rationalizations (STOP and Delegate Instead)

| 言い訳 | 実態 | 対処 |
|--------|---------|------------|
| 直接読むのが最も効率的 | 不要に 4k–22k トークン消費 | subagent に委譲 |
| 直接読む方がトークン効率が良い | ベースラインで予算の 9–45% | 常に委譲 |
| 一度きりの分析に最適 | 「一度きり」でもメインコンテキストを汚す | subagent 隔離 |
| JSON は単純 | 単純 ≠ トークン効率 | それでも委譲 |
| 形式理解が必要 | メインエージェントでは不要 | subagent が形式を扱う |
| 18k トークンは妥当な範囲 | 「妥当」は主観的言い訳 | 硬いルール: 委譲 |
| コンポーネントをさっと確認 | さっとでも JSON 全体を読む | subagent でテキスト抽出 |
| ファイルが小さい（16K） | 4k トークンは小さくない | サイズ閾値は無関係 |

## Red Flags - STOP and Delegate

次をしようとしたら自分を止める:

- `.excalidraw` に Read ツール
- コンポーネントの「さっと確認」
- 変更前の「構造理解」
- 「何があるか見る」ためにファイル読み込み
- 複数 diagram の横並び比較
- 「テキストだけ抽出」のため JSON パース

**これらはすべて Task ツールで subagent を使う意味。**

## Quick Reference

| 操作 | メインエージェント | subagent の返却 |
|-----------|-------------------|------------------|
| **Understand diagram** | 「Extract and explain」テンプレで委譲 | コンポーネント一覧＋関係 |
| **Modify diagram** | 「Add [X] connected to [Y]」テンプレで委譲 | 確認＋変更内容 |
| **Create diagram** | 「Create showing [description]」テンプレで委譲 | ファイル場所＋要約 |
| **Compare diagrams** | 「Compare [A] vs [B]」テンプレで委譲 | 差分（生 JSON 不可）|

## Token Analysis (Why This Matters)

ベースラインテストの実データ:

| シナリオ | 委譲なし | 委譲あり | 節約 |
|----------|-------------------|-----------------|---------|
| 単一大ファイル | 22k トークン（予算 45%） | 約 500 トークン（要約） | 98% |
| 2 ファイル比較 | 18k トークン（9%） | 約 800 トークン（diff 要約） | 96% |
| 変更タスク | 14k トークン（7%） | 約 300 トークン（確認） | 98% |

**コンテキスト汚染の影響:**

- 7 diagram すべてを読む: 67k トークン（200k 予算の 33%）
- 委譲時: 約 2k トークン（subagent に隔離）
- **節約: コンテキスト予算 97% 温存**

## Implementation Example

**❌ 悪い例（直接 Read）:**

```

User: "What architecture is shown in detailed-architecture.excalidraw.json?"

Agent: Let me read that file... [reads 22k tokens into main context]

```

**✅ 良い例（subagent 委譲）:**

```

User: "What architecture is shown in detailed-architecture.excalidraw.json?"

Agent: I'll use a subagent to extract the architecture details.

[Dispatches Task tool with general-purpose subagent]

Task: Extract and explain components in .ryanquinn3/ticketing/detailed-architecture.excalidraw.json

[Receives ~500 token summary with component list and relationships]

[Responds to user with architecture explanation, main context preserved]

```

## Why "Straightforward JSON" Doesn't Matter

よくある言い訳:「形式が単純だから読める」

**問題は複雑さではなく冗長さ:**

- 要素あたり 20+ プロパティの単純構造
- 繰り返しメタデータ（seed、version、nonce、roughness）
- 意味的に不要な位置データ（x、y、width、height）
- コンテンツと無関係な視覚スタイル（strokeColor、opacity、fillStyle）

**トークンコストは量で決まり、複雑さではない。**

「単純」な JSON でも 4k–22k トークンになる理由:

- 79 要素 × 約 280 トークン/要素＝22k トークン
- 大半はメタデータノイズ
- 重要なのはテキストラベルと関係のみ（約 10%）

## The Iron Law

**メインエージェントは Excalidraw ファイルを読まない。例外なし。**

対象外でも不可:

- 「さっと確認」
- 「小さいファイル」
- 「形式理解」
- 「一度きりの分析」
- 「最適な効率」

### Subagent Validation & Binary Image Checks

1. subagent が Excalidraw JSON レイアウトを更新する場合、書き込み前に構文を検証すること（例: `python -m json.tool` によるパース確認）。
2. 視覚レイアウトの検証が必要なら、生座標を読む代わりに静的画像（利用可能なら CLI で PNG）に変換する。

## References

**必須 - 自己完結ディレクティブ**:

このスキルは自己完結である。本ワークフロー実行中に明示的に作成されない限り、外部ファイルやディレクトリ参照を読み込まない。

**常に委譲。subagent による隔離は無料。**