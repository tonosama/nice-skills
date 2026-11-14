---
name: market-sizing
description: "トップダウンとボトムアップで TAM、SAM、SOM を用いて市場規模を見積もる。市場機会のサイジング、獲得可能市場の見積もり、投資家ピッチ準備、市場参入評価時に使用。"
---

# 市場規模の見積もり（TAM、SAM、SOM）

## 目的
製品の Total Addressable Market (TAM)、Serviceable Addressable Market (SAM)、Serviceable Obtainable Market (SOM) を見積もる。トップダウン・ボトムアップ両方、成長見込み、検証すべき主要仮説を含む。

## 手順

市場サイジング、機会評価、成長予測を専門とする戦略的市場アナリストである。

### 入力
指定された市場制約（地理、業界 vertical、顧客タイプなど）内で **$ARGUMENTS** の市場規模を見積もるタスク。

ユーザーが市場調査、業界レポート、財務データ、競合情報を提供した場合は直接読み分析する。Web 検索で現行市場データ、業界レポート、成長予測を取得する。

### 分析ステップ（段階的に考える）

1. **市場定義**: 市場境界を定義 — どの問題領域、どの顧客セグメント、どの地理・制約か
2. **トップダウン見積もり**: 業界全体規模から関連スライスに絞る
3. **ボトムアップ見積もり**: ユニットエコノミクス（顧客 × 価格 × 頻度）からクロス検証
4. **SAM スコープ**: 製品能力・チャネル・制約を踏まえ TAM のうち現実的にサービス可能な部分を特定
5. **SOM 見積もり**: 競争地位と GTM 能力に基づき今後 1〜3 年で達成可能なシェアを見積もる
6. **成長予測**: 今後 2〜3 年の TAM、SAM、SOM の変化を予測
7. **仮説マッピング**: 各見積もりの根底にある主要仮説を表面化

### 出力構成

**市場定義**
- 問題領域と顧客ニーズ
- 地理・セグメント境界
- 主要制約またはスコープ決定

**TAM (Total Addressable Market)**
- 出典と論理付きトップダウン見積もり
- クロス検証用ボトムアップ見積もり
- 両アプローチの調整
- 現行 TAM 値（年間収益機会）

**SAM (Serviceable Addressable Market)**
- 製品が現実的にサービス可能な TAM の割合
- 制約：地理、言語、チャネル、製品能力、価格ティア
- TAM に対する SAM の割合と論理

**SOM (Serviceable Obtainable Market)**
- 1〜3 年で現実的に達成可能なシェア
- 根拠：競争地位、GTM 能力、現トラクション
- SAM に対する SOM の割合と論理

**市場サマリー表**

| Metric | Current Estimate | 2-3 Year Projection |
|--------|-----------------|---------------------|
| TAM    |                 |                     |
| SAM    |                 |                     |
| SOM    |                 |                     |

**成長ドライバーとトレンド**
- 市場を拡大・縮小しうる主要要因
- 技術、規制、人口、行動のシフト
- 新興セグメントや隣接市場

**主要仮説とリスク**
- 各見積もりの根底仮説（番号付き）
- 各仮説の信頼度（high / medium / low）
- 最も不確実な仮説の検証方法
- 見積もりを大きく変えうる要因

## ベストプラクティス

- 常にトップダウンとボトムアップの両方で三角測量
- Web 検索で現行業界データ、アナリストレポート、市場ベンチマークを使用
- 市場データには出典を明記 — 根拠のない数値を避ける
- 仮説を明示；推定とデータを区別
- 価値ベース（収益）と量ベース（ユーザー／単位）のサイジングを区別
- 国際市場では通貨と購買力平価を考慮
- 信頼区間が広い見積もりにはフラグ
- 見積もりを精緻化する具体的データソースや調査を推奨

---

### 参考資料

- [Market Research: Advanced Techniques](https://www.productcompass.pm/p/market-research-advanced-techniques)
- [User Interviews: The Ultimate Guide to Research Interviews](https://www.productcompass.pm/p/interviewing-customers-the-ultimate)
- [Crossing the Chasm: The Ultimate Guide For PMs](https://www.productcompass.pm/p/crossing-the-chasm)
- [Product Innovation Masterclass](https://www.productcompass.pm/p/product-innovation-masterclass) (video course)