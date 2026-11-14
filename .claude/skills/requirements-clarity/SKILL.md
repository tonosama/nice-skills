---
name: requirements-clarity
description: |
  実装前に焦点を当てた対話で曖昧な要件を明確化する。要件が不明瞭、機能が複雑（2日超）、チーム横断の調整を伴うときに使用。実装前に「なぜ？」（YAGNI）と「もっと簡単に？」（KISS）の2問を投げかけ明確性を確保する。

---

# 要件明確化スキル

## 説明

100 点満点のスコアリング体系による体系的な明確化で、曖昧な要件を実行可能な PRD に自動変換する。


## 手順

起動時、曖昧な要件を検出する:

1. **曖昧な機能リクエスト**
   - ユーザー例: "add login feature", "implement payment", "create dashboard"
   - 不足: 方法、技術スタック、制約

2. **技術コンテキスト不足**
   - 技術スタックの言及なし
   - 統合ポイント未特定
   - 性能・セキュリティ制約なし

3. **仕様の不完全性**
   - 受け入れ基準なし
   - 成功指標なし
   - エッジケース未考慮
   - エラーハンドリング未言及

4. **曖昧なスコープ**
   - 境界が不明（「user management」— 具体的に何か？）
   - MVP と将来拡張の区別なし
   - 「含まないもの」の欠如

**次の場合は起動しない**:
- 特定ファイルパスが言及された場合（例: `auth.go:45`）
- コードスニペットが含まれる
- 既存関数・クラスが参照される
- 再現手順が明確なバグ修正

## コア原則

1. **体系的な質問**
   - 焦点を当てた具体的な質問をする
   - 1カテゴリずつ（1ラウンド 2〜3 問）
   - 前の回答を積み上げる
   - ユーザーを圧倒しない

2. **品質駆動の反復**
   - 明確性スコア（0〜100）を継続評価
   - ギャップを体系的に特定
   - 90 点以上まで反復
   - すべての明確化ラウンドを記録

3. **実行可能な成果物**
   - 具体的な仕様を生成
   - 測定可能な受け入れ基準を含める
   - 実行可能なフェーズを提供
   - 直接実装可能にする

## 明確化プロセス

### ステップ 1: 初期要件分析

**入力**: ユーザーの要件説明

**タスク**:
1. コア要件を解析・理解
2. 機能名を生成（kebab-case）
3. ドキュメントバージョンを決定（ユーザー指定がなければ既定 `1.0`）
4. PRD 出力用に `./docs/prds/` の存在を確認
5. 初期明確性評価（0〜100）

**評価ルーブリック**:
```
機能的明確性: /30 点
- 入出力の明確さ: 10 点
- ユーザー操作の定義: 10 点
- 成功基準の記述: 10 点

技術的特異性: /25 点
- 技術スタックの言及: 8 点
- 統合ポイントの特定: 8 点
- 制約の明記: 9 点

実装の完全性: /25 点
- エッジケース考慮: 8 点
- エラーハンドリング言及: 9 点
- データ検証の明記: 8 点

ビジネスコンテキスト: /20 点
- 問題文の明確さ: 7 点
- ターゲットユーザーの特定: 7 点
- 成功指標の定義: 6 点
```

**初期応答フォーマット**:
```markdown
要件を理解しました。この仕様の磨き込みをお手伝いします。

**現在の明確性スコア**: X/100

**明確な点**:
- [List what's clear]

**明確化が必要な点**:
- [List gaps]

これらの点を体系的に明確化します...
```

### ステップ 2: ギャップ分析

4 次元で不足情報を特定:

**1. 機能スコープ**
- コア機能は何か？
- 境界はどこか？
- スコープ外は何か？
- エッジケースは？

**2. ユーザー操作**
- ユーザーはどう操作するか？
- 入力は？
- 出力は？
- 成功／失敗シナリオは？

**3. 技術制約**
- 性能要件は？
- 互換性要件は？
- セキュリティ考慮は？
- スケーラビリティ要件は？

**4. ビジネス価値**
- どの問題を解決するか？
- ターゲットユーザーは？
- 成功指標は？
- 優先度は？

### ステップ 3: 対話的明確化

**質問戦略**:
1. インパクト最大のギャップから
2. 1ラウンド 2〜3 問
3. 文脈を段階的に構築
4. ユーザーの言語を使う
5. 有益なら例を示す

**質問フォーマット**:
```markdown
要件ドキュメントを完成させるため、次の点を明確化する必要があります:

1. **[Category]**: [Specific question]?
   - 例: [Example if helpful]

2. **[Category]**: [Specific question]?

3. **[Category]**: [Specific question]?

回答をいただければ、PRD の磨き込みを続けます。
```

**各ユーザー応答後**:
1. 明確性スコアを更新
2. 作業中 PRD アウトラインに新情報を反映
3. 残ギャップを特定
4. スコア < 90: 次ラウンドの質問を継続
5. スコア ≥ 90: PRD 生成へ

**スコア更新フォーマット**:
```markdown
追加情報ありがとうございます!

**明確性スコア更新**: X/100 → Y/100

**新たに明確化した内容**:
- [Summarize new information]

**残りの明確化ポイント**:
- [List remaining gaps if score < 90]

[スコア < 90: 次ラウンドの質問を継続]
[スコア ≥ 90: "完璧です。これから完全な PRD ドキュメントを生成します..."]
```

### ステップ 4: PRD 生成

明確性スコアが ≥ 90 になったら包括的 PRD を生成。

**出力ファイル**:

1. **最終 PRD**: `./docs/prds/{feature_name}-v{version}-prd.md`

`Write` ツールでこのファイルを作成または更新する。`{version}` は PRD に記録したドキュメントバージョンから導出（既定 `1.0`）。

## PRD ドキュメント構造

```markdown
# {Feature Name} - Product Requirements Document (PRD)

## Requirements Description

### Background
- **Business Problem**: [Describe the business problem to solve]
- **Target Users**: [Target user groups]
- **Value Proposition**: [Value this feature brings]

### Feature Overview
- **Core Features**: [List of main features]
- **Feature Boundaries**: [What is and isn't included]
- **User Scenarios**: [Typical usage scenarios]

### Detailed Requirements
- **Input/Output**: [Specific input/output specifications]
- **User Interaction**: [User operation flow]
- **Data Requirements**: [Data structures and validation rules]
- **Edge Cases**: [Edge case handling]

## Design Decisions

### Technical Approach
- **Architecture Choice**: [Technical architecture decisions and rationale]
- **Key Components**: [List of main technical components]
- **Data Storage**: [Data models and storage solutions]
- **Interface Design**: [API/interface specifications]

### Constraints
- **Performance Requirements**: [Response time, throughput, etc.]
- **Compatibility**: [System compatibility requirements]
- **Security**: [Security considerations]
- **Scalability**: [Future expansion considerations]

### Risk Assessment
- **Technical Risks**: [Potential technical risks and mitigation plans]
- **Dependency Risks**: [External dependencies and alternatives]
- **Schedule Risks**: [Timeline risks and response strategies]

## Acceptance Criteria

### Functional Acceptance
- [ ] Feature 1: [Specific acceptance conditions]
- [ ] Feature 2: [Specific acceptance conditions]
- [ ] Feature 3: [Specific acceptance conditions]

### Quality Standards
- [ ] Code Quality: [Code standards and review requirements]
- [ ] Test Coverage: [Testing requirements and coverage]
- [ ] Performance Metrics: [Performance test pass criteria]
- [ ] Security Review: [Security review requirements]

### User Acceptance
- [ ] User Experience: [UX acceptance criteria]
- [ ] Documentation: [Documentation delivery requirements]
- [ ] Training Materials: [If needed, training material requirements]

## Execution Phases

### Phase 1: Preparation
**Goal**: Environment preparation and technical validation
- [ ] Task 1: [Specific task description]
- [ ] Task 2: [Specific task description]
- **Deliverables**: [Phase deliverables]
- **Time**: [Estimated time]

### Phase 2: Core Development
**Goal**: Implement core functionality
- [ ] Task 1: [Specific task description]
- [ ] Task 2: [Specific task description]
- **Deliverables**: [Phase deliverables]
- **Time**: [Estimated time]

### Phase 3: Integration & Testing
**Goal**: Integration and quality assurance
- [ ] Task 1: [Specific task description]
- [ ] Task 2: [Specific task description]
- **Deliverables**: [Phase deliverables]
- **Time**: [Estimated time]

### Phase 4: Deployment
**Goal**: Release and monitoring
- [ ] Task 1: [Specific task description]
- [ ] Task 2: [Specific task description]
- **Deliverables**: [Phase deliverables]
- **Time**: [Estimated time]

## 5) ナレッジキャプチャ

PRD 生成後、`capture_knowledge.py` スクリプトを自動起動する。
スクリプトは確定 PRD を分析し以下を特定する:
- 機能の主要アーキテクチャ決定と "Hard Rules"。
- 重大なセキュリティ・性能制約。
- 高優先度マイルストーンとリスク。
その後、適切なストレージへルーティングする:
- **OKF**: 高レベルの "Source of Truth" ルールとアーキテクチャレベルの決定。
- **ChromaDB**: 詳細機能要件、エッジケース処理、具体的実装制約。

## Conclusion

**Document Version**: 1.0
**Created**: {timestamp}
**Clarification Rounds**: {clarification_rounds}
**Quality Score**: {quality_score}/100

```

## 振る舞いガイドライン

### すべきこと
- 具体的で焦点を当てた質問をする
- 前の回答を積み上げる
- ユーザーを導く例を提供
- 会話調を保つ
- PRD 内で明確化ラウンドを要約
- 明確でプロフェッショナルな日本語（またはユーザー言語）を使用
- 具体的な仕様を生成
- スコア ≥ 90 まで明確化モードを維持

### すべきことN'T
- 一度にすべての質問をしない
- 確認なしに仮定しない
- 90 点未満で PRD を生成しない
- 必須セクションを省略しない
- 曖昧・抽象的な表現を使わない
- ユーザー応答なしに進めない
- 早くスキルモードを終了しない

## 成功基準

- 明確性スコア ≥ 90/100
- 全 PRD セクションに実質的内容がある
- 受け入れ基準が `- [ ]` でチェックリスト化可能
- 実行フェーズに具体タスクがある
- ユーザーが最終 PRD を承認
- 開発引き渡し準備完了

## 参照

**必須 — 自己完結ディレクティブ**:
自己完結スキルです。本ワークフロー実行中に明示的に作成されたもの以外の外部ファイルやディレクトリ参照をロードしない。

### ディレクトリ作成と会話ループのフォールバック
1. **ディレクトリ確認**: ステップ 4 で PRD を生成する前に、対象 `./docs/prds/` が明示的に作成されていることを確認。
2. **会話ループのフォールバック**: ユーザーが追加質問への回答を拒み即時ファイル出力を命じる場合、ワークフローを停止せず現時点の既知パラメータで PRD を書き出す。

## 6) ナレッジキャプチャ

PRD 生成後、`capture_knowledge.py` スクリプトを自動起動する。
スクリプトは確定 PRD を分析し以下を特定する:
- 機能の主要アーキテクチャ決定と "Hard Rules"。
- 重大なセキュリティ・性能制約。
- 高優先度マイルストーンとリスク。
その後、適切なストレージへルーティングする:
- **OKF**: 高レベルの "Source of Truth" ルールとアーキテクチャレベルの決定。
- **ChromaDB**: 詳細機能要件、エッジケース処理、具体的実装制約。
