---
name: wwas
description: "Why-What-Acceptance 形式でプロダクトバックログ項目を作成 — 独立・価値あり・検証可能で戦略文脈付き。構造化バックログ項目作成、機能の作業項目分解、WWA 形式利用時に使う。"
---
# Why-What-Acceptance (WWA)

Why-What-Acceptance 形式でプロダクトバックログ項目を作成。戦略文脈付きの独立・価値あり・検証可能な項目を生成する。

**Use when:** バックログ項目作成、プロダクトインクリメント作成、機能の作業項目分解、チームへの戦略的意図の伝達。

**Arguments:**
- `$PRODUCT`: プロダクトまたはシステム名
- `$FEATURE`: 新機能または能力
- `$DESIGN`: デザインファイルへのリンク（Figma、Miro など）
- `$ASSUMPTIONS`: 主要仮定と戦略文脈

## Step-by-Step Process

1. **Define the strategic Why** — 作業をビジネス・チーム目標につなぐ
2. **Describe the What** — 説明は簡潔に、デザイン参照
3. **Write Acceptance Criteria** — 高レベル。詳細仕様ではない
4. **Ensure independence** — 任意の順序で開発可能
5. **Keep items negotiable** — 制約ではなくチーム対話を招く
6. **Make items valuable** — 各項目が測定可能なユーザーまたはビジネス価値を届ける
7. **Ensure testability** — 成果が観測・検証可能
8. **Size appropriately** — 1スプリント見積もりに足る小ささ

## Item Template

**Title:** [What will be delivered]

**Why:** [1-2 sentences connecting to strategic context and team objectives]

**What:** [Short description and design link. 1-2 paragraphs maximum. A reminder of discussion, not detailed specification.]

**Acceptance Criteria:**
- [Observable outcome 1]
- [Observable outcome 2]
- [Observable outcome 3]
- [Observable outcome 4]

## Example WWA Item

**Title:** Implement Real-Time Spending Tracker

**Why:** Users need immediate feedback on spending to make conscious budget decisions. This directly supports our goal to improve financial awareness and reduce overspending.

**What:** Add a real-time spending tracker that updates as users log expenses. The tracker displays their current week's spending against their set budget. Designs available in [Figma link]. This is a reminder of our discussions - detailed specifications will emerge during development conversations with the team.

**Acceptance Criteria:**
- Spending totals update within 2 seconds of logging an expense
- Budget progress is visually indicated with a progress bar
- Users can see remaining budget amount at a glance
- System handles multiple expense categories correctly

## Output Deliverables

- 機能向けバックログ項目一式
- 各項目に Why、What、Acceptance Criteria セクション
- 項目は独立し任意順序でデリバー可能
- 1スプリントでの見積もり・完了向けサイズ
- チーム意思決定のための明確な戦略文脈
- 実装ガイド用デザイン参照

---

### Further Reading

- [How to Write User Stories: The Ultimate Guide](https://www.productcompass.pm/p/how-to-write-user-stories)