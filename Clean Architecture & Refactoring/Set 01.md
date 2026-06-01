# Clean Architecture & Refactoring: Set 1
## Foundational Principles, Layer Separation & Code Hygiene

## 📘 10 Conceptual Questions
1. **SOLID in Laravel Context:** How does each principle (SRP, OCP, LSP, ISP, DIP) manifest in real Laravel code? Give concrete examples of violating vs. adhering to each in controllers, models, and services.
2. **Dependency Direction Rule:** Why should high-level modules (business logic/use cases) never depend on low-level modules (frameworks, DB, HTTP)? How does this rule change how you structure Laravel apps?
3. **Fat Controller Symptoms & Remedies:** What signals indicate a controller has grown too fat? How do Action Classes, Form Requests, Policies, and Services redistribute responsibility without over-engineering?
4. **Domain vs. Infrastructure Layers:** What belongs in the Domain layer (pure PHP, business rules, entities) vs. Infrastructure (Laravel, Eloquent, HTTP, queues)? Why is Eloquent technically an infrastructure detail, not domain logic?
5. **DTOs & Data Boundaries:** When should you use Data Transfer Objects instead of passing Eloquent models or arrays between layers? How do DTOs improve immutability, type safety, and decoupling?
6. **Refactoring vs. Rewriting:** What's the difference? Why is incremental "strangler fig" refactoring safer than full rewrites? How do you identify code smells that warrant refactoring vs. features that warrant new architecture?
7. **Tell, Don't Ask Principle:** How does shifting from procedural data extraction (`if ($order->status == 'paid')`) to behavioral delegation (`$order->markAsPaid()`) reduce coupling and improve encapsulation?
8. **Law of Demeter & Deep Chains:** Why is `$order->user->address->city->name` a code smell? How do you refactor deep object graphs without bloating models or breaking encapsulation?
9. **Testing Layered Architecture:** How do unit tests differ from integration tests in clean architecture? Why should domain tests run without Laravel bootstrapped, and how does that change your test suite structure?
10. **Modular Boundaries & Cross-Communication:** How do you split a monolithic Laravel app into logical modules (e.g., `Catalog`, `Billing`, `Cart`) without creating tight coupling? What should cross-module communication look like?

---

## ️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Extract Action Class**<br>Take a fat controller method (e.g., `CheckoutController@store`). Extract business logic into a `ProcessCheckout` action class. Controller should only handle HTTP request/response. | Controller method < 8 lines. Action contains validation, persistence, events. Tests mock action, not controller. No business logic in HTTP layer. |
| 2 | **DTO Boundary Implementation**<br>Replace array/Eloquent model passing in a service method with a strict `CheckoutData` DTO. Validate input at boundary, pass DTO inward. Verify type safety & immutability. | DTO uses `readonly` properties or getters. No array spreading. Type hints enforced. IDE autocomplete works. Refactoring doesn't break existing flow. |
| 3 | **Dependency Inversion Refactor**<br>Identify a class directly instantiating a concrete dependency (e.g., `new Mailer()` or `new StripeClient()`). Refactor to constructor injection via interface. Register in container. Mock in test. | Class depends on interface, not implementation. Container resolves correctly. Test passes with mock. No `new` keyword for external services in business logic. |
| 4 | **Law of Demeter Fix**<br>Find a deep chain like `$product->category->manager->email`. Refactor using delegation methods (`$product->getManagerEmail()`) or query scopes. Remove tight coupling. | Chain broken. Calling code only talks to direct neighbor. Model encapsulates navigation logic. Tests verify delegation without exposing internals. |
| 5 | **Tell, Don't Ask Refactor**<br>Convert procedural status checks (`if ($cart->isEmpty())` or `if ($order->canBeCancelled())`) into behavioral methods (`$cart->hasItems()`, `$order->cancel()`). Move state checks into owning class. | External code never reads internal state directly. Class exposes intent, not data. Tests verify behavior, not property values. Coupling reduced. |
| 6 | **Module Boundary Extraction**<br>Create a `Modules/Catalog` directory. Move `Product`, `Category`, and related controllers/services there. Ensure no direct imports from `Modules/Billing` or `Modules/Cart`. Use events/interfaces for cross-module calls. | `Catalog` has zero dependencies on other modules. Communication happens via published events or shared interfaces. `composer dump-autoload` shows clear separation. |
| 7 | **Form Request + Policy Separation**<br>Move validation from controller to `StoreProductRequest`. Move authorization to `ProductPolicy`. Controller method routes request, calls service, returns response. Test unauthorized/invalid scenarios. | Controller < 5 lines. Validation/authorization isolated. Policy enforces business rules. Tests cover 403/422/200 paths cleanly. |
| 8 | **Domain Event Extraction**<br>Create a plain PHP `OrderPlaced` event (zero Laravel dependencies). Dispatch it from domain layer. Handle it in infrastructure layer (mail, queue). Verify domain layer has no framework imports. | Event class is pure PHP. Handler lives in infrastructure. Domain layer remains framework-agnostic. Test verifies event payload without bootstrapping Laravel. |
| 9 | **Refactor God Class**<br>Identify a model/controller with >300 lines or >10 responsibilities. Split into focused classes (e.g., `ProductPricing`, `ProductInventory`, `ProductMedia`). Use composition over inheritance. | Original class shrinks >60%. New classes have single responsibility. Tests verify each concern independently. No behavior regression. |
| 10| **Architecture Linting Setup**<br>Install `larastan`/`phpstan` with strict rules. Enforce: no `mixed` types, return type declarations, ban `dd()`/`dump()` in src, require PHPDoc for complex methods. Fix all errors. Commit config. | `vendor/bin/phpstan analyse` passes level 8+. Zero `dd()` in src. CI blocks non-compliant PRs. Rules documented in `README.md`. |
