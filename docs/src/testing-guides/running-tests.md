## Running Tests

Now that we understand the structure, let's look at how to run tests.  
Rust's test runner is built into the `cargo` command and provides several options:

1. Run all tests in the project:
   ```bash
   cargo test
   ```

2. Run tests in a specific module (folder):
   ```bash
   cargo test --test folder_1_tests
   ```
   This command runs all tests in the `folder_1` module, as defined by its `mod.rs` file.

3. Run a specific test function:
   ```bash
   cargo test --test folder_1_tests test_function_name
   ```
   This runs a specific test function within the `folder_1` module.