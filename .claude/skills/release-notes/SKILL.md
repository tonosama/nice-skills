---
name: release-notes
description: "チケット、PRD、changelog からユーザー向けリリースノートを生成。カテゴリ別（新機能、改善、修正）の分かりやすい要約。リリースノート作成、changelog、プロダクト更新告知、出荷内容の要約時に使う。"
---

## Release Notes Generator

技術チケット、PRD、社内 changelog を、洗練されたユーザー向けリリースノートに変換する。

### Context

**$ARGUMENTS** のリリースノートを書いている。

ユーザーがファイル（JIRA エクスポート、Linear チケット、PRD、Git ログ、社内 changelog）を提供したら先に読む。プロダクト URL に言及があればウェブ検索でプロダクトとオーディエンスを把握。

### Instructions

1. **Gather raw material**: 提供されたチケット、changelog、説明をすべて読む。抽出:
   - 何が変わったか（機能、改善、修正）
   - 誰に影響するか（どのユーザーセグメント）
   - なぜ重要か（ユーザー便益）

2. **Categorize changes**:
   - **New Features**: まったく新しい能力
   - **Improvements**: 既存機能の強化
   - **Bug Fixes**: 解決した問題
   - **Breaking Changes**: ユーザー操作が必要なもの（マイグレーション、API 変更）
   - **Deprecations**: 提供終了予定の機能

3. **Write each entry** 次の原則で:
   - 技術変更ではなくユーザー便益を先に
   - 平易な言葉 — 専門用語、社内コード名、チケット番号を避ける
   - 各項目 1〜3 文
   - ユーザーがビジュアルやスクリーンショットを提供したら含める

   **Example transformations**:
   - Technical: "Implemented Redis caching layer for dashboard API endpoints"
   - User-facing: "Dashboards now load up to 3× faster, so you spend less time waiting and more time analyzing."

   - Technical: "Fixed race condition in concurrent checkout flow"
   - User-facing: "Fixed an issue where some orders could fail during high-traffic periods."

4. **Structure the release notes**:

   ```
   # [Product Name] — [Version / Date]

   ## New Features
   - **[Feature name]**: [1-2 sentence description of what it does and why it matters]

   ## Improvements
   - **[Area]**: [What got better and how it helps]

   ## Bug Fixes
   - Fixed [issue description in user terms]

   ## Breaking Changes (if any)
   - **Action required**: [What users need to do]
   ```

5. **Adjust tone** プロダクトの声に合わせる — B2B はプロフェッショナル、コンシューマーはフレンドリー、API は開発者向け。

Markdown として保存。HTML など別形式が必要なら変換。