# Clean Architecture & Refactoring: Set 4
## Testing Strategies, Cross-Cutting Concerns & Evolutionary Refactoring

## 📘 10 Conceptual Questions
1. **Test-Driven Refactoring Workflow:** How do you safely refactor code that has zero tests? Explain the "characterize → refactor → verify" cycle and why capturing current behavior (even if flawed) is safer than guessing requirements.
2. **Cross-Cutting Concerns Placement:** Where do logging, caching, metrics, and authorization belong in clean architecture? How do you avoid scattering `Cache::remember()` or `Log::info()` throughout business logic without violating layer boundaries?
3. **Replace Conditional with Polymorphism:** Why do long `if/else` or `switch` chains become maintenance traps? How do you refactor them into Strategy or State patterns without creating class explosion or scattering logic?
4. **Testing Boundaries & Mocking Strategy:** What should you mock vs. fake vs. use real implementations? How does over-mocking create brittle tests that pass locally but break in production, and what's the alternative?
5. **Decorator Pattern for Infrastructure Concerns:** How do you wrap business services with decorators for caching, retry logic, or audit logging without polluting the core domain? When does decoration become over-engineering?
6. **Testing Pure Domain Logic:** How do you write unit tests that run in <5ms without bootstrapping Laravel, hitting the database, or using facades? Explain why domain tests should be framework-agnostic and how that changes test structure.
7. **Data Clumps & Primitive Obsession:** When should related primitives (e.g., `$startDate`, `$endDate`, `$timezone`) be grouped into a value object? How does this improve validation, type safety, and domain expressiveness?
8. **Evolutionary Design & Fitness Functions:** How do you automate architectural checks (e.g., "no domain imports infrastructure", "controllers < 100 lines") in CI? Why are automated fitness functions better than manual reviews for enforcing standards?
9. **Refactoring Workflow Discipline:** What's the difference between refactoring and rewriting? How do you apply the Boy Scout Rule, micro-commits, and feature toggles to refactor incrementally without blocking releases?
10. **The "YAGNI" vs. "Over-Engineering" Balance:** How do you know when a pattern (CQRS, DDD, Repository) is necessary vs. premature? What signals indicate it's time to introduce abstraction, and what signals say "keep it simple"?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Extract Strategy Pattern**<br>Find a method with 4+ `if/else` branches (e.g., discount calculation, shipping routing). Refactor into a `ShippingStrategy` interface + concrete implementations. Resolve at composition root only. | Conditional chain eliminated. Each strategy isolated & testable. New strategy added without modifying existing code. OCP verified. |
| 2 | **Implement State Pattern**<br>Replace string-based status checks (`if ($order->status == 'shipped')`) with a `OrderState` class hierarchy (`PendingState`, `PaidState`, `ShippedState`). State transitions are explicit methods. | Status logic encapsulated. Invalid transitions throw exceptions. No `if/else` on status strings. Tests verify state machine behavior. |
| 3 | **Decorator for Cross-Cutting Concerns**<br>Wrap a service method with a `CachedOrderService` decorator. Implement caching at the boundary, not inside business logic. Verify cache invalidation on state changes. | Business logic contains zero `Cache::` calls. Decorator handles caching cleanly. Invalidation is explicit. Tests swap real/decorated implementations easily. |
| 4 | **Refactor Data Clumps to Value Objects**<br>Group related primitives (e.g., `$price`, `$currency`, `$taxRate`) into a `Money` or `PricingContext` value object. Add validation, formatting, and arithmetic methods. Update consumers. | Zero scattered primitives for the concept. IDE autocomplete & static analysis catch invalid values. Arithmetic is type-safe. |
| 5 | **Pure PHP Domain Unit Test**<br>Write a test for a pricing calculation that uses *no* database, *no* Laravel facades, and *no* HTTP layer. Use simple PHP objects. Verify it runs in <5ms and requires zero framework bootstrap. | Test passes offline. Zero framework imports. Logic verified purely via inputs/outputs. Fast, deterministic, framework-agnostic. |
| 6 | **Boundary Contract Testing**<br>Write integration tests that only cross layer boundaries (Controller → Service, Service → Repository). Mock everything outside the boundary. Verify data contracts, not internal implementation. | Tests catch API/contract breaks. Zero over-mocking. Fails gracefully on signature changes. Coverage focuses on seams, not internals. |
| 7 | **Lightweight CQRS Split**<br>Separate a read-heavy query from write logic. Create a `ProductListView` DTO populated via query builder (bypassing Eloquent relationships). Measure latency vs direct ORM. | Read query executes in <50ms. Zero joins on hot path. DTO structures data cleanly. Write path unaffected. |
| 8 | **Architecture Kata Refactor**<br>Take a tangled 200-line service. Extract responsibilities into focused classes. Apply SOLID. Write characterization tests first. Refactor safely. Document before/after metrics. | Original class shrinks >60%. New classes have single responsibility. Tests verify each concern independently. No behavior regression. |
| 9 | **Fitness Function CI Gate**<br>Install `phparkitect` or write PHPUnit assertions. Enforce: Controllers don't touch DB directly, Domain has zero `Illuminate\` imports, Services depend on interfaces. Fail CI on violation. | Architecture rules enforced automatically. PRs blocked on breach. Rules documented. Zero manual policing needed. |
| 10| **Refactoring Workflow Guide**<br>Document a team-safe refactoring process: characterization tests → micro-commits → feature flags → verification → cleanup. Apply it to one MultiMart module. Share as `REFACTORING.md`. | Process is repeatable. Refactoring completes without downtime or regressions. Guide covers rollback, testing, and communication steps. |
