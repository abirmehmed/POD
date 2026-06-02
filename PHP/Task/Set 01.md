# Advanced PHP: Set 1
## Modern PHP (8.1–8.3+) & Advanced Object-Oriented Design

## 📘 10 Conceptual Questions
1. **Enums vs. Constants vs. Classes:** Why are PHP 8.1 Backed Enums superior to class constants for defining fixed sets of values? How do they improve type safety, IDE autocompletion, and prevent invalid state compared to passing raw strings or integers?
 is the difference between `readonly` properties (PHP 8.1) and `readonly` classes (PHP 8.2)? Why are they the ultimate tool for creating immutable Value Objects, and what are the limitations (e.g., nested mutable objects)?
3. **Match Expression vs. Switch Statement:** Beyond syntax, how does `match` differ from `switch` in terms of return values, strict type checking (`===`), and fall-through behavior? Why is `match` considered safer and more functional?
4. **Attributes (Annotations) & Reflection:** How do PHP 8 Attributes replace legacy DocBlock annotations (like in Doctrine or older Laravel)? Explain how the `Reflection` API reads these at runtime, and why they are parsed at compile-time for better performance.
5. **Union vs. Intersection Types:** What is the semantic difference between `int|float` (Union) and `Countable&Iterator` (Intersection)? When should you use each to accurately model domain logic and enforce strict contracts?
6. **The Pitfalls of Traits:** Why are Traits often considered a code smell or an anti-pattern in advanced OOP? Explain the problems of hidden dependencies, naming collisions, and violated Liskov Substitution Principle (LSP), and why Composition is usually superior.
 is Constructor Property Promotion?** How does it reduce boilerplate, and what are its limitations (e.g., can you add DocBlocks or Attributes to promoted properties in PHP 8.0 vs 8.1+)?
8. **Named Arguments & Backward Compatibility:** How do named arguments (`function(foo: 1, bar: 2)`) improve readability? Conversely, why do they make changing the *names* of parameters in a public API a breaking change, even if the types remain the same?
9. **WeakMaps & Memory Management:** What is a `WeakMap` (PHP 8.0), and how does it differ from a standard array? Explain how it prevents memory leaks by allowing objects to be garbage collected even if they are used as keys in the map.
10. **Final Classes & Methods:** Why should you default to making your classes and methods `final`? How does this enforce the "Composition over Inheritance" principle and prevent unexpected behavior in child classes?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Refactor to Backed Enums**<br>Find a model or service using string/int constants for status (e.g., `STATUS_ACTIVE = 'active'`). Refactor it to a PHP 8.1 Backed Enum. Update all usages to enforce strict type hints (`Status $status`). | Invalid statuses cause a TypeError at runtime, not a silent bug. IDE autocompletes the enum cases perfectly. |
| 2 | **Immutable Value Object Creation**<br>Create a `Money` or `EmailAddress` class using PHP 8.2 `readonly class` (or `readonly` properties). Attempt to modify a property after instantiation and verify it throws an Error. | Class cannot be mutated after creation. All transformations (e.g., `add()`) return a *new* instance of the object. |
| 3 | **Switch to Match Refactor**<br>Find a complex `switch` statement in your codebase. Refactor it to a `match` expression. Ensure it returns a value directly and uses strict comparison. | Code is shorter, more readable, and throws an `UnhandledMatchError` if an unexpected value is passed (no silent fall-through). |
| 4 | **Custom Attribute Implementation**<br>Create a custom PHP Attribute (e.g., `#[Validate('required|string')]` or `#[Route('/api/users')]`). Write a script using the `ReflectionClass` or `ReflectionMethod` API to read and parse this attribute at runtime. | Script successfully extracts the attribute data from the class/method without relying on regex parsing of DocBlocks. |
| 5 | **Intersection Type Enforcement**<br>Create a function that requires an argument to be both `JsonSerializable` and `Stringable`. Create a class that implements both, and one that implements only one. Verify the type hint strictly enforces the intersection. | Passing an object missing either interface results in a fatal TypeError. |
| 6 | **Eliminate a Trait via Composition**<br>Find a Trait in your codebase (e.g., `HasTimestamps` or `Loggable`). Refactor it into a dedicated Service class or a Composition pattern. Inject the dependency via the constructor instead of `use TraitName`. | Class no longer uses `use TraitName`. Behavior is identical, but dependencies are now explicit and easily mockable in tests. |
| 7 | **Constructor Promotion Audit**<br>Refactor a service class with verbose property declarations and constructor assignments to use Constructor Property Promotion. Add PHP 8.1 Attributes to the promoted properties. | Code is significantly shorter. Attributes are correctly applied to the promoted properties (verified via Reflection). |
| 8 | **Named Arguments Refactoring**<br>Find a function call with 3+ arguments (e.g., `createUser($name, $email, $role, $active)`). Refactor the call site to use named arguments. Observe how it improves readability. | Call site is self-documenting. Reordering the arguments in the call does not break the code. |
| 9 | **WeakMap Caching Implementation**<br>Create a script that caches metadata about objects. Use a standard array first, and observe memory usage. Then, refactor to use `WeakMap` and observe how memory is freed when the original object is unset. | `WeakMap` allows the cached object to be garbage collected, preventing a memory leak in long-running processes. |
| 10| **Strict PHPStan Level 9 Enforcement**<br>Configure `phpstan/phpstan` to `level: 9` (max strictness) in your project. Fix all resulting errors (e.g., missing return types, unhandled nulls, invalid property access). | `vendor/bin/phpstan analyse` passes with 0 errors. Codebase is fully type-safe. |
