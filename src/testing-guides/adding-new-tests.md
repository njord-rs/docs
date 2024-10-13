# Adding New Tests

To add new tests to Njord:

1. Create a new folder in the `tests` directory if it doesn't exist.
2. Add a `mod.rs` file to declare submodules and shared utilities.
3. Create test files (e.g., `test_1.rs`, `test_2.rs`) within the folder.

For example, to add tests for a new feature called "user_management":

```
njord/
└── tests/
    └── user_management/
        ├── mod.rs
        ├── create_user.rs
        └── delete_user.rs
```

In the `mod.rs` file, declare the test files as modules:

```rust
mod create_user;
mod delete_user;
```

Write your test functions in `create_user.rs` and `delete_user.rs` using the `#[test]` attribute.

This structure allows you to:
- Group related tests together
- Share common setup and utilities across tests in the same module
- Easily run all tests for a specific feature

Remember to use descriptive names for your test functions and keep tests isolated and independent of each other.

---

## Best Practices

1. Group related tests in the same module (folder).
2. Use descriptive names for test functions.
3. Keep tests isolated and independent of each other.
4. Use setup and teardown functions in `mod.rs` for common operations.
5. Run tests frequently during development to catch issues early.

By following this structure and these practices, you can maintain a clean and efficient testing suite for Njord.
