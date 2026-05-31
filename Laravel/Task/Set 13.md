# Laravel Production Readiness, Team Dynamics & Sustainable Engineering: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **Technical Debt Management:** How do you identify, quantify, and prioritize technical debt? When is it acceptable to ship quick fixes vs refactor, and how do you prevent debt from becoming architectural bankruptcy?
2. **Architecture Decision Records (ADRs):** Why document architectural choices? What should an ADR contain (context, decision, consequences, status), and how do you prevent documentation rot in fast-moving teams?
3. **Incident Response & Blameless Post-Mortems:** How do you handle production outages systematically? Explain detection, mitigation, communication, rollback procedures, and how to extract learnings without finger-pointing.
4. **Team Code Review Culture:** What makes a PR review effective? How do you balance velocity vs quality, handle architectural disagreements, onboard new developers, and prevent review bottlenecks?
5. **Environment Parity & Local Dev Strategy:** How do you ensure dev/staging/prod behave identically? Explain Docker, `.env` management, local service mocking, and why "works on my machine" is a symptom of broken parity.
6. **Feature Flags & Progressive Delivery:** How do you decouple deployment from release? Explain dark launches, percentage rollouts, user-segment targeting, and how to enforce flag cleanup to avoid "flag pollution".
7. **Legacy Code Refactoring:** How do you safely modernize tangled codebases? Explain characterization tests, strangler fig pattern, incremental extraction, and how to refactor without stopping feature delivery.
8. **Backup, Recovery & Disaster Planning:** How do you guarantee data recoverability? Explain automated backups, restore testing, point-in-time recovery, RTO/RPO definitions, and why untested backups are worse than no backups.
9. **Cost-Aware Architecture:** How do you optimize cloud/database costs without sacrificing performance? Explain query budgeting, storage lifecycle, queue scaling efficiency, and how to monitor spend alongside latency.
10. **Business-Technical Alignment:** How do you translate vague requirements into testable, maintainable code? Explain domain storytelling, acceptance criteria mapping, handling scope changes, and protecting architecture from short-term pressure.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Write an ADR**<br>Document your multi-image upload architecture in `docs/adr/001-product-image-storage.md`. Include context, considered options, decision, consequences, and status. Link to it from README. | ADR follows standard template. Clear trade-offs documented. Status tracked (`Proposed`/`Accepted`/`Deprecated`). Team can reference it for future decisions. |
| 2 | **Simulate Production Incident**<br>Kill a queue worker, trigger a 500 error in checkout, execute rollback, then write a blameless post-mortem template covering timeline, impact, root cause, mitigation, and prevention actions. | Runbook created. Rollback tested safely. Post-mortem focuses on systems, not people. Action items assigned with owners/dates. |
| 3 | **Feature Flag System**<br>Implement database-backed flags with `isEnabled()`, percentage rollout, user targeting, and auto-cleanup after 30 days. Admin UI to toggle. Test dark launch without code changes. | Flags control behavior instantly. Rollout consistent via hash. Cleanup runs via scheduler. Zero conditional spaghetti in core logic. |
| 4 | **Legacy Refactor with Characterization Tests**<br>Write tests that capture current behavior of a fat controller/model. Refactor to service/action layer. Verify external behavior unchanged. Document before/after metrics. | Tests pass before/after. Controller shrinks to <8 lines. Service is unit-testable. No regression in functionality or performance. |
| 5 | **Docker Local Dev Parity**<br>Create `docker-compose.yml` for app, DB, Redis, Meilisearch, Mailpit. Ensure `.env.example` is complete. Verify `docker compose up` matches prod services. Document setup. | One-command local setup. Services mirror production. No manual dependency installs. README covers first-run steps and common issues. |
| 6 | **PR Review Checklist & Process**<br>Create `docs/PULL_REQUEST_TEMPLATE.md` and `docs/CODE_REVIEW_CHECKLIST.md` covering security, performance, testing, architecture, and DX. Run a mock review with structured feedback. | Checklist enforced in CI or PR description. Reviews focus on logic, not style. Bottlenecks identified. New devs onboarded faster. |
| 7 | **Backup & Restore Drill**<br>Automate DB backups via cron/script. Store in S3/local. Test full restore on staging. Document RTO (<15min) and RPO (<5min). Verify point-in-time recovery capability. | Backup runs reliably. Restore tested successfully. RTO/RPO met. Documentation covers emergency steps. No data loss in drill. |
| 8 | **Query Budgeting in CI**<br>Add assertions to feature tests: `assertQueryCountLessThan()`, `assertNoNPlusOne()`. Set thresholds. Fail PRs that exceed budgets. Document baselines per endpoint. | CI blocks performance regressions. Query counts tracked. N+1 caught early. Baselines updated when optimizations land. |
| 9 | **Common Failure Runbook**<br>Document detection, mitigation, and escalation for: queue backup, cache miss storms, DB connection exhaustion, payment webhook delays. Include commands, dashboards, and contact paths. | Runbook covers 4+ scenarios. Steps are executable. Dashboards/alerts referenced. Team can respond without tribal knowledge. |
| 10| **Requirement → Architecture Mapping**<br>Take vague requirement: "Users should get notified when prices drop." Break into user stories, acceptance criteria, domain events, data model changes, and minimal implementation plan. | Clear scope boundaries. Testable acceptance criteria. Domain events identified. Implementation plan respects existing architecture. No over-engineering. |
