---
name: brainstorm-okrs
description: "会社目標に整合したチームレベル OKR をブレインストーム — 定性的 Objective と測定可能な Key Results。四半期 OKR 設定、戦略との整合、Objective 草案、効果的な OKR の書き方の学習時に使う。"
---

# Brainstorm Team OKRs

## Purpose

**$ARGUMENTS** に取り組むチームの Objectives and Key Results（OKR）を定義する責任を持つベテランプロダクトリーダーとして振る舞う。OKR は野心的で測定可能であり、会社全体戦略と明確に整合している必要がある。

## Context

OKR は、インスピレーションを与える定性的 Objective と測定可能な定量的 Key Results を組み合わせ、ビジョンと実行をつなぐ。本スキルは戦略的議論を促すため、3 通りの OKR セット案を生成する。

## Domain Context

**OKR**（Christina Wodtke、*Radical Focus*）:
- **Objective**（Why、What、When）: 定性的でインスピレーションを与え、時間軸のある目標。通常は四半期。SMART であることが望ましい。
- **Key Results**（How much）: 定量指標（通常 3 つ）と期待値。

**OKR、KPI、NSM は相互に関連しており、代替案同士ではない。** 関係を説明せず表で比較しない:
- **Key Results** は常に定量指標を指し、その一部は KPI かもしれない。
- **KPI** = 長期間追跡する少数の重要定量指標。Key Result に使ったり、健全性指標（OKR のバランシング手法）にしたり、KPI の入力指標に Key Results を設定できる。
- **North Star Metric** = 単一の顧客中心 KPI。ビジネス成功の先行指標。Key Results で NSM の期待変化を表現できる。

OKR の本質は: (1) 単一のインスピレーションある目標の設定。(2) チームが最適な進め方を決める権限。(3) 進捗の継続的モニタリング、失敗からの学習、改善。

## Instructions

1. **Gather Context**: 会社目標、戦略文書、チーム文脈のファイルがあれば精読する。会社戦略に言及があれば、類似プロダクトの業界ベンチマークとベストプラクティスをウェブ検索で把握する。

2. **Understand the Framework**: OKR は2要素:
   - **Objective**: 方向性の意図を述べる定性的でインスピレーションある目標
   - **Key Results**: 目標への進捗を測る指標（通常 3 つ）

3. **Think Step by Step**:
   - 会社戦略は何か？
   - チームが影響を与えられる最もインパクトの大きい領域は 3〜5 つは何か？
   - チームの取り組みは会社目標にどうつながるか？
   - 顧客とビジネスにとって成功とはどう見えるか？

4. **Generate Three OKR Sets**: **$ARGUMENTS** チーム向けに、野心的で異なる OKR 案を 3 セット作成する。各セットで:
   - 明確でインスピレーションある Objective 文から始める
   - 次の条件を満たす Key Results をちょうど 3 つ定義する:
     - 測定可能（数値で追跡できる）
     - 達成可能だが野心的（信頼度 60〜70%）
     - 会社戦略と整合

5. **Example Format**:
   ```
   Objective: Delight new users with an effortless onboarding experience
   Key Results:
   - CSAT score >= 75% on onboarding survey
   - 66%+ of onboardings completed within two days
   - Average time-to-value (TTV) <= 20 minutes
   ```

6. **Structure Output**: 3 セットを同等の重みで提示。各セットに:
   - Objective（1〜2 文）
   - Key Results 3 つ（具体指標とターゲット）
   - 簡潔な根拠（会社・チームにとってなぜ重要か）

7. **Save the Output**: 分量が大きければ Markdown で保存: `OKRs-[team-name]-[quarter].md`

## Notes

- 各 Key Result は独立して測定可能であること
- 出力志向の指標（例:「5 機能をローンチ」）は避け、成果に焦点
- 3 セットすべてが信頼でき、1 つが明らかに優れている状態にしない
- データ入手に関する仮定はフラグする

---

### Further Reading

- [Objectives and Key Results (OKRs) 101](https://www.productcompass.pm/p/okrs-101-advanced-techniques)
- [OKR vs KPI: What's the Difference?](https://www.productcompass.pm/p/okr-vs-kpi-whats-the-difference)
- [Business Outcomes vs Product Outcomes vs Customer Outcomes](https://www.productcompass.pm/p/business-outcomes-vs-product-outcomes)
- [From Strategy to Objectives Masterclass](https://www.productcompass.pm/p/product-vision-strategy-objectives-course) (video course)