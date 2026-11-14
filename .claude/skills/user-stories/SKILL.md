---
name: user-stories
description: "3 C's（Card、Conversation、Confirmation）と INVEST 基準に沿ったユーザーストーリーを作成。説明、デザインリンク、受け入れ基準付き。ユーザーストーリー作成、機能のバックログ分解、受け入れ基準定義時に使う。"
---
# User Stories

3 C's（Card、Conversation、Confirmation）と INVEST 基準に沿ってユーザーストーリーを作成。説明、デザインリンク、受け入れ基準付きストーリーを生成。

**Use when:** ユーザーストーリー作成、機能のストーリー分解、バックログ項目作成、受け入れ基準定義。

**Arguments:**
- `$PRODUCT`: プロダクトまたはシステム名
- `$FEATURE`: ストーリーに分解する新機能
- `$DESIGN`: デザインファイルへのリンク（Figma、Miro など）
- `$ASSUMPTIONS`: 主要仮定や文脈

## Step-by-Step Process

1. **Analyze the feature** 提供デザインと文脈に基づく
2. **Identify user roles** と異なるユーザージャーニー
3. **Apply 3 C's framework:**
   - Card: シンプルなタイトルと一言
   - Conversation: 意図の詳細議論
   - Confirmation: 明確な受け入れ基準
4. **Respect INVEST criteria:** Independent、Negotiable、Valuable、Estimable、Small、Testable
5. **Use plain language** 小学卒業程度が理解できる表現
6. **Link to design files** 視覚参照用
7. **Output user stories** 構造化形式で

## Story Template

**Title:** [Feature name]

**Description:** As a [user role], I want to [action], so that [benefit].

**Design:** [Link to design files]

**Acceptance Criteria:**
1. [Clear, testable criterion]
2. [Observable behavior]
3. [System validates correctly]
4. [Edge case handling]
5. [Performance or accessibility consideration]
6. [Integration point]

## Example User Story

**Title:** Recently Viewed Section

**Description:** As an Online Shopper, I want to see a 'Recently viewed' section on the product page to easily revisit items I considered.

**Design:** [Figma link]

**Acceptance Criteria:**
1. The 'Recently viewed' section is displayed at the bottom of the product page for every user who has previously viewed at least 1 product.
2. It is not displayed for users visiting the first product page of their session.
3. The current product itself is excluded from the displayed items.
4. The section showcases product cards or thumbnails with images, titles, and prices.
5. Each product card indicates when it was viewed (e.g., 'Viewed 5 minutes ago').
6. Clicking on a product card leads the user to the corresponding product page.

## Output Deliverables

- 機能向けユーザーストーリー一式
- 各ストーリーにタイトル、説明、デザインリンク、受け入れ基準4〜6件
- ストーリーは独立し任意の順序で開発可能
- 1スプリントサイクル向けのサイズ
- 関連デザイン文書への参照

---

### Further Reading

- [How to Write User Stories: The Ultimate Guide](https://www.productcompass.pm/p/how-to-write-user-stories)