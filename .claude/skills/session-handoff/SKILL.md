---
name: session-handoff
description: |
  AI エージェントセッション引き継ぎ用の包括的 handoff 文書を作成。(1) handoff/メモリ/コンテキスト保存の依頼、(2) コンテキストウィンドウが上限に近い、(3) 主要マイルストーン完了、(4) 作業セッション終了、(5) 「save state」「create handoff」「I need to pause」「context is getting full」、(6) 「load handoff」「resume from」「continue where we left off」で再開。大量編集・複雑デバッグ・アーキ決定後は handoff を積極提案。長時間エージェントのコンテキスト枯渇を解消し、新エージェントが曖昧さゼロで継続できるようにする。

---

# Handoff

包括的 handoff 文書を作成し、新しい AI エージェントが曖昧さなく作業を継続できるようにする。長時間エージェントのコンテキスト枯渇問題を解く。

## モード選択

**handoff 作成?** 現状保存、作業一時停止、コンテキスト逼迫。
- 下記 CREATE ワークフローに従う

**handoff から再開?** 前回作業の継続、コンテキスト読込、既存 handoff 言及。
- 下記 RESUME ワークフローに従う

**積極提案?** 大量作業後（5 ファイル以上編集、複雑デバッグ、主要決定）:
> 「大きな進捗がありました。将来セッション用に handoff 文書の作成を検討してください。準備できたら 'create handoff' と言ってください。」

## CREATE ワークフロー

### Step 1: スキャフォールド生成

```bash
python scripts/create_handoff.py [task-slug] [--continues-from <previous-handoff>]
```

例: `python scripts/create_handoff.py "implementing-auth"`

**継続 handoff**（前作業へのリンク）:
```bash
python scripts/create_handoff.py "auth-part-2" --continues-from 2024-01-15-auth.md
```

スクリプトは次を行う:
- 必要なら `.claude/handoffs/` を作成
- タイムスタンプ付きファイル名
- timestamp、プロジェクトパス、git branch、直近コミット、変更ファイルを事前入力
- 継続時は handoff チェーンリンクを追加
- 編集用ファイルパスを出力

### Step 2: handoff 文書の完成

生成ファイルを開き、全 `[TODO: ...]` を記入。優先セクション:

1. **Current State Summary** — いま何が起きているか
2. **Important Context** — 次エージェントが **必ず** 知るべき情報
3. **Immediate Next Steps** — 明確で実行可能な最初のステップ
4. **Decisions Made** — 結果だけでなく根拠付きの選択

構造の参考: [references/handoff-template.md](references/handoff-template.md)

### Step 3: handoff の検証

```bash
python scripts/validate_handoff.py <handoff-file>
```

検証内容:
- [ ] `[TODO: ...]` プレースホルダーが残っていない
- [ ] 必須セクションが存在し内容がある
- [ ] secret（API キー、パスワード、トークン）の疑いがない
- [ ] 参照ファイルが存在する
- [ ] 品質スコア (0-100)

**secret 検出またはスコア 70 未満では確定しない。**

### Step 4: handoff 確認

ユーザーへ報告:
- handoff ファイルの場所
- 検証スコアと警告
- キャプチャしたコンテキストの要約
- 次セッションの最初のアクション

## RESUME ワークフロー

### Step 1: 利用可能 handoff の一覧

```bash
python scripts/list_handoffs.py
```

日付・タイトル・完了状態を表示。

### Step 2: 鮮度チェック

```bash
python scripts/check_staleness.py <handoff-file>
```

鮮度レベル:
- **FRESH**: 再開安全 — handoff 以降の変更が最小
- **SLIGHTLY_STALE**: 変更をレビューしてから再開
- **STALE**: 再開前にコンテキストを慎重に検証
- **VERY_STALE**: 新しい handoff の作成を検討

チェック: 経過時間、handoff 以降の git コミット、変更ファイル、ブランチ分岐、参照ファイル欠落

### Step 3: handoff の読み込み

いかなる作業もする前に handoff 文書を最後まで読む。

チェーンの場合（"Continues from" リンク）は前の handoff も読んで完全な文脈を得る。

### Step 4: コンテキスト検証

[references/resume-checklist.md](references/resume-checklist.md) のチェックリスト:

1. プロジェクトディレクトリと git branch が一致
2. ブロッカー解消済みか
3. 前提がまだ有効か
4. 競合のない変更ファイルか
5. 環境状態の確認

### Step 5: 作業開始

handoff の "Immediate Next Steps" #1 から開始。

参照:
- "Critical Files" — 重要パス
- "Key Patterns Discovered" — 守るべき規約
- "Potential Gotchas" — 既知の落とし穴

### Step 6: handoff 更新またはチェーン

作業中:
- "Pending Work" の完了項目をマーク
- 新発見を関連セクションへ追加
- 長セッション: `--continues-from` で新 handoff をチェーン

## Handoff チェーン

長期プロジェクトでは文脈系譜を維持:

```
handoff-1.md (initial work)
    ↓
handoff-2.md --continues-from handoff-1.md
    ↓
handoff-3.md --continues-from handoff-2.md
```

各 handoff は前身をリンクし、古いものを superseded 可能。新エージェント向けパンくずを提供。

チェーン再開時は最新を先に読み、必要に応じて前身を参照。

## 保存場所

`.claude/handoffs/`

命名: `YYYY-MM-DD-HHMMSS-[slug].md`

例: `2024-01-15-143022-implementing-auth.md`

## リソース

### scripts/

| Script | Purpose |
|--------|---------|
| `create_handoff.py [slug] [--continues-from <previous-handoff>]` | スマートスキャフォールドで新 handoff 生成 |
| `list_handoffs.py [path]` | プロジェクト内 handoff 一覧 |
| `validate_handoff.py <file>` | 完全性・品質・セキュリティチェック |
| `check_staleness.py <file>` | handoff コンテキストの鮮度評価 |

### references/

- [handoff-template.md](references/handoff-template.md) — テンプレ構造とガイダンス
- [resume-checklist.md](references/resume-checklist.md) — 再開エージェント向け検証チェックリスト

## 参照と読み込みトリガー

**必須 - LOADING TRIGGERS**:
- handoff 内の `[TODO: ...]` を編集する前、[handoff-template.md](references/handoff-template.md) を **必ず** 全文読む
- 再開時のブランチ分岐検証前、[resume-checklist.md](references/resume-checklist.md) を **必ず** 読む
- 一覧・鮮度チェック時はテンプレ参照ファイルを **読み込まない**（コンテキスト節約）

### 未追跡 Git ファイルとサマリー長制限

1. RESUME Step 2 の鮮度チェックで `git status --porcelain` を実行し未追跡ファイルを確認
2. "Current State Summary" は 3 段落未満に制限し再開エージェントの可読性を保つ