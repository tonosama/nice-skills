---
name: summarize-meeting
description: "会議トランスクリプトを日付、参加者、トピック、主要決定、要約ポイント、アクション項目付きの構造化メモに要約。会議録音の処理、議事録作成、ミーティング議事、議論の要約時に使う。"
---

# Summarize Meeting

## Purpose

**$ARGUMENTS** から明確で実行可能な会議要約を作成する責任を持つ経験豊富なプロダクトマネージャーとして振る舞う。生のトランスクリプトを、チームの整合と説明責任を保つ構造化されたわかりやすい要約に変換する。

## Context

会議要約はプロダクトチームで知識が広がり、説明責任が明確に残る手段である。よく構成された要約は、参加有無にかかわらず誰でも理解できる言葉で決定、要点、アクションを捉える。

## Instructions

1. **Gather the Meeting Content**: トランスクリプト、録音、メモファイルがあれば精読。文脈が必要な会議に言及があれば、関連資料をウェブ検索。

2. **Think Step by Step**:
   - 誰が出席し、役割は？
   - 主題・アジェンダは？
   - どんな決定がなされたか？
   - 次のステップと担当は？
   - 未解決の質問やブロッカーは？

3. **Extract Key Information**:
   - 主要議論トピックを特定
   - 会議中の決定を記録
   - 意見の相違や懸念にフラグ
   - オーナーと期限付きアクション項目を確定

4. **Create Structured Summary**: 次のテンプレートを使用:

   ```
   ## Meeting Summary

   **Date & Time**: [Date and start/end time]

   **Participants**: [Full names and roles, if available]

   **Topic**: [Short title—what was the meeting about?]

   **Summary**

   - **Point 1**: [Key discussion point or decision]
   - **Point 2**: [Key discussion point or decision]
   - **Point 3**: [Key discussion point or decision]
   - [Additional points as needed]

   **Action Items**

   | Due Date | Owner | Action |
   |----------|-------|--------|
   | [Date] | [Name] | [What needs to happen] |
   | [Date] | [Name] | [What needs to happen] |

   **Decisions Made**
   - [Decision 1]
   - [Decision 2]

   **Open Questions**
   - [Unresolved question 1]
   - [Unresolved question 2]
   ```

5. **Use Accessible Language**: 小学卒業程度が読める言葉。平易な用語。専門用語は避けるか短く説明。

6. **Prioritize Clarity**: 焦点:
   - ロードマップや戦略に影響する決定は？
   - 各人が何をすべきか？
   - いつまでに？

7. **Save the Output**: Markdown: `Meeting-Summary-[date]-[topic].md`

## Notes

- 客観的に — 個人的意見ではなく議論内容を要約
- アクション項目を明確に — 取りこぼしを防ぐ
- 大規模・複雑な会議はトピック別セクション分割を検討
- 包括的で協調的なチーム感のため「we」表現を使用