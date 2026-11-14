---
name: create-prd
description: "8 セクション構成（summary、contacts、background、objective、market segments、value propositions（6 部 JTBD）、solution、release）の意思決定向けリーン PRD を作成。デフォルトはリーン（約 1〜2 ページ）、依頼時は詳細版。PRD 作成、要件文書化、機能仕様、既存 PRD レビュー時に使う。"
---

# Create a Product Requirements Document

## Purpose

ユーザーが述べるプロダクトまたは機能について、経験豊富なプロダクトマネージャーとして PRD を書く。PRD は、エンジニア、デザイナー、経営、ステークホルダーが数分で読み、何を最初に・なぜ作るかで合意できるようにする。

## Lean by default

初版 PRD は、チームが判断して着手できる最短の文書: おおよそ 1〜2 ページ（約 600〜1,000 語）。長さは目安であり上限ではない。セグメントが 2〜3 ある PRD は長くなり得るが、各セクションは可能な限り短く。ユーザーが詳細版を求めたときだけ長く書く。

- **Complete** は、初版について判断を妨げるギャップがないことであり、すべての疑問に答えることではない。下の各セクションの質問はチェックリスト: この施策に重要なものだけ答え、残りは省略。
- **言うことが少ないセクションは 1〜2 行。** contacts が分からなければ Contacts セクションは丸ごと省略。後で埋める役割リストは書かない。パディングや「TBD」で埋めない。
- **Market Segments と Value Propositions は常に含める。** テンプレートの核: 誰がどんな問題を持ち、なぜ気にするか。バリュープロポジションはセクション 6 の 6 部テンプレートに従う。
- **捏造しない。** 分からないことは 7.4 Assumptions and open questions に明確にマークし、本文の事実として書かない。

## Instructions

1. **Start from what the user gave you.** 提供ファイルを読む。問題、ターゲットユーザー、成功の測り方が欠けていれば止まり、まず最大 3 つの短い質問を1メッセージで。PRD はその返信には書かない。ユーザーだけが知ること（問題の根拠、ユーザー、成功指標）だけ聞き、ウェブで調べられることは調査する。回答後に PRD を書く。ユーザーが進めてよいと言った場合のみ回答なしで書き、ギャップは仮定としてマーク。

2. **Research freely when it helps.** 市場、競合・代替、規制、最近の変化をウェブ検索し、ユーザー指定のソースを読む。PRD の判断を鋭くする: なぜ今か、セグメントは誰か、代替より何が優れるか。主張の横に出典を付ける。判断を変える調査結果だけ文書に入れる。それ以外の有用情報は返信のメモにし、PRD はリーンに保つ。

3. **Think before writing**（文書には載せない）: 何の問題を、誰のために、どう成功と測り、それを検証する最小の初版は何か？

4. **Write the 8 sections.** リーン版は重要なことだけ。詳細版はより多くの質問に答える。

   **1. Summary**（2〜3 文）
   - 何か、誰向けか、なぜ今か？

   **2. Contacts**（リーン版では任意）
   - 主要ステークホルダーの氏名、役割、コメント

   **3. Background**
   - 文脈: この取り組みは何についてか？
   - なぜ今か？何か変わったか、それとも初めて可能になったか？

   **4. Objective**
   - 目的は何か、顧客・会社にとってなぜ重要か？
   - ビジョン・戦略との整合（該当すれば1行）
   - Key Results: 測定可能な結果 1〜3 つ（SMART OKR 形式）

   **5. Market Segment(s)**（常に含める）
   - 誰のために作るか？ デモグラフィックではなく、人々の問題やジョブでセグメントを定義。
   - 初版は最大 2〜3 セグメントに集中。残りは後の版へ。
   - 制約は何か？

   **6. Value Proposition(s)**（常に含める。セクション 5 のセグメントごとに1つ）

   Paweł Huryn と Aatir Abdul Rauf の 6 部 JTBD バリュープロポジションテンプレートを使う。プロダクトではなく顧客から始め、代替を名指しする:
   - **Who:** セクション 5 のセグメント
   - **Why:** 達成しようとするジョブと、その障害となる問題
   - **What before:** 今日のやり方と、その痛み
   - **How:** ジョブを達成する解法（主要能力。機能はセクション 7）
   - **What after:** 改善された成果と、以前不可能だったことが可能になること
   - **Alternatives:** 私たちなしで何を使うか、なぜ私たちを選ぶか

   1〜2 文のバリュープロポジションステートメントで締める。リーン版では各部分 1〜2 行。詳細版は深掘りし、代替との Value Curve を追加可能。完全な手法と例は、インストールされていれば PM Skills product-strategy プラグインの value-proposition スキルにある。

   本セクションは PRD の背骨: セクション 7（Solution）が How を届け、セクション 4 の Key Results が What after を測る。揃っていなければ他より先に直す。

   **7. Solution**
   - 7.1 UX/Prototypes: 主要フロー、またはワイヤーフレームへのリンク（まだなければ省略）
   - 7.2 Key Features: 初版の必須機能、各 1〜2 行
   - 7.3 Technology（任意、関連するときのみ）
   - 7.4 Assumptions and open questions: 信じているが未証明のこと、まだ調べること

   **8. Release**
   - 初版に入るもの、後回し、明示的にやらないこと
   - どのくらいかかりそうか？ 正確な日付ではなく相対的な時間軸。

5. **Use accessible language.** 小学卒業程度が読める言葉。専門用語を避け、短い明確な文。

6. **Save the output** を Markdown: `PRD-[product-name].md`。

7. **Say what you left out.** 返信のみ（文書内ではなく）、意図的に省略したものを1行（例: 競合分析、詳細 UX フロー、ロールアウト計画）で列挙し、拡張を依頼できるようにする。

## Reviewing an existing PRD

同じ基準で確認: 初版の判断を妨げる欠落はあるか？ Market Segments があるか、各セグメントに 6 部（Who、Why、What before、How、What after、Alternatives）があるか？ Solution の How を届け、Key Results が What after を測っているか？ 仮定は仮定としてマークされているか？ チームに必要なものを失わずに何を削れるか？ まずギャップ、次に削減案を返す。

## Notes

- リーンがデフォルト。詳細は依頼時。
- 可能なら具体とデータ: 形容詞より数値や名前付きユーザーグループ。
- 仮定は明確にフラグし、チームが検証できるようにする。
- 非目標は目標と同様に重要: 初版がやらないことを述べる。

---

### Further Reading

- [How to Write a Product Requirements Document? The Best PRD Template.](https://www.productcompass.pm/p/prd-template)
- [A Proven AI PRD Template by Miqdad Jaffer (Product Lead @ OpenAI)](https://www.productcompass.pm/p/ai-prd-template)