---
name: gemini
description: |
  code review、plan review、大規模コンテキスト（>200k）処理のために AI CLI を実行するときに使用。大きな context window が必要な包括的分析向け。既定で Gemini 3 Pro を使用し、最先端の推論とコーディングに対応。

  code review、plan review、大規模コンテキスト（>200k）処理のために AI CLI を実行するときに使用。大きな context window が必要な包括的分析向け。既定で Gemini 3 Pro を使用し、最先端の推論とコーディングに対応。

---

# Gemini スキルガイド

## Gemini を使うタイミング

- アクティベーションを依頼されたとき
- **Code Review**: 複数ファイルにわたる包括的レビュー
- **Plan Review**: アーキテクチャ計画、技術仕様、ロードマップの分析
- **Big Context Processing**: 200k トークン超のコンテキスト（コードベース全体、ドキュメント群）
- **Multi-file Analysis**: 多数ファイル間の関係とパターンの理解

## ⚠️ 重要: バックグラウンド／非対話モードの警告

**バックグラウンドや非対話シェル（AI コーディングエージェントのツール呼び出しなど）では `--approval-mode default` を使わないこと。** 承認プロンプト待ちで無限にハングする。

**自動／バックグラウンドレビューでは:**
- ✅ 完全自動は `--approval-mode yolo`
- ✅ または timeout でラップ: `timeout 300 gemini ...`
- ❌ 非対話シェルで `--approval-mode default` は禁止

**Gemini ハングの症状:**
- 20 分以上 CPU 0% で稼働
- ネットワーク活動なし
- プロセス状態が `S`（sleeping）

**ハングプロセスの対処:**
```bash
# Check if hung
ps aux | grep gemini | grep -v grep

# Kill if necessary
pkill -9 -f "gemini.*gemini-3-pro-preview"
```

## タスクの実行

1. **単一プロンプト**で `AskUserQuestion` により使用モデルを確認。利用可能モデル:
   - `gemini-3-pro-preview` ⭐（フラッグシップ。コーディングと複雑推論向け。2.5 Pro 比ソフトウェアエンジニアリングで約 35% 向上）
   - `gemini-3-flash`（サブ秒レイテンシ。3 Pro 蒸留。速度最優先）
   - `gemini-2.5-pro`（レガシー、汎用）
   - `gemini-2.5-flash`（レガシー、コスト効率と thinking）
   - `gemini-2.5-flash-lite`（レガシー、最高速）

2. タスクに応じて approval mode を選択:
   - `default`: 承認を求める（⚠️ **対話ターミナルのみ**）
   - `auto_edit`: 編集ツールのみ自動承認（提案付き code review）
   - `yolo`: 全ツール自動承認（✅ バックグラウンド／自動タスクで必須）

3. オプションを組み立てる:
   - `-m, --model <MODEL>` — モデル選択
   - `--approval-mode <default|auto_edit|yolo>` — ツール承認
   - `-y, --yolo` — `--approval-mode yolo` の別名
   - `-i, --prompt-interactive "prompt"` — プロンプト実行後対話継続
   - `--include-directories <DIR>` — ワークスペースに追加ディレクトリ
   - `-s, --sandbox` — サンドボックス分離

4. **バックグラウンド／自動タスクでは常に `--approval-mode yolo`**、または timeout ラッパー。非対話シェルで `default` は禁止。

5. コマンド実行と出力取得。バックグラウンド例:
   ```bash
   # Recommended: Use yolo for background tasks
   gemini -m gemini-3-pro-preview --approval-mode yolo \
     "Review this codebase for security issues"

   # Or with timeout safety (5 min limit)
   timeout 300 gemini -m gemini-3-pro-preview --approval-mode yolo \
     "Review this codebase"
   ```

6. 対話セッションで初期プロンプト:
   ```bash
   gemini -m gemini-3-pro-preview -i "Review the authentication system" --approval-mode auto_edit
   ```

7. **完了後**ユーザーに伝える: 「Gemini 分析は完了しました。フォローアップは新セッションで開始するか、結果の深掘りを続けてください。」

### クイックリファレンス

| ユースケース | Approval mode | 主なフラグ |
| --- | --- | --- |
| バックグラウンド code review | `yolo` ✅ | `-m gemini-3-pro-preview --approval-mode yolo` |
| バックグラウンド分析 | `yolo` ✅ | 同上 |
| timeout 付きバックグラウンド | `yolo` ✅ | `timeout 300 gemini -m gemini-3-pro-preview --approval-mode yolo` |
| 対話 code review | `default` | `-m gemini-3-pro-preview --approval-mode default`（対話ターミナルのみ） |
| 自動編集付き review | `auto_edit` | `-m gemini-3-pro-preview --approval-mode auto_edit` |
| 自動リファクタリング | `yolo` | `-m gemini-3-pro-preview --approval-mode yolo` |
| 速度最優先バックグラウンド | `yolo` ✅ | `-m gemini-3-flash --approval-mode yolo` |
| コスト最適化バックグラウンド | `yolo` ✅ | `-m gemini-2.5-flash --approval-mode yolo` |
| 複数ディレクトリ分析 | `yolo`（バックグラウンド時） | `--include-directories <DIR1> --include-directories <DIR2>` |
| プロンプト付き対話 | `auto_edit` or `default` | `-i "prompt" --approval-mode <mode>` |

### モデル選択ガイド

| Model | 最適用途 | Context window | 特徴 |
| --- | --- | --- | --- |
| `gemini-3-pro-preview` ⭐ | **フラッグシップ**: 複雑推論、コーディング、エージェントタスク | 1M input / 64k output | Vibe coding、76.2% SWE-bench 等 |
| `gemini-3-flash` | サブ秒レイテンシ | 1M input / 64k output | 3 Pro 蒸留、TPU 最適化 |
| `gemini-2.5-pro` | レガシー汎用 | 1M input / 65k output | thinking mode |
| `gemini-2.5-flash` | レガシー高ボリューム | 1M input / 65k output | 低価格、thinking |
| `gemini-2.5-flash-lite` | レガシー最高スループット | 1M input / 65k output | 最小レイテンシ |

**Gemini 3 の利点**: ソフトウェアエンジニアリングで約 35% 高精度、SWE-bench 76.2% 等。ナレッジカットオフ: 2025年1月。

**近日**: 超複雑推論向け `gemini-3-deep-think`。

## 一般的なユースケース

### Code Review（バックグラウンド／自動）
```bash
# For background execution (AI coding agent, CI/CD, etc.)
gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Perform a comprehensive code review focusing on:
   1. Security vulnerabilities
   2. Performance issues
   3. Code quality and maintainability
   4. Best practices violations"

# With timeout safety (5 minutes)
timeout 300 gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Perform a comprehensive code review..."
```

### Plan Review（バックグラウンド／自動）
```bash
gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Review this architectural plan for:
   1. Scalability concerns
   2. Missing components
   3. Integration challenges
   4. Alternative approaches"
```

### Big Context Analysis（バックグラウンド／自動）
```bash
gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Analyze the entire codebase to understand:
   1. Overall architecture
   2. Key patterns and conventions
   3. Potential technical debt
   4. Refactoring opportunities"
```

### 対話 Code Review（ターミナルのみ）
```bash
gemini -m gemini-3-pro-preview --approval-mode default \
  "Review the authentication flow for security issues"
```

## フォローアップ

- AI CLI セッションは通常ワンショットまたは対話。Codex と異なり組み込み resume はない。
- フォローアップは前回所見をコンテキストに新規 Gemini セッションを開始する。
- 次アクション提案時は選択モデルと approval mode を再掲する。
- 各コマンド後 `AskUserQuestion` で次ステップを確認する。

## エラーハンドリング

- `gemini --version` または Gemini コマンドが非ゼロ終了なら停止して報告する。
- 失敗コマンドの再試行前に方針を確認する。
- 高インパクトフラグ（`--approval-mode yolo`、`-y`、`--sandbox`）は未承認なら `AskUserQuestion` で許可を得る。
- 警告や部分結果は要約し、`AskUserQuestion` で調整方針を確認する。

## ハングした Gemini プロセスのトラブルシューティング

### 検出
```bash
ps aux | grep gemini | grep -v grep
```

### 解決
```bash
pkill -9 -f "gemini.*gemini-3-pro-preview"
kill -9 <PID>
```

### 予防
- **バックグラウンド／自動では常に `--approval-mode yolo`**
- 安全のため `timeout 300 gemini ...`
- 非対話シェルで `default` 禁止

## 大規模コンテキスト処理のヒント

1. **具体性**: 分析内容を明確に構造化したプロンプト
2. **include-directories**: 関連ディレクトリを明示
3. **モデル選択**: 複雑推論は `gemini-3-pro-preview`、速度は `gemini-3-flash`、コストは `gemini-2.5-flash`
4. **Gemini 3 の強み**: ソフトウェアエンジニアリングとエージェントワークフロー
5. **タスク分割**: 大コンテキストでも構造化分析が有効
6. **所見の保存**: 構造化レポート出力を依頼する

## CLI バージョン

Gemini 3 対応は AI CLI v0.16.0 以降。`gemini --version` で確認。

## アンチパターンと安全警告

| 避けること | 理由 | 代わりに |
|-------|-----|---------|
| バックグラウンドで default | キーボード入力待ちでハング | 常に `--approval-mode yolo` |
| timeout 回避 | API 断でフリーズ | 先頭に `timeout 300` |
| コンテキスト過多 | 無関係ファイルで window 浪費 | パスを絞る |

## 参照

**必須 — 自己完結ディレクティブ**:

本スキルは自己完結。本ワークフロー実行中に明示的に作成しない限り、外部ファイルや参照ディレクトリを読み込まない。