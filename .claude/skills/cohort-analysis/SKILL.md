---
name: cohort-analysis
description: "ユーザーエンゲージメントデータのコホート分析 — リテンション曲線、機能採用トレンド、セグメント別インサイト。コホート別リテンション、時間経過の機能採用、チャーン傾向、エンゲージメントトレンドの分析時に使う。"
---

# Cohort Analysis & Retention Explorer

## Purpose

コホート別にユーザーのエンゲージメントとリテンションパターンを分析し、行動・機能採用・長期エンゲージメントのトレンドを特定する。定量インサイトと定性調査の推奨を組み合わせる。

## How It Works

### Step 1: Read and Validate Your Data
- ユーザーコホート情報を含む CSV、Excel、JSON を受け付ける
- データ構造を検証: コホート識別子、期間、エンゲージメント指標
- 欠損値とデータ品質の問題を確認
- 主要統計を要約（コホートサイズ、日付範囲、利用可能指標）

### Step 2: Generate Quantitative Analysis
- コホートリテンション率とエンゲージメントトレンドを計算
- リテンション曲線、離脱パターン、異常を特定
- コホート横断の機能採用率を算出
- 月次または期間ごとの変化を計算
- 依頼があれば pandas と numpy を使う Python 分析スクリプトを生成

### Step 3: Create Visualizations
- リテンションヒートマップ（コホート × 期間）を生成
- コホート推移の折れ線グラフを作成
- 機能採用の比較チャートを構築
- 離脱点とエンゲージメントトレンドを可視化
- インタラクティブチャートまたは静的画像で出力

### Step 4: Identify Insights & Patterns
- 次のような有意なパターンを1つ以上見つける:
  - 特定コホートの早期チャーン
  - 後期段階のエンゲージメント変化
  - 機能採用のクラスター
  - 季節性や時間的トレンド
- 意外な発見と乖離を強調
- コホートパフォーマンスを比較してベースラインを確立

### Step 5: Suggest Follow-Up Research
- 定性調査手法を推奨:
  - 離脱ユーザーへの targeted インタビュー
  - エンゲージコホートへの機能利用アンケート
  - 主要インタラクションパターンのセッションリプレイ
  - 高・低リテンションコホートの win/loss 分析
- フォローアップ定量調査の設計
- A/B テストや機能実験の提案

## Usage Examples

**Example 1: Upload CSV Data**
```
Upload cohort_engagement.csv with columns: cohort_month, weeks_active,
user_id, feature_x_usage, engagement_score

Request: "Analyze retention patterns and identify why Q4 2025 cohorts
underperform compared to Q3"
```

**Example 2: Describe Data Format**
```
"I have monthly user cohorts from Jan-Dec 2025. Each row shows:
cohort date, user ID, purchase frequency, and support tickets.
Analyze which cohorts show best long-term retention."
```

**Example 3: Feature Adoption Analysis**
```
Upload feature_usage.xlsx with cohort adoption data.

Request: "Compare adoption curves for our new feature across cohorts.
Which cohorts adopted fastest? Any patterns?"
```

## Key Capabilities

- **Data Reading**: CSV、Excel、JSON、SQL クエリ結果のインポート
- **Retention Analysis**: 時間経過に沿ったリテンション率の計算と可視化
- **Cohort Comparison**: コホートグループ間の指標比較
- **Anomaly Detection**: 異常パターンや離脱のフラグ
- **Python Scripts**: 継続分析用の再利用可能な分析コードの生成
- **Visualizations**: ヒートマップ、チャート、インタラクティブダッシュボード
- **Research Design**: ターゲットを絞ったフォローアップ調査とインタビュー手法の提案
- **Statistical Summary**: 定量指標と相関分析の提供

## Tips for Best Results

1. **時間軸を含める**: 複数期間にわたるデータを提供する
2. **コホートを明確に定義**: グルーピングを明示（登録月、機能ローンチ日など）
3. **文脈を提供**: 期間中のプロダクト変更、ローンチ、イベントを説明する
4. **複数指標**: リテンション、エンゲージメント、機能利用、売上などを含める
5. **十分なデータ**: 意味のあるパターン特定のため少なくとも 3〜4 コホート
6. **出力を具体指定**: 可視化、Python スクリプト、調査推奨を依頼する

## Output Format

受け取る内容:
- **Data Summary**: コホート概要とデータ品質評価
- **Quantitative Findings**: 主要指標、リテンション率、トレンド分析
- **Visualizations**: リテンション曲線・採用パターンのチャート
- **Pattern Identification**: データから得た 2〜3 の重要インサイト
- **Research Recommendations**: 具体的な定性・定量フォローアップ
- **Analysis Scripts**（依頼時）: 再現可能な分析用 Python コード
- **Next Steps**: 所見に基づく優先アクション

---

### Further Reading

- [Cohort Analysis 101: How to Reduce Churn and Make Better Product Decisions](https://www.productcompass.pm/p/cohort-analysis)
- [The Product Analytics Playbook: AARRR, HEART, Cohorts & Funnels for PMs](https://www.productcompass.pm/p/the-product-analytics-playbook-aarrr)
- [Are You Tracking the Right Metrics?](https://www.productcompass.pm/p/are-you-tracking-the-right-metrics)