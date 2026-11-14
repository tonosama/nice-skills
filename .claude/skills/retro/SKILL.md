---
name: retro
description: "構造化されたスプリントレトロスペクティブを進行 — うまくいったこと、いかなかったこと、オーナーと期限付きの優先アクション項目。レトロ実施、スプリント振り返り、フィードバックからのアクション作成、効果的なレトロの学習時に使う。"
---

## Sprint Retrospective Facilitator

洞察を引き出し、実行可能な改善を生む構造化レトロを実施する。

### Context

**$ARGUMENTS** のレトロを進行している。

ユーザーがファイル（スプリントデータ、ベロシティチャート、チームフィードバック、過去レトロメモ）を提供したら先に読む。

### Instructions

1. **Choose a retro format** 文脈に応じて（またはユーザーに選ばせる）:

   **Format A — Start / Stop / Continue**:
   - **Start**: 何を始めるべきか？
   - **Stop**: 何をやめるべきか？
   - **Continue**: うまくいっていること、続けるべきことは？

   **Format B — 4Ls (Liked / Learned / Lacked / Longed For)**:
   - **Liked**: チームが楽しんだことは？
   - **Learned**: 得た新しい知識は？
   - **Lacked**: 何が欠けていたか？
   - **Longed For**: 何があればよかったか？

   **Format C — Sailboat**:
   - **Wind (propels us)**: 前に進める力は？
   - **Anchor (holds us back)**: 遅らせるものは？
   - **Rocks (risks)**: 先にある危険は？
   - **Island (goal)**: 向かう先は？

2. **If the user provides raw feedback**（付箋、アンケート、Slack など）:
   - 類似項目をテーマにグループ化
   - 最も頻出トピックを特定
   - 感情パターン（フラストレーション、エネルギー、混乱）に注意

   入力がまだない場合は、議論前に匿名で集める。最初や最シニアの声が議題を決めないように: ムード用ワードクラウドまたは1〜5評価、うまくいった/いかなかったことを書いてアップvote。live-polls と live-qa スキルが両方をカバー。

3. **Analyze the sprint performance**:
   - スプリント目標: 達成したか？
   - ベロシティ vs コミット（過剰/過少コミット？）
   - 遭遇したブロッカーと解決方法
   - コラボパターン（うまくいったこと、いかなかったこと）

4. **Generate prioritized action items**:

   | Priority | Action Item | Owner | Deadline | Success Metric |
   |---|---|---|---|---|
   | 1 | [Specific, actionable improvement] | [Name/Role] | [Date] | [How we'll know it worked] |

   - アクションは2〜3件に限定（それ以上は実行されにくい）
   - 各項目は具体、割り当て可能、測定可能
   - 過去レトロアクションがあれば参照 — 完了したか？

5. **Create the retro summary**:
   ```
   ## Sprint [X] Retrospective — [Date]

   ### Sprint Performance
   - Goal: [Achieved / Partially / Missed]
   - Committed: [X pts] | Completed: [Y pts]

   ### Key Themes
   1. [Theme] — [summary]

   ### Action Items
   1. [Action] — [Owner] — [By date]

   ### Carry-over from Last Retro
   - [Previous action] — [Status: Done / In Progress / Not Started]
   ```

Markdown として保存。トーンは建設的 — 目的は改善であり非難ではない。