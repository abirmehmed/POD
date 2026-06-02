# Rust Programming Language: Set 4 (Final)
## Ecosystem, FFI, WebAssembly & Production Rust

## 📘 10 Conceptual Questions
1. **Cargo Workspaces:** How do workspaces solve dependency duplication and streamline multi-crate projects compared to a single monolithic crate? Why is this the standard pattern for building a library and its accompanying CLI tool?
2. **Macros (Declarative vs. Procedural):** What is the difference between `macro_rules!` (declarative) and Procedural Macros? Why are procedural macros (like `#[derive(Serialize)]`) incredibly powerful for code generation, but also more complex to compile and debug?
3. **FFI and `unsafe` Rust:** When interacting with C libraries (or PHP via FFI), why must we use the `unsafe` keyword? What guarantees does the Rust compiler *relinquish* in an `unsafe` block (e.g., no borrow checking), and what responsibilities does the developer take on?
4. **WebAssembly (Wasm) Compilation:** How does Rust's lack of a garbage collector and predictable memory model make it an ideal language for compiling to WebAssembly? What is the role of `wasm-bindgen` in bridging Rust and JavaScript?
5. **Production Error Handling (`anyhow` vs. `thiserror`):** Why is it idiomatic to use `thiserror` for library code (defining custom, structured error enums) and `anyhow` for application binary code (easy error propagation with `.context()`)?
6. **The Testing Pyramid in Rust:** How do unit tests (in the same file), integration tests (in the `tests/` directory), and documentation tests (`/// ``` `) work together? Why are doc tests uniquely powerful for keeping examples in sync with code?
7. **Benchmarking with `criterion`:** Why is the standard `#[bench]` attribute unstable in Rust? How does the `criterion` crate provide statistically significant benchmarking, including warm-up periods, outlier detection, and regression tracking?
8. **Linting and Code Quality (Clippy):** How does `cargo clippy` go beyond basic compilation errors to catch common Rust anti-patterns (e.g., unnecessary `.clone()`, inefficient loops, or suboptimal iterator usage)?
9. **Calling Rust from PHP (FFI):** How can you compile a Rust library into a C-compatible dynamic library (`.so`/`.dylib`/`.dll`) using `#[no_mangle]` and `extern "C"`, and safely load it in PHP using the native `FFI` extension?
10. **Publishing and Semantic Versioning:** What checks does `cargo publish` perform before uploading to crates.io? How does Rust's strict adherence to SemVer and the concept of "cargo semver checks" prevent breaking changes in dependent projects?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Setup a Cargo Workspace**<br>Initialize a workspace with a `lib` crate (`multimart-core`) and a `bin` crate (`multimart-cli`). Have the CLI depend on the core library. Run `cargo build --workspace`. | `Cargo.toml` at the root defines `[workspace]`. Both crates compile together, sharing a single `target/` directory and `Cargo.lock`. |
| 2 | **Write a Declarative Macro**<br>Create a `macro_rules!` named `hashmap!` that allows you to instantiate a `HashMap` elegantly: `hashmap!{ "key" => "value", "key2" => "value2" }`. | The macro expands correctly at compile time. It handles zero, one, or multiple key-value pairs without syntax errors. |
| 3 | **Production Error Handling**<br>Define a custom error enum using `thiserror` in your library. In your CLI `main` function, use `anyhow::Result` and call the library function, appending `.context("Failed to process")` to the error. | Errors are cleanly formatted with context. The library exports a structured error type, while the app handles it gracefully without panicking. |
| 4 | **FFI: Exposing Rust to C**<br>Write a Rust function `pub extern "C" fn calculate_discount(price: f64, percent: f64) -> f64`. Add `#[no_mangle]`. Configure `Cargo.toml` with `crate-type = ["cdylib"]` and build it. | Running `cargo build` produces a `.so` (Linux), `.dylib` (macOS), or `.dll` (Windows) file in the `target/debug` directory. |
| 5 | **FFI: Calling Rust from PHP**<br>Write a PHP script using the `FFI` extension. Load the compiled Rust library from Task 4. Define the C signature in PHP and call `calculate_discount(100.0, 20.0)`. | PHP script successfully loads the Rust library, executes the function, and outputs `80`. No segfaults or memory leaks. |
| 6 | **Integration Testing**<br>Create a `tests/integration_test.rs` file in your workspace. Import your `multimart-core` library and write a test that exercises a public API end-to-end, as an external user would. | `cargo test` runs the integration test successfully. The test cannot access `pub(crate)` or private items, proving true black-box testing. |
| 7 | **Documentation Tests**<br>Write a doc comment (`///`) for a public function in your library. Include a code block (` ``` `) demonstrating how to use it. Run `cargo test --doc`. | The documentation test executes the code block and passes. Running `cargo doc --open` generates beautiful, interactive HTML docs. |
| 8 | **CLI App with `clap`**<br>Build a CLI tool using `clap` (with the `derive` feature). It should accept a required `--name` argument and an optional `--verbose` flag, then print a formatted message. | Running `cargo run -- --name "Alice" --verbose` works perfectly. Running `cargo run -- --help` generates a professional, auto-formatted help menu. |
| 9 | **Benchmarking with Criterion**<br>Set up a `criterion` benchmark comparing naive string concatenation (`s = s + "a"`) in a loop vs. using `s.push_str("a")` or `.collect::<String>()`. | `cargo bench` runs, performs statistical analysis, and generates an HTML report showing the performance difference between the two approaches. |
| 10| **Clippy & Rustfmt Enforcement**<br>Run `cargo clippy -- -D warnings` and `cargo fmt --check` on your entire workspace. Fix every single suggestion until the codebase is 100% clean and idiomatic. | Both commands exit with code 0. Your code follows official Rust style guidelines and avoids all known anti-patterns. |
