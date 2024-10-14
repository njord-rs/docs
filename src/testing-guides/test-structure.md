# Test Structure

Njord uses Rust's built-in testing framework.  
Tests are organized in a modular structure within the `tests` directory:

```
njord/
└── tests/
    ├── folder_1/
    │   ├── mod.rs
    │   ├── test_1.rs
    │   └── test_2.rs
    │
    └── folder_2/
        ├── mod.rs
        ├── test_1.rs
        └── test_2.rs
```

## mod.rs

The `mod.rs` file is crucial in Rust's module system. In the context of testing:

1. It acts as an entry point for a test module.
2. It declares submodules (other test files) in the folder.
3. It can contain shared utilities, setup code, or common test functions.

A typical `mod.rs` file might look like this:

```rust
// Declare submodules
mod test_1;
mod test_2;

// Shared test setup
use crate::YourStruct;

#[cfg(test)]
mod tests {
    use super::*;

    // A helper function for tests in this module
    fn setup() -> YourStruct {
        // Setup code here
    }

    // A test that can be used by submodules
    #[test]
    fn common_test() {
        let instance = setup();
        // Test assertions
    }
}
```

This structure allows you to organize related tests into a single module, making them more maintainable as your project grows.
