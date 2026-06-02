# Rust Programming Language: Set 1
## The Rust Paradigm Shift: Ownership, Borrowing & Lifetimes

## 📘 10 Conceptual Questions
1. **Ownership & Move Semantics:** In PHP, `$a = $b` copies the value (or uses Copy-on-Write). In Rust, it *moves* ownership. What does "moving" mean, and why does the original variable become invalid? How does this eliminate double-free errors without a Garbage Collector?
 a Reference (`&`) vs. Mutable Reference (`&mut`)?** What are the strict rules of borrowing (e.g., "either one mutable reference OR any number of immutable references, but not both")? How does this prevent data races at compile time?
3. **Lifetimes (`'a`):** What is a lifetime in Rust, and why is it *not* a garbage collection mechanism? Explain how lifetimes are simply a way for the Borrow Checker to ensure that a reference never outlives the data it points to (preventing dangling pointers).
4. **Stack vs. Heap:** How does Rust decide whether data goes on the Stack or the Heap? Why are types like `i32` or `bool` stored on the Stack, while `String` or `Vec` require Heap allocation, and how does this impact performance?
5. **`String` vs. `&str`:** What is the fundamental difference between an owned `String` and a borrowed string slice `&str`? When should you use each in function signatures to write the most flexible and efficient code?
6. **Pattern Matching (`match`) vs. `switch`:** How does Rust's `match` expression differ from PHP's `switch`? Explain the concepts of "exhaustiveness checking" and "destructuring," and why `match` must cover all possible cases or include a `_` wildcard.
7. **Shadowing vs. Mutability (`let mut`):** What is variable shadowing (e.g., `let x = 5; let x = x + 1;`)? How does it differ from making a variable mutable, and why is it useful for transforming data types (e.g., parsing a string to an integer and keeping the same variable name)?
8. **Structs and `impl` Blocks:** Rust does not have classes or inheritance. How do you model object-oriented concepts using `struct` and `impl` blocks? What is the difference between an associated function (like `::new()`) and a method (which takes `&self`)?
9. **The `Clone` Trait:** If you *want* to duplicate Heap-allocated data (like a `String`), how do you do it explicitly? Why does Rust force you to call `.clone()` instead of doing it implicitly, and what is the performance implication?

10. **The Borrow Checker as a Mentor:** Beginners often fight the Borrow Checker. How should you reframe your mindset? Instead of viewing compiler errors as annoyances, how do they actually guide you toward safer, more predictable architecture?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Fix the Borrow Checker**<br>Write a function that takes a `String`, tries to calculate its length, and then tries to print the original `String`. Intentionally trigger a "use of moved value" error. Fix it by passing a reference (`&String` or `&str`) instead of taking ownership. | Code compiles cleanly. You understand that passing by reference allows the caller to retain ownership. |
| 2 | **Mutable Borrowing Rules**<br>Write a function that modifies a `Vec<i32>` by pushing a new element. Call it, then try to read the `Vec` while a mutable reference to it is still active. Fix the code by ensuring the mutable borrow scope ends before the immutable read. | Code compiles. You demonstrate understanding of non-overlapping mutable and immutable borrows. |
| 3 | **Explicit Cloning**<br>Create a `User` struct with a `String` name. Write a function that takes ownership of a `User` and returns it. In `main`, call this function twice with the same `User` instance. Fix the second call by using `.clone()`. | Code compiles. You observe the explicit memory allocation happening via `.clone()`. |
| 4 | **String vs. &str Mastery**<br>Write a function `fn get_first_word(s: &str) -> &str` that returns a slice of the string up to the first space. Call it with both a `String` variable and a string literal `"hello world"`. | Function accepts both owned and borrowed strings seamlessly. Returns a valid `&str` slice. |
| 5 | **Exhaustive Pattern Matching**<br>Create an `enum` called `NetworkState` with variants: `Connected`, `Disconnected`, `Error(String)`. Write a `match` expression that prints a specific message for each, including extracting the error string from the `Error` variant. | Code compiles without warnings. The `Error` variant successfully destructures and binds the inner `String` to a variable. |
| 6 | **Variable Shadowing**<br>Write a script that takes a string `"  42  "`, uses `.trim()` to remove whitespace, parses it to an `i32`, and multiplies it by 2. Use *shadowing* (`let spaces = ...; let spaces = ...;`) to keep the variable name the same while changing its type. | Code compiles. The final variable is an `i32` with the value `84`. No `mut` keyword is used. |
| 7 | **Structs and Methods**<br>Define a `Rectangle` struct with `width` and `height` (both `u32`). Implement an `impl` block with an associated function `square(size: u32) -> Rectangle` and a method `area(&self) -> u32`. | You can create a rectangle via `Rectangle::square(5)` and call `rect.area()` successfully. |
| 8 | **Lifetime Annotation**<br>Write a function `fn longest<'a>(x: &'a str, y: &'a str) -> &'a str` that returns the longer of two string slices. Call it in `main` and print the result. | Code compiles. You understand that the `'a` annotation tells the compiler the returned reference is valid as long as *both* input references are valid. |
| 9 | **Ownership in Loops**<br>Create a `Vec<String>`. Write a `for` loop that consumes the vector (`for s in vec`). Notice you can't use `vec` afterward. Refactor the loop to borrow the elements (`for s in &vec`) so the vector remains usable after the loop. | Vector is successfully iterated over, and you can still print or modify the vector after the loop completes. |
| 10| **Cargo Workspace Basics**<br>Initialize a new Cargo workspace (`cargo new my_rust_project --lib` and `cargo new my_cli`). Configure the root `Cargo.toml` to include both. Make the CLI depend on the library and call a function from it. | `cargo run` in the CLI directory successfully executes code defined in the library crate. |
