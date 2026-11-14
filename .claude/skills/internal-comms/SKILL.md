---
name: internal-comms
description: |
  社内コミュニケーション、ステータス更新、ニュースレター、FAQ 文書のルーティングと統合の専門家。社内ワークプレース向けコミュニケーション、リーダーシップ更新、チームレポートの作成・編集・フォーマット・下書き依頼時にこのスキルを起動する。Keywords: 3P updates, company newsletter, status reports, leadership updates, project updates, incident reports, FAQ responses, internal communication, staff announcement, examples/.

  社内コミュニケーション、ステータス更新、ニュースレター、FAQ 文書のルーティングと統合の専門家。社内ワークプレース向けコミュニケーション、リーダーシップ更新、チームレポートの作成・編集・フォーマット・下書き依頼時にこのスキルを起動する。Keywords: 3P updates, company newsletter, status reports, leadership updates, project updates, incident reports, FAQ responses, internal communication, staff announcement, examples/.
---

# 社内コミュニケーションルーター

これは **ナビゲーションスキル** である。依頼されたコミュニケーション種別に応じ、`examples/` ディレクトリから対応する詳細 Markdown ガイドラインを **MUST** で読み込むこと。

---

## マインドセットと思考フレームワーク

任意のコミュニケーションを書く前に自問する:

1. **Audience は誰か？**（経営層、直属チーム、横断パートナー、全社か？）
2. **Core Message は何か？**（最重要の要点は？最初の文または見出しに置く。）
3. **求められる Action は？**（FYA - For Your Action か、FYI - For Your Information か？）
4. **Tone は？**（透明、簡潔、データ駆動、祝福的か？）

---

## シナリオ検出と読み込みトリガー

コミュニケーション種別を特定し、下書き前に `examples/` の対応ファイルを **直ちに** 読み込むこと:

| 依頼コミュニケーション種別 | 対象ガイドファイル（必須 - `view_file` で読み込み） | カバー内容 |
| :--- | :--- | :--- |
| **Progress, Plans, Problems** | [`examples/3p-updates.md`](file:///home/jsoehner/yuv-skills-backup/internal-comms/examples/3p-updates.md) | チームまたは横断ステークホルダー向け週次/隔週更新。 |
| **Newsletters / Staff Announcements** | [`examples/company-newsletter.md`](file:///home/jsoehner/yuv-skills-backup/internal-comms/examples/company-newsletter.md) | 全社ダイジェスト、部門マイルストーン、月次サマリー。 |
| **Q&A / FAQs** | [`examples/faq-answers.md`](file:///home/jsoehner/yuv-skills-backup/internal-comms/examples/faq-answers.md) | 従業員・顧客の質問に直接的かつ建設的に回答。 |
| **Other / General Comms** | [`examples/general-comms.md`](file:///home/jsoehner/yuv-skills-backup/internal-comms/examples/general-comms.md) | インシデント更新、臨時リーダーシップメッセージ、プロジェクトマイルストーン。 |

---

## 手順シーケンス

1. **分類:** ユーザーの依頼が 4 バケットのどれに当たるか特定する。
2. **参照読み込み:** `examples/` の該当ファイルで `view_file` を実行する。**フォーマットを推測しないこと。**
3. **コンテキスト抽出:** ユーザーが提供した生の事実、データポイント、箇条書きを集める。
4. **下書き:** 読み込んだ参照で定義された構造・制約・スタイルを適用する。
5. **NEVER リストで精査:** 下書きが下記アンチパターンに違反していないか確認する。

---

## NEVER アンチパターン

| Action | Why | Consequences | Correct Alternative |
| :--- | :--- | :--- | :--- |
| **NEVER** 参照ファイルを読まずにステータスレポートや 3P 更新を書く。 | 組織/チームごとに厳格な構造好みがある。 | 体裁不備、必須セクション欠落。 | 正確なガイドパスで `view_file` を実行する。 |
| **NEVER** 重要コンテキストを文末に埋める。 | 忙しい経営営層・メンバーはスキャンし全文は読まない。 | 意思決定見落とし、アクション遅延、可視性低下。 | サマリー/TL;DR と主要アクションを最初の 2 文に置く。 |
| **NEVER** 専門用語や曖昧な期限（"soon", "next week"）を使う。 | 曖昧さは混乱と期待ズレを生む。 | 期限逸失、説明責任の欠如。 | 具体日（例: "by EOD Friday, Oct 24"）と略語定義を使う。 |
| **NEVER** ステータス更新で悪いニュース・後退・ブロッカーを隠す。 | 問題隠蔽は支援・リソース提供を妨げる。 | エスカレーション遅延、表面化時の信頼失墜。 | "Problems" または "Blockers" に緩和策と共に明記する。 |

---

## 使いやすさとフォールバック

* **直接一致なし:** テンプレに一致しない依頼（例: post-mortem インシデントレポート）では、既定ベースラインとして [`examples/general-comms.md`](file:///home/jsoehner/yuv-skills-backup/internal-comms/examples/general-comms.md) を読み込む。
* **詳細不足:** データなしで更新を求められたら、成果をでっち上げない。ユーザーにプロンプトする:
  > "To draft this update, please provide: (1) Key achievements/launches this period, (2) Critical milestones for next period, and (3) Any blockers/problems you are facing."

## 6) 知識の取り込み

全社アナウンスまたは複雑な社内コミュニケーション確定後、`capture_knowledge.py` スクリプトを自動起動する。
スクリプトはコミュニケーションを分析し以下を特定する:
- 新しい全社ポリシーまたはプロジェクト横断ルール。
- 主要ステークホルダー名と連絡先。
- 繰り返しパターンやフィードバックループ。
情報は適切なストレージへルーティングされる:
- **OKF**: 高レベルコミュニケーション標準、ブランドボイス、告知テンプレート。
- **ChromaDB**: 個別コミュニケーションログ、ステークホルダーサマリー、フィードバック起因の改善。