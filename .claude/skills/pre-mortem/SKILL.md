---
name: pre-mortem
description: "PRD またはローンチ計画のプレモーテムリスク分析。Tiger（実問題）、Paper Tiger（大げさな懸念）、Elephant（言われない不安）に分類し、launch-blocking、fast-follow、track に整理。ローンチ準備、計画のストレステスト、想定外の洗い出し時に使う。"
---

# Pre-Mortem: Risk Analysis for Product Launch

## Purpose

**$ARGUMENTS** についてベテランプロダクトマネージャーとしてプレモーテム分析を実施する。ローンチ失敗を想定し逆算して、実リスクを特定し、認識された心配と区別し、ローンチ阻害要因への対策計画を作る。

## Context

プレモーテムは、まだ動けるローンチ前にうまくいかないことを構造的に洗い出す演習である。失敗を仮定することで隠れた懸念を表に出し、正当な脅威と過大評価された心配を分ける。

## Instructions

1. **Gather the PRD**: PRD またはプロダクト計画ファイルがあれば精読。プロダクト、市場、主要仮定、タイムラインを理解。関連すれば競合・市場条件をウェブ検索。

2. **Think Step by Step**:
   - 14日後にローンチすると想像する
   - 失敗したと想像する — 採用されず、売上目標未達、評判低下
   - 何がうまくいかなかったか？
   - 何を見落とし、実行しなかったか？
   - 何を過信していたか？

3. **Categorize Risks**: 各潜在失敗を3類型に分類:

   **Tigers**: あなた自身が見る、計画を脱線させうる実問題
   - 証拠・経験・明確な論理に基づく
   - 夜眠れなくするもの
   - 行動が必要

   **Paper Tigers**: 他人が心配しそうだが、あなたは信じない問題
   - 表面的には妥当だが起きにくいか過大
   - 大きなリソース投資は不要
   - ステークホルダー整合のため文書化する価値あり

   **Elephants**: 問題か不確かで、チームが十分議論していないこと
   - 誰も検証していない暗黙の懸念・仮定
   - 本当かもしれない
   - ローンチ前に調査に値する

4. **Classify Tigers by Urgency**:

   **Launch-Blocking**: ローンチ前に解決必須
   - 例: コア機能の欠陥、規制ブロッカー、主要顧客依存未充足

   **Fast-Follow**: ローンチ後30日以内に解決必須
   - 例: パフォーマンス問題、二次機能の未完成

   **Track**: ローンチ後モニター。問題化したら対処
   - 例: あればよい機能、エッジケース

5. **Create Action Plans**: Launch-Blocking Tiger ごとに:
   - リスクを明確に記述
   - 具体的緩和策を提案
   - 最適なオーナー（機能/人物）を特定
   - 判断/完了日を設定

6. **Structure Output**: 分析を次の形式で提示:

   ```
   ## Pre-Mortem Analysis: [Product Name]

   ### Tigers (Real Risks)
   [各実リスクとカテゴリ、緩和計画]

   ### Paper Tigers (Overblown Concerns)
   [各項目と、真のリスクでない理由]

   ### Elephants (Unspoken Worries)
   [各項目と調査アプローチの推奨]

   ### Action Plans for Launch-Blocking Tigers
   [各項目: Risk、Mitigation、Owner、Due Date]
   ```

7. **Save the Output**: Markdown: `PreMortem-[product-name]-[date].md`

## Notes

- 正直かつ建設的 — 目的は責任追及ではなくローンチ準備の向上
- 不明ならデフォルトは Tiger。早期対処のほうがよい
- 分析にクロスファンクショナル視点（エンジニアリング、デザイン、GTM）を含める
- Elephant は名付きで上げにくい。チームでは議論前に匿名でリスク収集: リスクを書きアップvote するライブ Q&A ルーム（live-qa スキル）
- ローンチ2〜3週前にプレモーテムを再訪し、緩和が順調か確認

---

### Further Reading

- [How Meta and Instagram Use Pre-Mortems to Avoid Post-Mortems](https://www.productcompass.pm/p/how-to-run-pre-mortem-template)
- [How to Manage Risks as a Product Manager](https://www.productcompass.pm/p/how-to-manage-risks-as-a-product-manager)