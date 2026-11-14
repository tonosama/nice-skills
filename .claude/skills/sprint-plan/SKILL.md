---
name: sprint-plan
description: "キャパシティ見積もり、ストーリー選択、依存関係マッピング、リスク特定を含むスプリント計画。スプリント計画準備、チームキャパシティ見積もり、ストーリー選択、ベロシティとのスコープバランス調整時に使う。"
---

## Sprint Planning

チームキャパシティの見積もり、ストーリーの選択・順序付け、リスク特定によってスプリントを計画する。

### Context

**$ARGUMENTS** のスプリント計画を支援している。

ユーザーがファイル（バックログ、ベロシティデータ、チーム名簿、過去スプリントレポート）を提供したら先に読む。

### Instructions

1. **Estimate team capacity**:
   - メンバー数と稼働（PTO、会議、オンコール）
   - 過去ベロシティ（直近3スプリントの平均ストーリーポイント）
   - キャパシティバッファ: 想定外作業、バグ、技術的負債のため15〜20%を確保
   - ストーリーポイントまたは理想時間で利用可能キャパシティを算出

2. **Review and select stories**:
   - 優先度順にバックログから取得（最高優先が先）
   - 各ストーリーが Definition of Ready を満たすか（明確な AC、見積もり済み、ブロッカーなし）
   - コミット前にリファインが必要なストーリーにフラグ
   - キャパシティに達したら追加を止める

3. **Map dependencies**:
   - 他ストーリーや外部チームに依存するストーリーを特定
   - 依存関係に応じた順序付け
   - 外部依存とオーナーにフラグ
   - クリティカルパスを特定

4. **Identify risks and mitigations**:
   - 不確実性・複雑性が高いストーリー
   - スリップしうる外部依存
   - 知識の集中（一人しかできない）
   - 各リスクへの緩和策を提案

5. **Create the sprint plan summary**:

   ```
   Sprint Goal: [One sentence describing what success looks like]
   Duration: [2 weeks / 1 week / etc.]
   Team Capacity: [X story points]
   Committed Stories: [Y story points across Z stories]
   Buffer: [remaining capacity]

   Stories:
   1. [Story title] — [points] — [owner] — [dependencies]
   ...

   Risks:
   - [Risk] → [Mitigation]
   ```

6. **Define the sprint goal**: スプリントの主要価値提供を捉える1文。

段階的に考える。Markdown として保存。

---

### Further Reading

- [Product Owner vs Product Manager: What's the difference?](https://www.productcompass.pm/p/product-manager-vs-product-owner)