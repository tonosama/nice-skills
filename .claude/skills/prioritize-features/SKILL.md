---
name: prioritize-features
description: "影響・工数・リスク・戦略整合性に基づき機能アイデアのバックログを優先順位付けし、上位5件を推奨する。機能バックログの優先付け、スコープ決定、プロダクトアイデアのランキング時に使う。"
---

## 機能バックログの優先順位付け

機能アイデアのバックログを評価・ランク付けし、取り組むべき上位5件を特定する。

### コンテキスト

**$ARGUMENTS** の機能優先順位付けを支援している。

ユーザーがファイル（スプレッドシート、バックログ、機会評価）を渡した場合は、直接読み分析する。

### ドメインコンテキスト

フレームワーク選択の指針は `prioritization-frameworks` スキルを参照。主な推奨：

**Opportunity Score**（Dan Olsen, *The Lean Product Playbook*）は顧客課題の評価に推奨: Opportunity Score = Importance × (1 − Satisfaction)、0–1に正規化。高 Importance + 低 Satisfaction = 最良の機会。**課題（機会）**を優先し、ソリューションではない。

**ICE** はイニシアティブの迅速なスコアリングに推奨: Impact（Opportunity Score × # Customers）× Confidence × Ease。**RICE** は大規模チーム向けに Reach を別因子として追加。

### 手順

ユーザーがプロダクト目的、望む成果、機能アイデアを説明する。次のステップを順に進める。

1. **優先度の理解**: プロダクト目的と成功メトリクスを確認する。

2. **各機能**を次で評価する：
   - **Impact**: 望む成果にどれだけ効くか？顧客データがあれば Opportunity Score を考慮。
   - **Effort**: 開発・デザイン・調整にどれだけ必要か？
   - **Risk**: 不確実性はどれだけあるか？検証すべき前提は何か？
   - **Strategic alignment**: プロダクトビジョンと現行目標にどれだけ合うか？

3. **上位5機能**を推奨する：
   - 明確なランキング（1–5）
   - 各選択の簡潔な根拠
   - 考慮した主要トレードオフ
   - 下げたものとその理由

4. 役立てば**優先順位テーブル**として提示する。

段階的に考える。出力が大きければ Markdown として保存。

---

### さらに読む

- [Kano Model: How to Delight Your Customers Without Becoming a Feature Factory](https://www.productcompass.pm/p/kano-model-how-to-delight-your-customers)
- [The Product Management Frameworks Compendium + Templates](https://www.productcompass.pm/p/the-product-frameworks-compendium)
- [Continuous Product Discovery Masterclass (CPDM)](https://www.productcompass.pm/p/cpdm) (video course)