---
name: analyze-feature-requests
description: "テーマ、戦略整合、インパクト、工数、リスクで機能要望リストを分析・優先順位付け。顧客機能要望のレビュー、バックログのトリアージ、優先順位決定時に使用。"
---

## 機能要望の分析

プロダクト目標に照らして顧客の機能要望を分類・評価・優先順位付けする。

### コンテキスト

**$ARGUMENTS** の機能要望を分析する。

ユーザーがファイル（機能要望のスプレッドシート、CSV、文書）を提供した場合は直接読み分析する。構造化データならサマリー表の作成を検討。

### ドメインコンテキスト

顧客にソリューション設計をさせない。**機能ではなく機会（問題）** を優先。顧客報告の問題評価には **Opportunity Score**（Dan Olsen）を使用：Opportunity Score = Importance × (1 − Satisfaction)、0〜1 に正規化。詳細とテンプレートは `prioritization-frameworks` スキル参照。

### 手順

ユーザーがプロダクト目標と機能要望を説明する。次のステップで進める：

1. **目標の理解**：優先順位付けの指針となるプロダクト目的と望む成果を確認。

2. **テーマへの分類**：関連要望をグループ化し各テーマに名前を付ける。

3. **戦略整合の評価**：各テーマが述べた目標にどれだけ整合するか評価。

4. **上位 3 機能の優先順位付け**、基準：
   - **Impact**: 顧客価値と影響を受けるユーザー数
   - **Effort**: 必要な開発・デザインリソース
   - **Risk**: 技術・市場の不確実性
   - **Strategic alignment**: プロダクトビジョン・目標との適合

5. **各上位機能について**提供：
   - 根拠（顧客ニーズ、戦略整合）
   - 検討すべき代替ソリューション
   - 高リスク仮説
   - 最小工数で仮説を検証する方法

段階的に考える。Markdown で保存するか構造化出力ドキュメントを作成。

---

### 参考資料

- [Kano Model: How to Delight Your Customers Without Becoming a Feature Factory](https://www.productcompass.pm/p/kano-model-how-to-delight-your-customers)
- [Continuous Product Discovery Masterclass (CPDM)](https://www.productcompass.pm/p/cpdm) (video course)