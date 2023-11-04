# Tests in Rust


You might have heard the saying "if it compiles, it works" in the Rust community. But, does it _really_ work?

Does your code **behave as intended**?

Have you considered **all edge cases**?

Sure, Rust has a robust type system, and a borrow checker that will prevent you from shooting yourself in the foot, but that's no substitute for **good old-fashioned** **testing**.

Luckily for us, the Rust community has developed many useful crates that make testing Rust code easier!

Today I would like to share some of them with you:

[**assert\_cmd**](https://letsgetrusty.krtra.com/c/Fh4RlEodjOoQ/JPaK)

Easily write integration tests for your CLI applications.

**[serial\_test](https://letsgetrusty.krtra.com/c/kNCWvch2PHrK/JPaK)**

Create serialised Rust tests.

[**mockall**](https://letsgetrusty.krtra.com/c/UPiKe7R6AVYf/JPaK)

A powerful mocking library for Rust.

[**arbritrary**](https://letsgetrusty.krtra.com/c/b1Jm7vrd58Za/JPaK)

Construct a type with arbitrary values.

**[proptest](https://letsgetrusty.krtra.com/c/oU7mh2wG9ulF/JPaK)**

Hypothesis-like property-based testing and shrinking.

[**insta**](https://letsgetrusty.krtra.com/c/AlLOYmrcIpdT/JPaK)

A snapshot testing library for Rust.

[**httpmock**](https://letsgetrusty.krtra.com/c/dMxZjLAt71Cp/JPaK) and **[wiremock](https://letsgetrusty.krtra.com/c/9H8JKs4OqBXc/JPaK)**

HTTP mocking libraries for Rust.

[**proc-macro2**](https://letsgetrusty.krtra.com/c/7h3Wx1tpVma4/JPaK)

A wrapper around the procedural macro API of the compiler's _proc\_macro_ crate. Allows procedural macros to be unit tested amongst other things.

**[cargo-nextest](https://letsgetrusty.krtra.com/c/VmQnL8fXul9c/JPaK)** **\*BONUS\***

A next-generation test runner for Rust which offers beautiful test results, flacky test detection, and can be **60% faster than cargo test**.

**[cargo-tarpaulin](https://letsgetrusty.krtra.com/c/BZGsOIz7q4oQ/JPaK) \*BONUS 2\***

A code coverage reporting tool for the Cargo build system.

