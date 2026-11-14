---
name: pricing-strategy
description: "価格モデル、競合価格分析、支払意思の推定、価格弾力性を含む価格戦略を分析・設計する。価格設定、価格モデルの評価、価格変更の準備、Freemium と有料の比較時に使う。"
---

## 価格戦略

価値提供、競合ポジション、支払意思に根ざした価格戦略を設計する。

### コンテキスト

**$ARGUMENTS** 向けの価格戦略を策定している。

ユーザーがファイル（競合価格、アンケートデータ、財務モデル、利用データ）を渡した場合は先に読む。必要なら競合価格調査にウェブ検索を使う。

### 手順

1. **提供価値の理解**:
   - コアバリュープロポジションは何か？
   - 顧客の代替手段（とそのコスト）は？
   - プロダクトがもたらす定量化可能な成果は？（時間短縮、収益増、コスト削減）
   - その価値に基づく支払意思は？

2. **価格モデルの評価** — 最適なものを推奨：

   | Model | Best For | Example |
   |---|---|---|
   | **Flat-rate** | シンプルなプロダクト、予測可能コスト | Basecamp ($99/mo flat) |
   | **Per-seat** | コラボツール、チームプロダクト | Slack, Figma |
   | **Usage-based** | インフラ、API プロダクト | AWS, Twilio |
   | **Tiered** | 明確なユーザーセグメントがあるプロダクト | 多くの SaaS (Free/Pro/Enterprise) |
   | **Freemium** | バイラル／ネットワーク効果があるプロダクト | Spotify, Notion |
   | **Freemium + usage** | プラットフォームプロダクト | Vercel, OpenAI API |
   | **Value-based** | インパクトの大きいエンタープライズツール | Salesforce, Palantir |

3. **競合価格の分析**:
   - 競合の価格帯と含まれる内容をマップする
   - 自プロダクトの位置づけ（プレミアム、ミッドマーケット、バジェット）
   - 価格のギャップや機会を特定する
   - 業界の価格慣行に注意する

4. **価格構造の設計**:
   - **Tiers**: 明確な差別化で2–4段階を定義
   - **Feature gating**: 各段階にどの機能を入れるか？（恣意的な上限ではなく価値メトリクスを使う）
   - **Value metric**: 何の単位で課金するか？（users、events、storage、API calls）
   - **Anchor pricing**: 最も人気の段階が当然の選択に感じるよう設定
   - **Annual discount**: 月額から通常15–20%オフ

5. **価格感度の推定**:
   - Van Westendorp Price Sensitivity Meter（アンケートデータがある場合）:
     - Too cheap → 品質への懸念
     - Cheap → 良い価値
     - Expensive → 躊躇し始める
     - Too expensive → 買わない
   - または競合価格と提供価値から推定

6. **価格実験の計画**:
   - 価格ページの A/B テスト（異なる価格、段階名、機能バンドル）
   - 支払意思を試す創業者主導の営業会話
   - 異なる価格アンカーでのランディングページテスト
   - 価格帯別コンバージョン率のコホート分析

7. **価格推奨の出力**:
   ```
   Recommended Model: [Model type]
   Value Metric: [What you charge on]

   | Tier | Price | Target Segment | Key Features | Positioning |
   |---|---|---|---|---|

   Key Assumptions:
   - [Assumption] → [How to test]

   Risks:
   - [Risk] → [Mitigation]
   ```

段階的に考える。Markdown として保存。ローンチ前に検証が必要な前提にはフラグを立てる。

---

### さらに読む

- [Product Pricing Strategies 101](https://www.productcompass.pm/p/product-pricing-strategies-101)
- [The AI Product Pricing Masterclass: OpenAI Product Lead on Why SaaS Pricing Fails in AI (and How to Fix It)](https://www.productcompass.pm/p/ai-product-pricing) (video course)