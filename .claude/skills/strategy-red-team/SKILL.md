---
name: strategy-red-team
description: "PRD、ロードマップ、戦略をレッドチーム — 現実が先に攻撃する前に荷重を支える仮定を攻撃。各主張をスティールマンしてから攻撃し、失敗モードを impact × likelihood × cheapness-to-test で順位付け。各項目に最安テストと kill criteria を返す。計画ストレステスト、戦略プレッシャーテスト、仮定への挑戦、経営レビュー前準備時に使う。"
---

# Strategy Red-Team: Attack the Assumptions Before Reality Does

## Purpose

**$ARGUMENTS** を鋭く公正な対抗者としてレビューする。多くの計画は丁寧なフィードバックしか生き残らない。本スキルは計画を破綻させうる荷重仮定を見つけ、正直に攻撃し、各項目について今週得る証拠、kill criteria、最安テストを返す。

## Context

レッドチームはプレモーテムではない。プレモーテムは計画が既に失敗したと想像し理由を語る。レッドチームは荷重仮定とロジックを**今**攻撃し、最も安いものを試す時間がまだある。自信だけでなく判断を高める。

目的はより鋭い決定であり、長いリスクリストではない。テスト付きの実 kill-assumption 5つが、汎用リスク20より価値がある。

## Instructions

1. **Extract every claim.** 計画を読み、真だと断言する内容を列挙 — ユーザー、市場、制約、メカニズム、タイムラインについて。荷重（誤れば計画が死ぬ）と装飾的を分ける。攻撃する価値があるのは荷重のみ。

2. **Steelman, then attack.** 各荷重主張について、まず真である最強の理由を述べる。それを攻撃する — ストローマンではない。弱い版への攻撃は無価値。

3. **Write each failure mode as "Fails if ___."** 具体で反証可能に。「Fails if activation isn't actually the constraint」は「execution risk」より良い。

4. **Rank by (impact if wrong) × (likelihood wrong) × (cheapness to test).** リスト上位は*今週*試すもの — インパクト大、誤っている可能性、安く確認可能。順位を表面化し、埋めない。

5. **Self-refute, don't fabricate.** 計画が既に反証を示していない限りデフォルトは「このリスクは現実的」。ただし主張が本当に筋が通っていればはっきりそう言う — 疑いを製造するレッドチームはゴム印と同様に無用。計画にない弱点は捏造しない。

6. **For each surviving kill-assumption, give the operator something to do:**
   - **Fails if:** 計画を壊す正確な条件
   - **Evidence to get this week:** 安く確認または殺す具体的データ、クエリ、会話
   - **Kill criterion:** 停止または方針変更する閾値
   - **Cheapest test:** 信念を動かす最小実験

7. **Optional cross-model mode.** ユーザーが第二意見を求め、別モデル（Codex、Gemini、第二 Claude）に届くなら同計画を通し、両者の不一致をフラグ — モデルファミリーごとに見落とす点が異なる。デフォルトは単一モデル。依頼時以外は摩擦を足さない。

8. **Structure the output (make it screenshot-native):**

   ```
   ## Red-Team: [plan in one line]

   ### Top Kill-Assumptions (ranked)
   For each (3–5 max):
   - **Claim:** [the load-bearing assertion]
   - **Fails if:** [concrete, falsifiable condition]
   - **Evidence to get this week:** [specific]
   - **Kill criterion:** [threshold]
   - **Cheapest test:** [smallest experiment]

   ### What's Well-Reasoned
   [State explicitly what holds up — and why. Don't manufacture doubt.]

   ### What I Couldn't Assess
   [Gaps where the plan didn't give enough to judge.]
   ```

## Notes

- ストローマン禁止 — スティールマンを攻撃するか攻撃しない。
- 汎用リスクリスト禁止 — 各項目は*この*計画に固有。
- 捏造禁止 — 健全ならそう言う。
- 容赦なく順位付け — 最安の高インパクトテストが要点。
- 感情的役割は間違った賭けに自信を持って出荷する恐れからの解放。恐れることだけでなく*何をするか*で締める。

---

### Further Reading

- [Assumption Prioritization Canvas: How to Identify And Test The Right Assumptions](https://www.productcompass.pm/p/assumption-prioritization-canvas)
- [How to Manage Risks as a Product Manager](https://www.productcompass.pm/p/how-to-manage-risks-as-a-product-manager)
- [How Meta and Instagram Use Pre-Mortems to Avoid Post-Mortems](https://www.productcompass.pm/p/how-to-run-pre-mortem-template)