# Laravel Reliability, Testing & Quality Engineering: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **Mutation Testing vs Code Coverage:** Why is 100% line coverage a misleading metric? How does mutation testing measure actual test effectiveness, and what does a "surviving mutant" reveal about your test suite?
2. **Property-Based vs Example-Based Testing:** How does testing invariants and properties (e.g., "cart total never decreases when items are added") catch edge cases that hardcoded examples miss? When does fuzzing outperform traditional assertions?
3. **Contract Testing for APIs:** How do consumer-driven contract tests prevent integration breakages between Laravel APIs and frontend/mobile consumers? When should you use PACT vs schema validation vs simple `assertJsonStructure()`?
4. **Database State in Tests:** Compare `RefreshDatabase`, `DatabaseTransactions`, and `DatabaseTruncation`. When does each cause flaky tests, deadlocks, or CI slowdowns? How do you safely test cross-connection queries or raw SQL?
5. **Testing Asynchronous & Queue Workflows:** How do you reliably test jobs that retry, delay, chain, or depend on external services? Explain `Queue::fake()`, `Bus::fake()`, and how to verify side effects without race conditions or real network calls.
6. **Flaky Test Anatomy & Prevention:** What causes flaky tests in Laravel (time zones, random data, shared static state, async timing)? How do you systematically identify, isolate, and eliminate them before they poison CI?
7. **Security Testing Automation:** How do you automate security validation in Laravel? Explain CSRF/XSS test cases, mass assignment verification, dependency scanning (`composer audit`), and how to integrate OWASP checks into your pipeline.
8. **Performance Budgets in CI:** How do you measure and enforce performance constraints? Explain query count assertions, memory leak detection, execution time thresholds, and how to fail builds on regression without false positives.
9. **Test Data Strategy & Factory Design:** How do you design factories that produce realistic, boundary-condition data without hardcoding? Explain sequences, states, seeded randomization, and how to avoid "test data pollution" across suites.
10. **Zero-Downtime Test-Driven Refactoring:** How do you safely refactor legacy Laravel code without breaking behavior? Explain characterization tests, approval testing, and the "test → refactor → verify" loop for fat controllers or tangled models.

---

## ️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Invariant/Property Testing**<br>Write a test for `CartService::calculateTotal()` that verifies: total never decreases when items are added, discounts cap at 100%, tax applies correctly across boundaries. Use seeded random inputs. | Test passes 100+ random iterations. Catches at least 1 edge case traditional tests would miss. No hardcoded values. Runs deterministically with seed. |
| 2 | **Mutation Testing Setup**<br>Install Infection PHP or Laravel Mutation Testing. Run against `CartService` or `ProductPolicy`. Identify surviving mutants. Write targeted tests to kill them. Document mutation score vs coverage. | Mutation score >80%. Surviving mutants documented. Tests added specifically to kill them. CI records score trend. |
| 3 | **API Contract Validation**<br>Create a contract test for `/api/products` that validates: JSON schema, required fields, pagination structure, error format, and content-type headers. Ensure it fails if contract breaks. | Test blocks breaking changes. Schema validates nested relationships. Error responses match spec. Runs in <50ms. |
| 4 | **Flaky Test Diagnosis & Fix**<br>Write a test that intentionally uses `now()`, random IDs, or shared static state. Run it 20 times in parallel. Fix using `Carbon::setTestNow()`, deterministic seeds, proper teardown. | Test passes 100/100 parallel runs. No shared state leaks. Time-dependent logic isolated. CI reports zero flakes. |
| 5 | **Queue Side-Effect Verification**<br>Test a job that sends email, updates inventory, and dispatches another job. Use `Mail::fake()`, `Bus::fake()`. Assert chaining, order, payload. Verify no real side effects. | All fakes capture correct calls. Chaining order verified. Zero external calls made. Test runs <20ms. |
| 6 | **Performance Budget Enforcement**<br>Add assertions to feature tests: `assertQueryCountLessThan(15)`, `assertMemoryUsageLessThan('64M')`, `assertExecutionTimeLessThan(200)`. Fail CI on regression. | Tests fail when budgets exceeded. Metrics logged. No false positives on cache-warmed runs. CI blocks regressions. |
| 7 | **Security Test Suite**<br>Write tests verifying: CSRF tokens reject POST without `_token`, mass assignment blocks unexpected fields, XSS payloads are escaped in Blade, rate limiting returns 429 consistently. | All security vectors covered. Tests pass on clean install. Blade rendering verified. Rate limiter respects config. |
| 8 | **Characterization Tests for Legacy**<br>Take a complex `CheckoutController` method. Write tests that capture current behavior (inputs/outputs). Refactor to `CheckoutService` without changing behavior. | Tests pass before/after refactor. Behavior unchanged. Controller shrinks to <8 lines. Service is unit-testable. |
| 9 | **Advanced Factory Design**<br>Create `OrderFactory` with `state('cancelled')`, `sequence()`, realistic random data, and `->recycle()`. Seed 100 orders. Verify distribution, relationships, and query performance. | Factory produces valid, diverse data. Relationships persist. Queries perform efficiently. No N+1 in seed. |
| 10| **CI Quality Gates Pipeline**<br>Add `phpstan`, `pest --coverage`, mutation test, `composer audit`, and performance assertions to GitHub Actions. Set minimum thresholds. Block merges if quality drops. | Pipeline runs on PR. Fails below thresholds. Reports visible. Zero-downtime merge policy enforced. |
