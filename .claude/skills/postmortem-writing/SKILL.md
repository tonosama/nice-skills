---
name: postmortem-writing
description: 根本原因分析・タイムライン・アクション項目付きの効果的な blameless ポストモーテムを作成する。インシデントレビュー、ポストモーテム文書作成、インシデント対応プロセス改善時に使用する。
---

# Postmortem Writing

組織学習を促しインシデント再発を防ぐ、効果的な blameless ポストモーテム作成の包括ガイド。

## このスキルを使わないとき

- タスクが postmortem writing と無関係な場合
- 本スコープ外の別ドメインやツールが必要な場合

## 手順

- 目的、制約、必要な入力を明確にする。
- 関連するベストプラクティスを適用し、成果を検証する。
- 実行可能なステップと検証方法を提供する。

## このスキルを使うとき

- インシデント後レビューを実施する
- ポストモーテム文書を書く
- blameless ポストモーテム会議をファシリテートする
- 根本原因と寄与要因を特定する
- 実行可能なフォローアップ項目を作成する
- 組織学習文化を構築する

## 中核概念

### 1. Blameless 文化

| Blame-Focused | Blameless |
|---------------|-----------|
| "Who caused this?" | "What conditions allowed this?" |
| "Someone made a mistake" | "The system allowed this mistake" |
| Punish individuals | Improve systems |
| Hide information | Share learnings |
| Fear of speaking up | Psychological safety |

### 2. ポストモーテムのトリガー

- SEV1 or SEV2 incidents
- Customer-facing outages > 15 minutes
- Data loss or security incidents
- Near-misses that could have been severe
- Novel failure modes
- Incidents requiring unusual intervention

## クイックスタート

### ポストモーテムのタイムライン
```
Day 0: Incident occurs
Day 1-2: Draft postmortem document
Day 3-5: Postmortem meeting
Day 5-7: Finalize document, create tickets
Week 2+: Action item completion
Quarterly: Review patterns across incidents
```

## テンプレート

### テンプレート 1: 標準ポストモーテム

```markdown
# Postmortem: [Incident Title]

**Date**: 2024-01-15
**Authors**: @alice, @bob
**Status**: Draft | In Review | Final
**Incident Severity**: SEV2
**Incident Duration**: 47 minutes

## Executive Summary

On January 15, 2024, the payment processing service experienced a 47-minute outage affecting approximately 12,000 customers. The root cause was a database connection pool exhaustion triggered by a configuration change in deployment v2.3.4. The incident was resolved by rolling back to v2.3.3 and increasing connection pool limits.

**Impact**:
- 12,000 customers unable to complete purchases
- Estimated revenue loss: $45,000
- 847 support tickets created
- No data loss or security implications

## Timeline (All times UTC)

| Time | Event |
|------|-------|
| 14:23 | Deployment v2.3.4 completed to production |
| 14:31 | First alert: `payment_error_rate > 5%` |
| 14:33 | On-call engineer @alice acknowledges alert |
| 14:35 | Initial investigation begins, error rate at 23% |
| 14:41 | Incident declared SEV2, @bob joins |
| 14:45 | Database connection exhaustion identified |
| 14:52 | Decision to rollback deployment |
| 14:58 | Rollback to v2.3.3 initiated |
| 15:10 | Rollback complete, error rate dropping |
| 15:18 | Service fully recovered, incident resolved |

## Root Cause Analysis

### What Happened

The v2.3.4 deployment included a change to the database query pattern that inadvertently removed connection pooling for a frequently-called endpoint. Each request opened a new database connection instead of reusing pooled connections.

### Why It Happened

1. **Proximate Cause**: Code change in `PaymentRepository.java` replaced pooled `DataSource` with direct `DriverManager.getConnection()` calls.

2. **Contributing Factors**:
   - Code review did not catch the connection handling change
   - No integration tests specifically for connection pool behavior
   - Staging environment has lower traffic, masking the issue
   - Database connection metrics alert threshold was too high (90%)

3. **5 Whys Analysis**:
   - Why did the service fail? → Database connections exhausted
   - Why were connections exhausted? → Each request opened new connection
   - Why did each request open new connection? → Code bypassed connection pool
   - Why did code bypass connection pool? → Developer unfamiliar with codebase patterns
   - Why was developer unfamiliar? → No documentation on connection management patterns

### System Diagram

```
[Client] → [Load Balancer] → [Payment Service] → [Database]
                                    ↓
                            Connection Pool (broken)
                                    ↓
                            Direct connections (cause)
```

## Detection

### What Worked
- Error rate alert fired within 8 minutes of deployment
- Grafana dashboard clearly showed connection spike
- On-call response was swift (2 minute acknowledgment)

### What Didn't Work
- Database connection metric alert threshold too high
- No deployment-correlated alerting
- Canary deployment would have caught this earlier

### Detection Gap
The deployment completed at 14:23, but the first alert didn't fire until 14:31 (8 minutes). A deployment-aware alert could have detected the issue faster.

## Response

### What Worked
- On-call engineer quickly identified database as the issue
- Rollback decision was made decisively
- Clear communication in incident channel

### What Could Be Improved
- Took 10 minutes to correlate issue with recent deployment
- Had to manually check deployment history
- Rollback took 12 minutes (could be faster)

## Impact

### Customer Impact
- 12,000 unique customers affected
- Average impact duration: 35 minutes
- 847 support tickets (23% of affected users)
- Customer satisfaction score dropped 12 points

### Business Impact
- Estimated revenue loss: $45,000
- Support cost: ~$2,500 (agent time)
- Engineering time: ~8 person-hours

### Technical Impact
- Database primary experienced elevated load
- Some replica lag during incident
- No permanent damage to systems

## Lessons Learned

### What Went Well
1. Alerting detected the issue before customer reports
2. Team collaborated effectively under pressure
3. Rollback procedure worked smoothly
4. Communication was clear and timely

### What Went Wrong
1. Code review missed critical change
2. Test coverage gap for connection pooling
3. Staging environment doesn't reflect production traffic
4. Alert thresholds were not tuned properly

### Where We Got Lucky
1. Incident occurred during business hours with full team available
2. Database handled the load without failing completely
3. No other incidents occurred simultaneously

## Action Items

| Priority | Action | Owner | Due Date | Ticket |
|----------|--------|-------|----------|--------|
| P0 | Add integration test for connection pool behavior | @alice | 2024-01-22 | ENG-1234 |
| P0 | Lower database connection alert threshold to 70% | @bob | 2024-01-17 | OPS-567 |
| P1 | Document connection management patterns | @alice | 2024-01-29 | DOC-89 |
| P1 | Implement deployment-correlated alerting | @bob | 2024-02-05 | OPS-568 |
| P2 | Evaluate canary deployment strategy | @charlie | 2024-02-15 | ENG-1235 |
| P2 | Load test staging with production-like traffic | @dave | 2024-02-28 | QA-123 |

## Appendix

### Supporting Data

#### Error Rate Graph
[Link to Grafana dashboard snapshot]

#### Database Connection Graph
[Link to metrics]

### Related Incidents
- 2023-11-02: Similar connection issue in User Service (POSTMORTEM-42)

### References
- [Connection Pool Best Practices](internal-wiki/connection-pools)
- [Deployment Runbook](internal-wiki/deployment-runbook)
```

### テンプレート 2: 5 Whys 分析

```markdown
# 5 Whys Analysis: [Incident]

## Problem Statement
Payment service experienced 47-minute outage due to database connection exhaustion.

## Analysis

### Why #1: Why did the service fail?
**Answer**: Database connections were exhausted, causing all new requests to fail.

**Evidence**: Metrics showed connection count at 100/100 (max), with 500+ pending requests.

---

### Why #2: Why were database connections exhausted?
**Answer**: Each incoming request opened a new database connection instead of using the connection pool.

**Evidence**: Code diff shows direct `DriverManager.getConnection()` instead of pooled `DataSource`.

---

### Why #3: Why did the code bypass the connection pool?
**Answer**: A developer refactored the repository class and inadvertently changed the connection acquisition method.

**Evidence**: PR #1234 shows the change, made while fixing a different bug.

---

### Why #4: Why wasn't this caught in code review?
**Answer**: The reviewer focused on the functional change (the bug fix) and didn't notice the infrastructure change.

**Evidence**: Review comments only discuss business logic.

---

### Why #5: Why isn't there a safety net for this type of change?
**Answer**: We lack automated tests that verify connection pool behavior and lack documentation about our connection patterns.

**Evidence**: Test suite has no tests for connection handling; wiki has no article on database connections.

## Root Causes Identified

1. **Primary**: Missing automated tests for infrastructure behavior
2. **Secondary**: Insufficient documentation of architectural patterns
3. **Tertiary**: Code review checklist doesn't include infrastructure considerations

## Systemic Improvements

| Root Cause | Improvement | Type |
|------------|-------------|------|
| Missing tests | Add infrastructure behavior tests | Prevention |
| Missing docs | Document connection patterns | Prevention |
| Review gaps | Update review checklist | Detection |
| No canary | Implement canary deployments | Mitigation |
```

### テンプレート 3: クイックポストモーテム（軽微インシデント）

```markdown
# Quick Postmortem: [Brief Title]

**Date**: 2024-01-15 | **Duration**: 12 min | **Severity**: SEV3

## What Happened
API latency spiked to 5s due to cache miss storm after cache flush.

## Timeline
- 10:00 - Cache flush initiated for config update
- 10:02 - Latency alerts fire
- 10:05 - Identified as cache miss storm
- 10:08 - Enabled cache warming
- 10:12 - Latency normalized

## Root Cause
Full cache flush for minor config update caused thundering herd.

## Fix
- Immediate: Enabled cache warming
- Long-term: Implement partial cache invalidation (ENG-999)

## Lessons
Don't full-flush cache in production; use targeted invalidation.
```

## ファシリテーションガイド

### ポストモーテム会議の進行

```markdown
## Meeting Structure (60 minutes)

### 1. Opening (5 min)
- Remind everyone of blameless culture
- "We're here to learn, not to blame"
- Review meeting norms

### 2. Timeline Review (15 min)
- Walk through events chronologically
- Ask clarifying questions
- Identify gaps in timeline

### 3. Analysis Discussion (20 min)
- What failed?
- Why did it fail?
- What conditions allowed this?
- What would have prevented it?

### 4. Action Items (15 min)
- Brainstorm improvements
- Prioritize by impact and effort
- Assign owners and due dates

### 5. Closing (5 min)
- Summarize key learnings
- Confirm action item owners
- Schedule follow-up if needed

## Facilitation Tips
- Keep discussion on track
- Redirect blame to systems
- Encourage quiet participants
- Document dissenting views
- Time-box tangents
```

## 避けるべきアンチパターン

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| **Blame game** | Shuts down learning | Focus on systems |
| **Shallow analysis** | Doesn't prevent recurrence | Ask "why" 5 times |
| **No action items** | Waste of time | Always have concrete next steps |
| **Unrealistic actions** | Never completed | Scope to achievable tasks |
| **No follow-up** | Actions forgotten | Track in ticketing system |

## ベストプラクティス

### 推奨
- **Start immediately** - Memory fades fast
- **Be specific** - Exact times, exact errors
- **Include graphs** - Visual evidence
- **Assign owners** - No orphan action items
- **Share widely** - Organizational learning

### 非推奨
- **Don't name and shame** - Ever
- **Don't skip small incidents** - They reveal patterns
- **Don't make it a blame doc** - That kills learning
- **Don't create busywork** - Actions should be meaningful
- **Don't skip follow-up** - Verify actions completed

## Resources

- [Google SRE - Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)
- [Etsy's Blameless Postmortems](https://codeascraft.com/2012/05/22/blameless-postmortems/)
- [PagerDuty Postmortem Guide](https://postmortems.pagerduty.com/)


## Memory Sync

重要な技術的所見、アーキテクチャ上の決定、コードリファクタリング、リスク評価を完了したあと、ローカルの memory capture を**必ず**起動する。

1. 最終サマリーまたは成果物をプロジェクトディレクトリ内の Markdown ファイルとして保存する。
2. capture スクリプトを実行する：
   ```bash
   python3 ~/memory_system/capture_knowledge.py <file_path>
   ```
3. これにより、新しい学び、ポリシー、技術スニペットが正しいローカルストレージ（OKF または ChromaDB）へ自動的にルーティングされる。
