# Clean Architecture & Refactoring: Set 3
## Domain-Driven Refactoring & Boundary Mapping

## 📘 10 Conceptual Questions
1. **Aggregate Roots:** How do you decide which model is the "root" of a cluster of objects (e.g., `Order` vs. `OrderItem`)? Why should child objects only be modified through the root, and how does this prevent inconsistent state?
2. **Domain Events vs. Framework Events:** Laravel's event system is convenient, but why is coupling business logic directly to `Illuminate\Events` considered a framework leak? How do you use plain PHP objects for domain events and map them to framework listeners only at the boundary?
3. **Transactional Boundaries:** How does wrapping a multi-step process in a single database transaction at the *Service Layer* differ from doing it in the Controller? Why is "Unit of Work" often implicit in Laravel, and when do you need to make it explicit?
4. **CQRS in Practice:** You know the theory, but how do you implement Command Query Responsibility Segregation in a Laravel app without over-engineering? When do you need separate read models (DTOs/Views) vs. just using Eloquent for everything?
5. **Repository Pattern Reality Check:** Is the Repository pattern an anti-pattern in Laravel because Eloquent is already an Active Record implementation? When does wrapping Eloquent in a repository add value, and when is it just "fake abstraction"?
6. **Refactoring Observers to Services:** Laravel Observers are great for "fire and forget," but they often hide complex logic. How do you refactor a fat Observer (e.g., `OrderObserver::saved`) into a domain service or event handler to improve testability?
7. **Optimistic vs. Pessimistic Locking:** How do you handle race conditions when two users update the same resource simultaneously? Compare `lockForUpdate()` (pessimistic) vs. `version` columns (optimistic) and explain when to use each.
8. **Testing Domain Logic in Isolation:** How do you write unit tests for business rules without hitting the database or loading the Laravel framework? Explain the use of "Fakes" and "Value Objects" to test pure logic in milliseconds.
9. **Cross-Cutting Concerns:** Where do you put logic that touches multiple domains, like "Audit Logging" or "Notifications"? Explain how to use Domain Events to trigger these side-effects without polluting the core business logic.
10. **Handling Long-Running Processes (Sagas):** When a business process takes minutes or hours (e.g., KYC verification, shipping approval), how do you persist the state and manage retries without blocking the request or losing progress?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Define an Aggregate Root**<br>Refactor `Order` and `OrderItems` so that `OrderItems` can *only* be modified via methods on the `Order` entity (e.g., `$order->addItem()`). Remove direct access to `OrderItem::save()`. | Child objects cannot be persisted independently. All changes go through the root. Consistency rules (e.g., "total must match items") are enforced by the root. |
| 2 | **Decouple Domain Events**<br>Create a plain PHP `OrderShipped` event class (zero Laravel imports). Dispatch it from the domain layer. Create a Laravel listener that maps it to a notification. Verify the domain layer has no framework imports. | Domain event is pure PHP. Listener lives in infrastructure. Domain layer is testable without Laravel. Event payload is immutable. |
| 3 | **Implement Optimistic Locking**<br>Add a `version` column to a high-contention model. Implement a check that throws an exception if the version doesn't match on save. Write a test simulating concurrent updates. | Concurrent updates fail safely with a clear error. Version increments correctly. No data overwrite. UI shows conflict message. |
| 4 | **Extract CQRS Read Model**<br>Create a "Product Summary" view that joins products, categories, and current stock. Bypass Eloquent relationships for this view; use raw SQL or query builder. Map results to a DTO. | Read query executes in <50ms. Zero joins on hot path. DTO structures data cleanly. Write path unaffected. |
| 5 | **Refactor Fat Observer**<br>Take a model Observer with >20 lines of logic. Move that logic to a dedicated Service class or Domain Event handler. The Observer should only trigger the service/handler. | Observer is <5 lines. Logic is testable in isolation. No side-effects in the model itself. Tests mock the service, not the observer. |
| 6 | **Transaction Boundary Enforcement**<br>Wrap a complex multi-step process (e.g., checkout) in a single transaction at the Service Layer level, not the Controller. Ensure that if step 3 fails, steps 1 and 2 roll back. | All or nothing persistence. Controller is unaware of transaction details. Service method is atomic. Tests verify rollback on failure. |
| 7 | **Pure PHP Unit Test**<br>Write a test for a pricing calculation that uses *no* database, *no* Laravel facades, and *no* HTTP layer. Use simple PHP objects. Verify it runs in <5ms. | Test passes offline. Zero framework imports. Logic is verified purely via inputs/outputs. Fast execution. |
| 8 | **Repository Pattern Implementation**<br>Create an interface `ProductRepository`. Implement `EloquentProductRepository`. Inject the interface into a Service. Mock the interface in a unit test. | Service depends on interface, not Eloquent. Test passes with mock. Real implementation uses `where()` clauses. No `Product::` calls in service. |
| 9 | **Saga/Process Manager Stub**<br>Create a state-machine-like structure for a "User Onboarding" process (steps: Email Verify -> Payment -> Welcome). Persist state in DB. Allow resuming from the last successful step. | State is persisted between steps. Failed steps can be retried. Process tracks progress. No step is skipped. |
| 10| **Audit Trail via Events**<br>Implement a global listener for domain events that logs "User X performed Action Y" to a database table. Ensure the logging is asynchronous and doesn't block the main flow. | Audit records are created for key actions. Logging is queued/async. Main flow performance is unaffected. Audit table is queryable. |
