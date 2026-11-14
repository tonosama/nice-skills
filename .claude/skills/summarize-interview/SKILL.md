---
name: summarize-interview
description: "顧客インタビュー書き起こしを JTBD・満足シグナル・アクション項目を含む構造化テンプレートに要約する。インタビュー録音・書き起こしの処理、ディスカバリーインタビューの統合、インタビューサマリー作成時に使う。"
---

## 顧客インタビューの要約

インタビュー書き起こしを、Jobs to Be Done、満足度、 action items に焦点を当てた構造化サマリーに変換する。

### コンテキスト

**$ARGUMENTS** のプロダクトディスカバリー向けに顧客インタビューを要約している。

ユーザーがインタビュー書き起こしを提供する — 添付ファイル（テキスト、PDF、音声書き起こし）または直接貼り付け。添付があれば先に読む。

### 手順

1. 要約の前に**書き起こし全文**を注意深く読む。

2. 下の**サマリーテンプレート**を埋める。情報がなければ「-」。必要なら数値は定性的表現に置き換える（例：「not satisfied」）。

3. **明確で簡単な言葉**を使う — 小学校卒業程度でも理解できるサマリーにする。

### 出力テンプレート

```
**Date**: [Date and time of the interview]
**Participants**: [Full names and roles]
**Background**: [Background information about the customer]

**Current Solution**: [What solution they currently use]

**What They Like About Current Solution**:
- [Job to be done, desired outcome, importance, and satisfaction level]

**Problems With Current Solution**:
- [Job to be done, desired outcome, importance, and satisfaction level]

**Key Insights**:
- [Unexpected findings or notable quotes]

**Action Items**:
- [Date, Owner, Action — e.g., "2025-01-15, Paweł Huryn, Follow up with customer about pricing"]
```

サマリーをユーザーのワークスペースに Markdown ドキュメントとして保存する。

---

### さらに読む

- [User Interviews: The Ultimate Guide to Research Interviews](https://www.productcompass.pm/p/interviewing-customers-the-ultimate)
- [Continuous Product Discovery Masterclass (CPDM)](https://www.productcompass.pm/p/cpdm) (video course)