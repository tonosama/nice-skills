---

name: writing-clearly-and-concisely

description: |

  人が読む文章（ドキュメント、コミットメッセージ、エラーメッセージ、説明、レポート、UI テキスト）を書くときに使う。Strunk の古典的ルールで、より明確で力強くプロフェッショナルな文体にする。

---

# 明確で簡潔な文章

## 概要

明確さと力強さで書く。このスキルは「すべきこと」（Strunk）と「避けること」（AI 文体パターン）を扱う。

## このスキルを使うとき

人間向けの文章を書くあらゆる場面:

- ドキュメント、README、技術説明
- コミットメッセージ、PR 説明
- エラーメッセージ、UI コピー、ヘルプ、コメント
- レポート、要約、説明
- 明瞭化のための編集

**人間が読む文を書くなら、このスキルを使う。**

## コンテキスト節約戦略

コンテキストが厳しいとき:

1. 判断で下書きを書く
2. 下書きと該当セクションファイルを subagent に渡す
3. コピーエディットして改訂を受け取る

1 セクションのみ（約 1,000–4,500 tokens）を読み込むと、全体より大幅に節約できる。

## Elements of Style

William Strunk Jr. の *The Elements of Style* (1918) は、明確に書き、容赦なく削ることを教える。

### ルール

**Elementary Rules of Usage（文法・句読点）:**

1. 単数所有格は 's を付ける
2. 系列の各語の後にコンマ（最後を除く）
3. 括号的表現をコンマで囲む
4. 等位節を導く接続詞の前にコンマ
5. コンマだけで独立節を結ばない
6. 文を二つに割らない
7. 文頭の分詞句は文法的主語を指す

**Elementary Principles of Composition:**

8. トピックごとに一段落
9. 段落は主題文で始める
10. **能動態を使う**
11. **肯定形で述べる**
12. **具体的で明確な言葉を使う**
13. **不要な語を省く**
14. だらだら続く緩い文を避ける
15. 等位の考えは同じ形で表す
16. **関連する語を近づける**
17. 要約では時制を一つに保つ
18. **強調語は文の終わりに置く**

### リファレンスファイル

上記は Strunk 原文の要約。完全な説明と例:

| セクション | ファイル | 約 Tokens |
|---------|------|---------|
| 文法・句読点・コンマ | `02-elementary-rules-of-usage.md` | 2,500 |
| 段落、能動態、簡潔さ | `03-elementary-principles-of-composition.md` | 4,500 |
| 見出し、引用、書式 | `04-a-few-matters-of-form.md` | 1,000 |
| 語の選び、よくある誤り | `05-words-and-expressions-commonly-misused.md` | 4,000 |

**多くのタスクは `03-elementary-principles-of-composition.md` だけで足りる** — 能動態、肯定形、具体性、省略をカバーする。

## 避けるべき AI 文体パターン

LLM は統計的平均に回帰し、_generic で膨らんだ散文を生む。避ける:

- **大げさな形容**: pivotal, crucial, vital, testament, enduring legacy
- **空虚な -ing 句**: ensuring reliability, showcasing features
- **宣伝的形容詞**: groundbreaking, seamless, robust, cutting-edge
- **AI 頻出語**: delve, leverage, multifaceted, foster, realm, tapestry
- **過剰な装飾**: 箇条書きの乱用、emoji、太字の多用

具体的に書く。実際に何をするかを述べる。

背景研究は `signs-of-ai-writing.md` を参照。

## リファレンスと読み込みトリガー

**必須 — LOADING TRIGGERS**:

- Step 20 でドキュメント段落や構成を編集する前に、**必ず** `03-elementary-principles-of-composition.md` を全文読む。
- 文法や等位節を確認する前に、**必ず** `02-elementary-rules-of-usage.md` を読む。
- 下書きスコープ段階ではトークン節約のためルール類を読み込まない。

### 受動態の制限と語数削減

1. **受動態**: 文法的に能動態が不可能な文脈以外では受動態を使わない。
2. **簡潔さ目標**: 最終版は初稿より語数で少なくとも 15% 短くする。

## Memory Sync

主要な技術的知見、アーキテクチャ上の判断、コードのリファクタリング、リスク評価を完了したら、ローカルの memory capture を**必ず**起動する。

1. 最終サマリまたは成果物をプロジェクトディレクトリ内の Markdown ファイルとして保存する。
2. キャプチャスクリプトを実行する:
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. 新しい学び、ポリシー、技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動ルーティングされる。