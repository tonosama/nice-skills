---

name: perplexity

description: |

  Perplexity AI による Web 検索・調査。ユーザーが「search」「find」「look up」「ask」「research」「what's the latest」などと言った汎用クエリ向け。ライブラリ／フレームワーク docs（Context7）やワークスペース質問には使わない。



  Perplexity AI による Web 検索・調査。ユーザーが「search」「find」「look up」「ask」「research」「what's the latest」などと言った汎用クエリ向け。ライブラリ／フレームワーク docs（Context7）やワークスペース質問には使わない。



---



# Perplexity Tools



ユーザーが汎用クエリで「search」「find」「look up」「ask」「research」「what's the latest」と言った場合にのみ使用。ライブラリ／フレームワーク docs（Context7）、gt CLI（Graphite MCP）、ワークスペース質問（Nx MCP）には使わない。



## クイックリファレンス



**どの Perplexity ツール？**

- 検索結果／URL が必要 → **Perplexity Search**

- 対話形式の回答が必要 → **Perplexity Ask**

- 深い調査が必要 → **Researcher agent**（`/research <topic>`）



**Perplexity 以外—代わりにこれを使う:**

- ライブラリ／フレームワーク docs → **Context7 MCP**

- Graphite `gt` CLI → **Graphite MCP**

- このワークスペース → **Nx MCP**

- 特定 URL → **URL Crawler**



## Perplexity Search



**使用タイミング:**

- 汎用検索、リソース探索

- 最新ベストプラクティス、近時情報

- チュートリアル／ブログ記事の発見

- 「search for...」「find...」「look up...」



**デフォルトパラメータ（常に使用）:**

```typescript

mcp__perplexity__perplexity_search({

  query: "your search query",

  max_results: 3,           // Default is 10 - too many!

  max_tokens_per_page: 512  // Reduce per-result content

})

```



**上限を上げる場合:**

次のときのみ:

- ユーザーが包括的な結果を明示的に要求

- 初回検索で有用なものがなかった

- 複数ソースが必要な複雑トピック



```typescript

// Increased limits (use sparingly)

mcp__perplexity__perplexity_search({

  query: "complex topic",

  max_results: 5,

  max_tokens_per_page: 1024

})

```



## Perplexity Ask



**使用タイミング:**

- 検索結果ではなく対話的説明が必要

- Web 情報の統合

- 現状文脈付きの概念説明



**使用例:**

```typescript

mcp__perplexity__perplexity_ask({

  messages: [

    {

      role: "user",

      content: "Explain how postgres advisory locks work"

    }

  ]

})

```



**使わない場合:**

- ライブラリドキュメント（Context7）

- 深い多ソース調査（researcher agent）



## 禁止ツール



**使用禁止:** `mcp__perplexity__perplexity_research`



**代わりに:** Researcher agent（`/research <topic>`）

- Token コスト: 30–50k tokens

- 引用付き多ソース統合

- 複雑な質問のみ控えめに



## ツール選択チェーン



**優先順:**

1. **Context7 MCP** — ライブラリ／フレームワーク docs

2. **Graphite MCP** — `gt` CLI の言及

3. **Nx MCP** — このワークスペースの質問

4. **Perplexity Search** — 汎用検索

5. **Perplexity Ask** — 対話回答

6. **Researcher agent** — 深い多ソース調査

7. **WebSearch** — 最後の手段（Perplexity 尽きた後）



## 例



**✅ 正しい—Perplexity Search:**

- "Find postgres migration best practices"

- "Search for React testing tutorials"

- "Look up latest trends in microservices"



**✅ 正しい—Perplexity Ask:**

- "Explain how postgres advisory locks work"

- "What are the trade-offs of microservices?"



**❌ 誤り—Context7 を使う:**

- "Search for React hooks documentation" → Context7 MCP

- "Find Next.js routing docs" → Context7 MCP

- "Look up Temporal workflow API" → Context7 MCP



**❌ 誤り—Graphite MCP を使う:**

- "Search for gt stack commands" → Graphite MCP

- "Find gt branch workflow" → Graphite MCP



**❌ 誤り—Nx MCP を使う:**

- "Search for build config"（このワークスペース） → Nx MCP

- "Find project dependencies"（このワークスペース） → Nx MCP



## 要点



- **要点を先に** — 主張は冒頭か？

- **「gt」= Graphite MCP** — `gt` の言及はすべて Graphite

- **深い調査 = /research** — perplexity_research ツールではない

- **フォールバックチェーン** — Search → Ask → WebSearch（最後）



## 参考文献



**必須—自己完結ディレクティブ**:

本スキルは自己完結型。本 workflow 実行中に明示的に作成されない限り、外部ファイルやディレクトリを読み込まない。



### クエリ簡略化とエンドポイントタイムアウト



1. **クエリ簡略化**: 入力から会話的な filler（「please find me the latest information about...」等）を除き検索パースコストを下げる。

2. **エンドポイントタイムアウト**: Perplexity ツールが失敗／タイムアウトしたら、外部 Web クエリの前にローカルワークスペースインデックスを確認するフォールバックを使う。