---
name: prioritization-frameworks
description: "9 つの優先付けフレームワークの参照ガイド — 式、使い分け、テンプレート（RICE、ICE、Kano、MoSCoW、Opportunity Score など）。優先付け手法の選択、RICE vs ICE の比較、各アプローチの学習時に使う。"
---

## Prioritization Frameworks Reference

文脈に合った優先付けフレームワークを選び適用するための参照ガイド。

### Core Principle

顧客に解決策を設計させない。**機能ではなく問題（機会）**を優先する。

グループが項目を採点・並べるときは、議論前に独立して票を集め、最シニアの意見が全員をアンカーしないようにする（dot voting と confidence vote は live-polls スキル）。

### Opportunity Score (Dan Olsen, *The Lean Product Playbook*)

顧客問題の優先付けに推奨されるフレームワーク。

各ニーズについて顧客に **Importance** と **Satisfaction** を調査（0〜1 に正規化）。

関連する3式:
- **Current value** = Importance × Satisfaction
- **Opportunity Score** = Importance × (1 − Satisfaction)
- **Customer value created** = Importance × (S2 − S1)、S1 = 前の満足度、S2 = 後の満足度

Importance 高 + Satisfaction 低 = 最高 Opportunity Score = 最良の機会。Importance vs Satisfaction チャートの左上象限がスイートスポット。解決策ではなく顧客問題を優先。

### ICE Framework

イニシアティブとアイデアの優先付けに有用。価値だけでなくリスクと経済性も考慮。

- **I** (Impact) = Opportunity Score × 影響を受ける顧客数
- **C** (Confidence) = どれだけ確信があるか（1-10）。リスクを反映。
- **E** (Ease) = 実装の容易さ（1-10）。経済要因を反映。

**Score** = I × C × E。高いほど先に優先。

### RICE Framework

ICE の Impact を2因子に分割。より細かい粒度が必要な大きなチーム向け。

- **R** (Reach) = 影響を受ける顧客数
- **I** (Impact) = Opportunity Score（顧客あたりの価値）
- **C** (Confidence) = 確信度（0-100%）
- **E** (Effort) = 実装工数（人月）

**Score** = (R × I × C) / E

### 9 Frameworks Overview

| Framework | Best For | Key Insight |
|-----------|----------|-------------|
| Eisenhower Matrix | 個人タスク | 緊急 vs 重要 — 個人 PM タスク管理向け |
| Impact vs Effort | タスク/イニシアティブ | シンプル 2×2 — 迅速トリアージ。戦略判断の厳密さには不向き |
| Risk vs Reward | イニシアティブ | Impact vs Effort だが不確実性を考慮 |
| **Opportunity Score** | 顧客問題 | **推奨。** Importance × (1 − Satisfaction)。0〜1 正規化。 |
| Kano Model | 期待の理解 | Must-be、Performance、Attractive、Indifferent、Reverse。優先付けより理解向け。 |
| Weighted Decision Matrix | 多因子決定 | 基準に重み、各オプションを採点。ステークホルダー合意に有用。 |
| **ICE** | アイデア/イニシアティブ | Impact × Confidence × Ease。迅速優先付けに推奨。 |
| **RICE** | 大規模アイデア | (Reach × Impact × Confidence) / Effort。ICE に Reach を追加。 |
| MoSCoW | 要件 | Must/Should/Could/Won't。注意: プロジェクト管理起源。 |

### Templates

- [Opportunity Score intro (PDF)](https://drive.google.com/file/d/1ENbYPmk1i1AKO7UnfyTuULL5GucTVufW/view)
- [Importance vs Satisfaction Template — Dan Olsen (Google Slides)](https://docs.google.com/presentation/d/1jg-LuF_3QHsf6f1nE1f98i4C0aulnRNMOO1jftgti8M/edit#slide=id.g796641d975_0_3)
- [ICE Template (Google Sheets)](https://docs.google.com/spreadsheets/d/1LUfnsPolhZgm7X2oij-7EUe0CJT-Dwr-/edit?usp=share_link&ouid=111307342557889008106&rtpof=true&sd=true)
- [RICE Template (Google Sheets)](https://docs.google.com/spreadsheets/d/1S-6QpyOz5MCrV7B67LUWdZkAzn38Eahv/edit?usp=sharing&ouid=111307342557889008106&rtpof=true&sd=true)

---

### Further Reading

- [The Product Management Frameworks Compendium + Templates](https://www.productcompass.pm/p/the-product-frameworks-compendium)
- [Kano Model: How to Delight Your Customers Without Becoming a Feature Factory](https://www.productcompass.pm/p/kano-model-how-to-delight-your-customers)
- [Continuous Product Discovery Masterclass (CPDM)](https://www.productcompass.pm/p/cpdm) (video course)