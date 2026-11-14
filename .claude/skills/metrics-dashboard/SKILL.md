---
name: metrics-dashboard
description: "主要メトリクス、データソース、可視化タイプ、アラート閾値を含むプロダクトメトリクスダッシュボードを定義・設計する。メトリクスダッシュボード作成、KPI 定義、プロダクト分析のセットアップ、データ監視計画の構築時に使う。"
---

## プロダクトメトリクスダッシュボード

適切なメトリクス、可視化、アラート閾値を備えた包括的なプロダクトメトリクスダッシュボードを設計する。

### コンテキスト

**$ARGUMENTS** 向けのメトリクスダッシュボードを設計している。

ユーザーがファイル（既存ダッシュボード、分析データ、OKR、戦略ドキュメント）を渡した場合は、先に読む。

### ドメインコンテキスト

**Metrics vs KPIs vs NSM**: Metrics = 測定可能なすべて。KPIs = 長期にわたって追う少数の重要定量メトリクス。North Star Metric = 事業成功の先行指標となる単一の顧客中心 KPI。

**良いメトリクスの4基準**（Ben Yoskovitz, *Lean Analytics*）: (1) Understandable — 共通言語を作る。(2) Comparative — スナップショットではなく時系列。(3) Ratio or Rate — 総数より示唆に富む。(4) Behavior-changing — 黄金律：「行動を変えないメトリクスは悪いメトリクス」。

**8種類のメトリクス**: Vanity vs Actionable（行動を変えるのは actionable のみ）、Qualitative vs Quantitative（WHAT vs WHY — 両方必要；顧客との対話は止めない）、Exploratory vs Reporting（データ探索で予期しない洞察）、Lagging vs Leading（先行指標で学習サイクルを速く、例：顧客苦情は離脱の先行指標）。

**5つのアクションステップ**: (1) 4基準でメトリクスを監査。(2) ダッシュボード更新 — 主要メトリクスがすべて良いものであること。(3) vanity メトリクスを特定 — 使い方に注意。(4) leading / lagging を分類。(5) 1つの問題を選びデータを深掘り。

ケーススタディと詳細：[Are You Tracking the Right Metrics?](https://www.productcompass.pm/p/are-you-tracking-the-right-metrics) by Ben Yoskovitz

### 手順

1. **メトリクスフレームワークの特定** — レイヤーで整理する：

   **North Star Metric**: コア価値提供を最もよく捉える単一メトリクス

   **Input Metrics**（3–5）: North Star を動かすレバー

   **Health Metrics**: プロダクト全体の健全性を守るガードレール

   **Business Metrics**: 収益、コスト、ユニットエコノミクス

2. **各メトリクスについて定義する**：

   | Metric | Definition | Data Source | Visualization | Target | Alert Threshold |
   |---|---|---|---|---|---|
   | [Name] | [Exact calculation: numerator/denominator, time window] | [Where the data comes from] | [Line chart / Bar / Number / Funnel] | [Goal value] | [When to trigger an alert] |

3. **ダッシュボードレイアウトの設計**：

   ```
   ┌─────────────────────────────────────────────┐
   │  NORTH STAR: [Metric] — [Current Value]     │
   │  Trend: [↑/↓ X% vs last period]             │
   ├──────────────────┬──────────────────────────┤
   │  Input Metric 1  │  Input Metric 2          │
   │  [Sparkline]     │  [Sparkline]             │
   ├──────────────────┼──────────────────────────┤
   │  Input Metric 3  │  Input Metric 4          │
   │  [Sparkline]     │  [Sparkline]             │
   ├──────────────────┴──────────────────────────┤
   │  HEALTH: [Latency] [Error Rate] [NPS]       │
   ├─────────────────────────────────────────────┤
   │  BUSINESS: [MRR] [CAC] [LTV] [Churn]        │
   └─────────────────────────────────────────────┘
   ```

4. **レビュー頻度の設定**：
   - **Daily**: 運用健全性（エラー、レイテンシ、重要フロー）
   - **Weekly**: Input メトリクスとエンゲージメント傾向
   - **Monthly**: North Star、ビジネスメトリクス、OKR 進捗
   - **Quarterly**: 戦略レビューとメトリクスの再調整

5. **アラートの定義**：
   - どの閾値で調査を起動するか？
   - 誰に、どのチャネルで通知するか？
   - 期待される応答時間は？

6. **文脈に応じたツールの推奨**：
   - Amplitude、Mixpanel、PostHog — プロダクト分析
   - Looker、Metabase、Mode — SQL ベースのダッシュボード
   - Datadog、Grafana — 運用健全性

段階的に考える。ダッシュボード仕様を Markdown ドキュメントとして保存する。

---

### さらに読む

- [The Ultimate List of Product Metrics](https://www.productcompass.pm/p/the-ultimate-list-of-product-metrics)
- [The North Star Framework 101](https://www.productcompass.pm/p/the-north-star-framework-101)
- [The Product Analytics Playbook: AARRR, HEART, Cohorts & Funnels for PMs](https://www.productcompass.pm/p/the-product-analytics-playbook-aarrr)
- [AARRR (Pirate) Metrics: The 5-Stage Framework for Growth](https://www.productcompass.pm/p/aarrr-pirate-metrics)
- [The Google HEART Framework: Your Guide to Measuring User-Centric Success](https://www.productcompass.pm/p/the-google-heart-framework)
- [Funnel Analysis 101: How to Track and Optimize Your User Journey](https://www.productcompass.pm/p/funnel-analysis)
- [Are You Tracking the Right Metrics?](https://www.productcompass.pm/p/are-you-tracking-the-right-metrics)
- [Continuous Product Discovery Masterclass (CPDM)](https://www.productcompass.pm/p/cpdm) (video course)