---

name: skill-judge

description: |

  公式仕様とベストプラクティスに照らして Agent Skill の設計品質を評価する。SKILL.md や skill パッケージのレビュー・監査・改善時に使用。多次元スコアリングと実行可能な改善提案を提供する。



---



# Skill Judge（スキル判定）



公式仕様および 17 以上の公式例から導いたパターンに照らして Agent Skill を評価する。



---



## コア哲学



### Skill とは何か



Skill はチュートリアルではない。Skill は**知識の外部化メカニズム**である。



従来の AI 知識はモデルパラメータに閉じ込められている。新能力を教えるには：

```

Traditional: Collect data → GPU cluster → Train → Deploy new version

Cost: $10,000 - $1,000,000+

Timeline: Weeks to months

```



Skill はこれを変える：

```

Skill: Edit SKILL.md → Save → Takes effect on next invocation

Cost: $0

Timeline: Instant

```



これは「AI をトレーニングする」から「AI を教育する」へのパラダイムシフトである — トレーニング不要のホットスワップ可能な LoRA adapter のように。自然言語の Markdown を編集するとモデルの振る舞いが変わる。



### コア公式



> **良い Skill = Expert-only Knowledge − What Claude Already Knows**



Skill の価値は **knowledge delta** — 提供内容とモデルが既に知っていることの差 — で測る。



- **Expert-only knowledge**: 意思決定ツリー、トレードオフ、エッジケース、アンチパターン、ドメイン固有の思考フレームワーク — 多年の経験で蓄積されるもの

- **What Claude already knows**: 基本概念、標準ライブラリの使い方、一般的プログラミングパターン、汎用ベストプラクティス



Skill が「PDF とは何か」「for-loop の書き方」を説明すると、Claude が既に持つ知識を圧縮している。これは **token waste** — context window は system prompt、会話履歴、他 Skill、ユーザーリクエストと共有される公共資源である。



### Tool 対 Skill



| 概念 | 本質 | 機能 | 例 |

|---------|---------|----------|---------|

| **Tool** | モデルが CAN すること | アクション実行 | bash, read_file, write_file, WebSearch |

| **Skill** | モデルが KNOWS すること | 意思決定を導く | PDF processing, MCP building, frontend design |



Tool は能力境界を定義する — bash tool がなければモデルはコマンドを実行できない。

Skill は知識を注入する — frontend-design Skill がなければモデルは汎用 UI を産む。



**方程式**：

```

General Agent + Excellent Skill = Domain Expert Agent

```



同じ Claude モデルでも、読み込む Skill が違えば別のエキスパートになる。



### Skill 内の3種類の知識



評価時は各セクションを分類する：



| 種別 | 定義 | 扱い |

|------|------------|-----------|

| **Expert** | Claude が本当に知らない | 必ず残す — これが Skill の価値 |

| **Activation** | Claude は知るが思い浮かばないことがある | 短ければ残す — リマインダー |

| **Redundant** | Claude が確実に知っている | 削除すべき — token を浪費 |



Skill 設計の技は Expert コンテンツを最大化し、Activation は控えめに、Redundant は容赦なく削ることである。



---



## 評価次元（合計120点）



### D1: Knowledge Delta（20点）— コア次元



最重要次元。Skill は本物のエキスパート知識を追加しているか？



| スコア | 基準 |

|-------|----------|

| 0-5 | Claude が知る基礎を説明（X とは、コードの書き方、標準ライブラリチュートリアル） |

| 6-10 | 混在：明白な内容でエキスパート知識が薄まる |

| 11-15 | ほぼエキスパート知識、冗長性最小 |

| 16-20 | 純粋な knowledge delta — 全段落が token に見合う |



**レッドフラグ**（即座にスコア≤5）：

- 「X とは何か」型の基本概念セクション

- 標準操作のステップバイステップチュートリアル

- 一般的ライブラリの使い方の説明

- 汎用ベストプラクティス（「クリーンコード」「エラー処理」など）

- 業界標準用語の定義



**グリーンフラグ**（高い knowledge delta の指標）：

- 非自明な選択の意思決定ツリー（「X が失敗したら Y。理由は Z」）

- エキスパートのみが知るトレードオフ（「A は速いが B はエッジケース C に対応」）

- 実務経験からのエッジケース

- 「非自明な理由により X を NEVER」

- ドメイン固有の思考フレームワーク



**評価の問い**：

1. 各セクションで問う：「Claude は既にこれを知っているか？」

2. 説明があるとき問う：「Claude への説明か、Claude のための説明か？」

3. Expert／Activation／Redundant の段落数を数える



---



### D2: Mindset + Appropriate Procedures（15点）



Skill は**思考パターン**と**必要なドメイン固有手順**の両方をエキスパート水準で伝えるか？



エキスパートと初心者の差は「操作の仕方」ではなく「問題の考え方」である。ただし Claude がドメイン固有手順を持たないとき、思考パターンだけでは足りない。



**重要な区別**：

| 種別 | 例 | 価値 |

|------|---------|-------|

| **Thinking patterns** | "Before designing, ask: What makes this memorable?" | 高 — 意思決定を形作る |

| **Domain-specific procedures** | "OOXML workflow: unpack → edit XML → validate → pack" | 高 — Claude が知らない可能性 |

| **Generic procedures** | "Step 1: Open file, Step 2: Edit, Step 3: Save" | 低 — Claude は既知 |



| スコア | 基準 |

|-------|----------|

| 0-3 | Claude が既知の汎用手順のみ |

| 4-7 | ドメイン手順はあるが思考フレームワーク不足 |

| 8-11 | 思考パターンとドメイン workflow の良いバランス |

| 12-15 | エキスパート級：思考を形作り Claude が知らない手順も提供 |



**価値ある手順の条件**：

- Claude がトレーニングされていない workflow（新ツール、独自システム）

- 非自明な正しい順序（例：「pack の前に validate」）

- 見落としやすい重要ステップ（例：「編集後に数式を再計算 MUST」）

- ドメイン固有シーケンス（例：MCP server の4フェーズ開発）



**冗長な手順の条件**：

- 汎用ファイル操作（open、read、write、save）

- 標準プログラミングパターン（ループ、条件分岐、エラー処理）

- 文書化済みの一般的ライブラリ利用



**エキスパートの思考パターンの例**：

```markdown

Before [action], ask yourself:

- **Purpose**: What problem does this solve? Who uses it?

- **Constraints**: What are the hidden requirements?

- **Differentiation**: What makes this solution memorable?

```



**価値あるドメイン手順の例**：

```markdown

### Redlining Workflow (Claude wouldn't know this sequence)

1. Convert to markdown: `pandoc --track-changes=all`

2. Map text to XML: grep for text in document.xml

3. Implement changes in batches of 3-10

4. Pack and verify: check ALL changes were applied

```



**冗長な汎用手順の例**：

```markdown

Step 1: Open the file

Step 2: Find the section

Step 3: Make the change

Step 4: Save and test

```



**テスト**：

1. Claude に何を考えるべきか伝えるか？（thinking patterns）

2. Claude が知らないことをどう実行するか伝えるか？（domain procedures）



良い Skill は必要に応じて両方を提供する。



---



### D3: Anti-Pattern Quality（15点）



Skill に効果的な NEVER リストがあるか？



**なぜ重要か**：エキスパート知識の半分は「やってはいけないこと」を知ること。シニアデザイナーが白背景の紫グラデーションを見て「AI 生成すぎ」と身構える直感は、数多の地雷を踏んだ結果である。



Claude はこれらの地雷を踏んでいない。Inter の過剰使用や紫グラデが AI 生成のシグネチャであることを知らない。良い Skill は「絶対禁止」を明示する。



| スコア | 基準 |

|-------|----------|

| 0-3 | アンチパターン言及なし |

| 4-7 | 汎用警告（"avoid errors"、"be careful" など） |

| 8-11 | 理由付きの具体的 NEVER リスト |

| 12-15 | WHY 付きエキスパート級アンチパターン — 経験でしか学べないこと |



**エキスパート級アンチパターン**（具体＋理由）：

```markdown

NEVER use generic AI-generated aesthetics like:

- Overused font families (Inter, Roboto, Arial)

- Cliched color schemes (particularly purple gradients on white backgrounds)

- Predictable layouts and component patterns

- Default border-radius on everything

```



**弱いアンチパターン**（曖昧、理由なし）：

```markdown

Avoid making mistakes.

Be careful with edge cases.

Don't write bad code.

```



**テスト**： Would an expert read the anti-pattern list and say "yes, I learned this the hard way"? Or would they say "this is obvious to everyone"?



---



### D4: Specification Compliance — とくに Description（15点）



Skill は公式フォーマット要件に従うか？**description の品質に特に注力。**



| スコア | 基準 |

|-------|----------|

| 0-5 | frontmatter 欠落または不正フォーマット |

| 6-10 | frontmatter はあるが description が曖昧／不完全 |

| 11-13 | 有効な frontmatter、WHAT はあるが WHEN が弱い |

| 14-15 | 完璧：WHAT、WHEN、トリガーキーワードを含む包括的 description |



**frontmatter 要件**：

- `name`: 小文字、英数字＋ハイフンのみ、≤64文字

- `description`: **最重要フィールド** — skill が使われるかを決める



---



**description が最重要である理由**：



```

┌─────────────────────────────────────────────────────────────────────┐

│  SKILL ACTIVATION FLOW                                              │

│                                                                     │

│  User Request → Agent sees ALL skill descriptions → Decides which  │

│                 (only descriptions, not bodies!)     to activate    │

│                                                                     │

│  If description doesn't match → Skill NEVER gets loaded            │

│  If description is vague → Skill might not trigger when it should  │

│  If description lacks keywords → Skill is invisible to the Agent   │

└─────────────────────────────────────────────────────────────────────┘

```



**厳しい真実**：内容が完璧でも description が貧弱な Skill は**無用** — 起動されない。description は Agent に「この状況で使え」と伝える**唯一の機会**。



---



**description は3つの問いに答える**：



1. **WHAT**: この Skill は何をするか？（機能）

2. **WHEN**: どんな状況で使うか？（トリガーシナリオ）

3. **KEYWORDS**: どの語がこの Skill を起動すべきか？（検索可能な語）



**優れた description**（3要素すべて）：

```yaml

**Description:** "Comprehensive document creation, editing, and analysis with support

for tracked changes, comments, formatting preservation, and text extraction.

When Claude needs to work with professional documents (.docx files) for:

(1) Creating new documents, (2) Modifying or editing content,

(3) Working with tracked changes, (4) Adding comments, or any other document tasks"

```



分析：

- WHAT: creation, editing, analysis, tracked changes, comments

- WHEN: "When Claude needs to work with... for: (1)... (2)... (3)..."

- KEYWORDS: .docx files, tracked changes, professional documents



**Poor description** (missing elements):

```yaml

**Description:** "处理文档相关功能"

```



問題点：

- WHAT: vague ("文档相关功能" — what specifically?)

- WHEN: missing (when should Agent use this?)

- KEYWORDS: missing (no ".docx", no specific scenarios)



**別の悪い例**：

```yaml

**Description:** "A helpful skill for various tasks"

```



無用 — Agent はいつ起動すればよいか分からない。



---



**description 品質チェックリスト**：

- [ ] 具体的機能を列挙（「X を手伝う」だけでない）

- [ ] 明示的トリガーシナリオ（"Use when..."、"When user asks for..."）

- [ ] 検索可能キーワード（拡張子、ドメイン用語、動詞）

- [ ] Agent がいつ使うか正確に分かる具体性

- [ ] この skill を MUST で使うシナリオ（"can be used" だけでない）



---



### D5: Progressive Disclosure（15点）



Skill は適切なコンテンツ階層化を実装しているか？



Skill ロードは3層：

```

Layer 1: Metadata (always in memory)

         Only name + description

         ~100 tokens per skill



Layer 2: SKILL.md Body (loaded after triggering)

         Detailed guidelines, code examples, decision trees

         Ideal: < 500 lines



Layer 3: Resources (loaded on demand)

         scripts/, references/, assets/

         No limit

```



| スコア | 基準 |

|-------|----------|

| 0-5 | すべて SKILL.md にダンプ（>500行、構造なし） |

| 6-10 | references があるがロードタイミング不明 |

| 11-13 | MANDATORY トリガー付き良好な階層 |

| 14-15 | 完璧：意思決定ツリー＋明示トリガー＋"Do NOT Load" ガイダンス |



**references ディレクトリがある Skill**では Loading Trigger 品質を確認：



| トリガー品質 | 特徴 |

|-----------------|-----------------|

| 弱い | 末尾に references 列挙、ロードガイダンスなし |

| 並 | トリガーはあるが workflow に埋め込まれていない |

| 良 | workflow ステップに MANDATORY トリガー |

| 優 | シナリオ検出＋条件トリガー＋"Do NOT Load" |



**ロードの問題**：

```

Loading too little ◄─────────────────────────────────► Loading too much

- References sit unused                    - Wastes context space

- Agent doesn't know when to load          - Irrelevant info dilutes key content

- Knowledge is there but never accessed    - Unnecessary token overhead

```



**良いロードトリガー**（workflow 埋め込み）：

```markdown

### Creating New Document



**MANDATORY - READ ENTIRE FILE**: Before proceeding, you MUST read

[`docx-js.md`](docx-js.md) (~500 lines) completely from start to finish.

**NEVER set any range limits when reading this file.**



**Do NOT load** `ooxml.md` or `redlining.md` for this task.

```



**悪いロードトリガー**（列挙のみ）：

```markdown

## References

- docx-js.md - for creating documents

- ooxml.md - for editing

- redlining.md - for tracking changes

```



**単純な Skill**（references なし、<100行）：簡潔さと自己完結で採点。**必須**：references なしの自己完結 skill では、*Do NOT load external files or reference directories* を明示。



---



### D6: Freedom Calibration（15点）



具体性のレベルはタスクの脆弱性に適切か？



タスクごとに制約レベルが異なる。自由度を脆弱性に合わせる問題である。



| スコア | 基準 |

|-------|----------|

| 0-5 | 深刻な不一致（創造タスクに硬直スクリプト、脆弱 ops に曖昧） |

| 6-10 | 部分的に適切、不一致あり |

| 11-13 | ほとんどのシナリオで良好（軽微な不一致） |

| 14-15 | 完璧：エラー時の明確ルールと判断が価値のある柔軟テンプレ |



**自由度スペクトラム**：



| タスク種別 | あるべき姿 | 理由 | 例 Skill |

|-----------|-------------|-----|---------------|

| Creative/Design | 高い自由度 | 複数正解、差別化が価値 | frontend-design |

| Code review | 中程度の自由度 | 原則はあるが判断が必要 | code-review |

| File format operations | 低い自由度 | 1バイト誤りで破損、一貫性が重要 | docx, xlsx, pdf |



**高い自由度**（テキスト指示）：

```markdown

Commit to a BOLD aesthetic direction. Pick an extreme: brutally minimal,

maximalist chaos, retro-futuristic, organic natural...

```



**中程度の自由度**（疑似コードまたはパラメータ化）：

```markdown

Review priority:

1. Security vulnerabilities (must fix)

2. Logic errors (must fix)

3. Performance issues (should fix)

4. Maintainability (optional)

```



**低い自由度**（特定スクリプト、正確な手順）：

```markdown

**MANDATORY**: Use exact script in `scripts/create-doc.py`

Parameters: --title "X" --author "Y"

Do NOT modify the script.

```



**テスト**： Ask "if Agent makes a mistake, what's the consequence?"

- 影響大 → 低い自由度（分散許容小）

- 影響小 → 高い自由度（柔軟パラメータ可）

- 影響中 → 中程度の自由度（原則で制約、判断が鍵）





---



### D7: Pattern Recognition（10点）



Skill は確立された公式パターンに従うか？



17 の公式 Skill を分析し、5つの主要設計パターンを特定した：



| パターン | 〜行数 | 主な特徴 | 例 | 使用タイミング |

|---------|--------|---------------------|---------|-------------|

| **Mindset** | ~50 | 思考>技法、強い NEVER リスト、高い自由度 | frontend-design | センスが要る創造タスク |

| **Navigation** | ~30 | 最小 SKILL.md、サブファイルへルート | internal-comms | 複数の独立シナリオ |

| **Philosophy** | ~150 | 二段階：Philosophy → Express、職人性重視 | canvas-design | 独創性が要るアート／創作 |

| **Process** | ~200 | 段階 workflow、チェックポイント、中程度の自由度 | mcp-builder | 複雑な多段階プロジェクト |

| **Tool** | ~300 | 意思決定ツリー、コード例、低い自由度 | docx, pdf, xlsx | 特定フォーマットの精密操作 |



| スコア | 基準 |

|-------|----------|

| 0-3 | 認識可能パターンなし、カオス構造 |

| 4-6 | パターンに部分的従うが大きな逸脱 |

| 7-8 | 明確なパターン、軽微な逸脱 |

| 9-10 | 適切パターンの巧みな適用 |



**パターン選択ガイド**：



| タスクの特徴 | 推奨パターン |

|---------------------------|---------------------|

| センスと創造性が必要 | Mindset（〜50行） |

| 独創性と職人品質が必要 | Philosophy（〜150行） |

| 複数の独立サブシナリオ | Navigation（〜30行） |

| 複雑な多段階プロジェクト | Process（〜200行） |

| 特定フォーマットの精密操作 | Tool（〜300行） |



---



### D8: Practical Usability（15点）



Agent は実際にこの Skill を効果的に使えるか？



| スコア | 基準 |

|-------|----------|

| 0-5 | 混乱・不完全・矛盾・未検証ガイダンス |

| 6-10 | 使えるが目立つギャップ |

| 11-13 | 一般的ケースへの明確ガイダンス |

| 14-15 | エッジケースとエラー処理を含む包括的カバー |



**確認項目**：

- **Decision trees**: 複数パスシナリオでどの経路を取るか明確か？

- **Code examples**: 実際に動くか？壊れる疑似コードか？

- **Error handling**: 主アプローチ失敗時は？フォールバックは？

- **Edge cases**: 珍しいが現実的なシナリオをカバーしているか？

- **Actionability**: Agent は即実行できるか、自力で解く必要があるか？



**良い usability**（意思決定ツリー＋フォールバック）：

```markdown

| タスク | Primary Tool | Fallback | Fallback を使うとき |

|------|-------------|----------|----------------------|

| Read text | pdftotext | PyMuPDF | Need layout info |

| Extract tables | camelot-py | tabula-py | camelot fails |



**よくある問題**：

- スキャン PDF：pdftotext が空 → 先に OCR

- 暗号化 PDF：権限エラー → パスワード付き PyMuPDF

```



**悪い usability**（曖昧）：

```markdown

Use appropriate tools for PDF processing.

Handle errors properly.

Consider edge cases.

```



---



## 評価時にしてはならないこと



- **NEVER** 見た目がプロ／整形されているだけで高得点を付けない

- **NEVER** token waste を無視 — 冗長段落ごとに減点

- **NEVER** 長さに感銘 — 43行 Skill が 500行を凌ぐことがある

- **NEVER** 意思決定ツリーの頭内テストを飛ばす — 正しい選択に至るか

- **NEVER** 基本説明を「有用な文脈」で許容しない

- **NEVER** アンチパターン欠如を見逃す — NEVER リストなしは大きなギャップ

- **NEVER** 全手順が価値あると仮定 — ドメイン固有と汎用を区別

- **NEVER** description を過小評価 — 貧弱な description = 起動されない

- **NEVER** 「いつ使う」を本文だけに — ロード前に Agent が見るのは description のみ



---



## 評価プロトコル



### ステップ1: 初回パス — Knowledge Delta スキャン



SKILL.md を全文読み、各セクションで次を問う：

> 「Claude は既にこれを知っているか？」



Mark each section as:

- **[E] Expert**: Claude が本当に知らない — 付加価値

- **[A] Activation**: Claude は知るが短いリマインダーが有用 — 許容

- **[R] Redundant**: Claude が確実に知っている — 削除すべき



おおよその比率 E:A:R を算出

- 良い Skill：Expert >70%、Activation <20%、Redundant <10%

- 並 Skill：Expert 40–70%、Activation 高

- 悪い Skill：Expert <40%、Redundant 高



### ステップ2: 構造分析



```

[ ] frontmatter の妥当性確認

[ ] SKILL.md の総行数カウント

[ ] 全 reference ファイルとサイズ列挙

[ ] Skill が従うパターンの特定

[ ] ロードトリガー確認（references がある場合）

```



### ステップ3: 各次元を採点



8次元それぞれについて：

1. 具体的根拠を探す（該当行を引用）

2. 一行根拠付きでスコア付与

3. 満点未満なら具体的改善を記録



### ステップ4: 合計とグレード算出



```

Total = D1 + D2 + D3 + D4 + D5 + D6 + D7 + D8

Max = 120 points

```



**グレード尺度**（パーセントベース）：

| グレード | パーセント | 意味 |

|-------|------------|---------|

| A | 90%+ (108+) | 優秀 — 本番投入可能なエキスパート Skill |

| B | 80-89% (96-107) | 良好 — 軽微な改善が必要 |

| C | 70-79% (84-95) | 妥当 — 改善経路が明確 |

| D | 60-69% (72-83) | 平均以下 — 重大な問題 |

| F | <60% (<72) | 不良 — 根本的再設計が必要 |



### ステップ5: レポート生成



```markdown

# Skill 評価レポート: [Skill Name]



## サマリー

- **合計スコア**: X/120 (X%)

- **グレード**: [A/B/C/D/F]

- **パターン**: [Mindset/Navigation/Philosophy/Process/Tool]

- **Knowledge 比率**: E:A:R = X:Y:Z

- **判定**: [一文評価]



## Dimension Scores



| 次元 | スコア | 満点 | メモ |

|-----------|-------|-----|-------|

| D1: Knowledge Delta | X | 20 | |

| D2: Mindset vs Mechanics | X | 15 | |

| D3: Anti-Pattern Quality | X | 15 | |

| D4: Specification Compliance | X | 15 | |

| D5: Progressive Disclosure | X | 15 | |

| D6: Freedom Calibration | X | 15 | |

| D7: Pattern Recognition | X | 10 | |

| D8: Practical Usability | X | 15 | |



## 重大な問題

[Skill の有効性に大きく影響する必須修正を列挙]



## 上位3つの改善

1. [Highest impact improvement with specific guidance]

2. [Second priority improvement]

3. [Third priority improvement]



## 詳細分析

[For each dimension scoring below 80%, provide:

- What's missing or problematic

- Specific examples from the Skill

- Concrete suggestions for improvement]

```



---



## よくある失敗パターン



### パターン1: The Tutorial

```

症状：PDF とは、Python の仕組み、基本ライブラリ利用を説明

根本原因：Skill はモデルを「教える」ものと著者が思う

修正：Claude は既知。基本説明をすべて削除。

     エキスパートの判断・トレードオフ・アンチパターンに集中。

```



### パターン2: The Dump

```

症状：SKILL.md が800行超で全含有

根本原因：Progressive disclosure 設計なし

修正：コアルーティングと意思決定ツリーを SKILL.md に（理想<300行）

     詳細は references/ にオンデマンドロード

```



### パターン3: The Orphan References

```

症状：references はあるがファイルがロードされない

根本原因：明示的ロードトリガーなし

修正：workflow 分岐点に "MANDATORY - READ ENTIRE FILE" を追加

     過剰ロード防止に "Do NOT Load" を追加

```



### パターン4: The Checkbox Procedure

```

症状：Step 1, 2, 3… 機械的手順

根本原因：手順で考え、思考フレームワークで考えない

修正：「X の前に自問…」へ変換

     操作列ではなく意思決定原則に集中

```



### パターン5: The Vague Warning

```

症状："Be careful"、"avoid errors"、"consider edge cases"

根本原因：問題は分かるが具体を言語化していない

修正：具体例と非自明な理由付き NEVER リスト

     "NEVER use X because [経験で学ぶ具体的問題]"

```



### パターン6: The Invisible Skill

```

症状：内容は優秀だが起動されにくい

根本原因：description が曖昧、キーワード欠如、トリガーシナリオ不足

修正：description は WHAT、WHEN、KEYWORDS に答える

     "Use when..." + 具体シナリオ + 検索可能語



修正例：

BAD:  "Helps with document tasks"

GOOD: "Create, edit, and analyze .docx files. Use when working with

       Word documents, tracked changes, or professional document formatting."

```



### パターン7: The Wrong Location

```

症状：「いつ使う」が本文にあり description にない

根本原因：3層ロードの誤解

修正：トリガー情報をすべて description へ

     本文はトリガー決定後にのみロード

```



### パターン8: The Over-Engineered

```

症状：README、CHANGELOG、INSTALLATION_GUIDE、CONTRIBUTING

根本原因：Skill をソフトウェアプロジェクト扱い

修正：補助ファイルをすべて削除。Agent がタスクに必要なものだけ。

     Skill 自体のドキュメントは不要。

```



### パターン9: The Freedom Mismatch

```

症状：創造タスクに硬直スクリプト、脆弱操作に曖昧ガイダンス

根本原因：タスク脆弱性を考慮していない

修正：創造には高い自由度（原則、手順ではない）

     脆弱には低い自由度（正確なスクリプト、パラメータなし）

```



---



## クイックリファレンス・チェックリスト



```

┌─────────────────────────────────────────────────────────────────────────┐

│  SKILL EVALUATION QUICK CHECK                                           │

├─────────────────────────────────────────────────────────────────────────┤

│                                                                         │

│  KNOWLEDGE DELTA (most important):                                      │

│    [ ] No "What is X" explanations for basic concepts                   │

│    [ ] No step-by-step tutorials for standard operations                │

│    [ ] Has decision trees for non-obvious choices                       │

│    [ ] Has trade-offs only experts would know                           │

│    [ ] Has edge cases from real-world experience                        │

│                                                                         │

│  MINDSET + PROCEDURES:                                                  │

│    [ ] Transfers thinking patterns (how to think about problems)        │

│    [ ] Has "Before doing X, ask yourself..." frameworks                 │

│    [ ] Includes domain-specific procedures Claude wouldn't know         │

│    [ ] Distinguishes valuable procedures from generic ones              │

│                                                                         │

│  ANTI-PATTERNS:                                                         │

│    [ ] Has explicit NEVER list                                          │

│    [ ] Anti-patterns are specific, not vague                            │

│    [ ] Includes WHY (non-obvious reasons)                               │

│                                                                         │

│  SPECIFICATION (description is critical!):                              │

│    [ ] Valid YAML frontmatter                                           │

│    [ ] name: lowercase, ≤64 chars                                       │

│    [ ] description answers: WHAT does it do?                            │

│    [ ] description answers: WHEN should it be used?                     │

│    [ ] description contains trigger KEYWORDS                            │

│    [ ] description is specific enough for Agent to know when to use     │

│                                                                         │

│  STRUCTURE:                                                             │

│    [ ] SKILL.md < 500 lines (ideal < 300)                               │

│    [ ] Heavy content in references/                                     │

│    [ ] Loading triggers embedded in workflow                            │

│    [ ] Has "Do NOT Load" for preventing over-loading                    │

│                                                                         │

│  FREEDOM:                                                               │

│    [ ] Creative tasks → High freedom (principles)                       │

│    [ ] Fragile operations → Low freedom (exact scripts)                 │

│                                                                         │

│  USABILITY:                                                             │

│    [ ] Decision trees for multi-path scenarios                          │

│    [ ] Working code examples                                            │

│    [ ] Error handling and fallbacks                                     │

│    [ ] Edge cases covered                                               │

│                                                                         │

└─────────────────────────────────────────────────────────────────────────┘

```



---



## メタ質問



どの Skill を評価するときも、この根本問いに戻る：



> **「このドメインのエキスパートがこの Skill を見て、**

> **『はい、多年かけて学んだ知識が捉えられている』と言うか？****



答えが yes → Skill は真の価値がある。

答えが no → Claude の既知を圧縮しているだけ。



最高の Skill は**圧縮されたエキスパートの脳** — デザイナー10年の美的蓄積を43行に、文書エキスパートの運用経験を200行の意思決定ツリーに圧縮する。



圧縮対象は Claude が持たないものでなければならない。さもなくばゴミ圧縮である。



---



## 自己評価ノート



本 Skill（skill-judge）自体が評価に合格すべきである：



- **Knowledge Delta**: Claude が単独では生成しない具体的評価基準を提供

- **Mindset**: チェック項目だけでなく Skill 品質の考え方を形作る

- **Anti-Patterns**: 具体的禁止の「評価時にしてはならないこと」セクション

- **Specification**: 包括的 description を伴う有効な frontmatter

- **Progressive Disclosure**: 自己完結、外部 references 不要

- **Freedom**: 評価タスクに適した中程度の自由度

- **Pattern**: 意思決定フレームワーク付き Tool パターン

- **Usability**: 明確なプロトコル、レポートテンプレ、クイックリファレンス







キャリブレーション演習として、本 Skill を自身に対して評価する。



## References



**必須 — 自己完結ディレクティブ**：

これは自己完結型 skill。本ワークフロー実行中に明示的に作成されない限り、外部ファイルや参照ディレクトリをロードしないこと。



### グレードインフレ制限と Token 検証コマンド

1. **グレードインフレ制限**: 高い採点基準を保つため、Grade A は108点以上の skill に厳密に限定する。

2. **Token 検証**: 推奨 token 境界内に収まるよう、`wc -w SKILL.md` などの語数カウント実行を提案する。


## Memory Sync

主要な技術的知見・アーキテクチャ決定・コードリファクタリング・リスク評価を完了したら、ローカルメモリキャプチャを**必ず**起動すること。

1. 最終サマリーまたは成果物をプロジェクトディレクトリに Markdown ファイルとして保存する。
2. キャプチャスクリプトを呼び出す：
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. 新たな学び・ポリシー・技術スニペットが適切なローカルストレージ（OKF または ChromaDB）へ自動ルーティングされる。
