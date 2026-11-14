---
name: prioritize-assumptions
description: "Impact × Risk マトリクスで前提を優先順位付けし、それぞれに実験を提案する。前提リストのトリアージ、最初に何を検証するかの決定、前提優先度キャンバスの適用時に使う。"
---

## 前提の優先順位付け

Impact × Risk マトリクスで前提をトリアージし、狙いを絞った実験を提案する。

### コンテキスト

**$ARGUMENTS** の前提優先順位付けを支援している。

ユーザーが前提やリサーチデータを含むファイルを渡した場合は、先に読む。

### ドメインコンテキスト

**ICE** は前提の優先順位付けに有効: Impact（Opportunity Score × # Customers）× Confidence（1–10）× Ease（1–10）。Opportunity Score = Importance × (1 − Satisfaction)、0–1に正規化（Dan Olsen）。**RICE** は Impact を Reach × Impact に分離: (R × I × C) / E。完全な式とテンプレートは `prioritization-frameworks` スキルを参照。

### 手順

ユーザーが優先順位付けする前提リストを提供する。次のフレームワークを適用する。

1. **各前提**について2次元を評価する：
   - **Impact**: この前提を検証することで生じる価値と、影響を受ける顧客数（ICE では Impact = Opportunity Score × # Customers）
   - **Risk**: (1 - Confidence) × Effort として定義

2. **Impact × Risk マトリクス**で各前提を分類する：
   - **Low Impact, Low Risk** → 高優先度の前提が片付くまで検証は延期
   - **High Impact, Low Risk** → 実装へ進む（低リスク・高リターン）
   - **Low Impact, High Risk** → アイデアを拒否（投資に見合わない）
   - **High Impact, High Risk** → 検証用の実験を設計

3. **検証が必要な各前提**について、次を満たす実験を提案する：
   - 最小の努力で検証学習を最大化
   - 意見ではなく実際の行動を測定
   - 明確な成功メトリクスと閾値がある

4. **結果**を優先度付きマトリクスまたは表で提示する。

段階的に考える。出力が大きければ Markdown として保存。

---

### さらに読む

- [Assumption Prioritization Canvas: How to Identify And Test The Right Assumptions](https://www.productcompass.pm/p/assumption-prioritization-canvas)
- [Continuous Product Discovery Masterclass (CPDM)](https://www.productcompass.pm/p/cpdm) (video course)