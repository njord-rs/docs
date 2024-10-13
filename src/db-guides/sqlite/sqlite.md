# SQlite

## Establish a connection

To establish a connection we first need to call the `sqlite::open()` function and use it with a match statement.

```rust
let db_name = "njord.db";
let db_path = Path::new(&db_name);

match sqlite::open(db_path) {
    Ok(c) => {
        println!("Database opened successfully!");
        
        // Additional logic when we are connected.
        // We need to open a connection and pass it 
        // to the corresponding sqlite function.
    }
    Err(err) => eprintln!("Error opening the database: {}", err),
}
```

## Insert data

```rust
let user = User {
    id: AutoIncrementPrimaryKey::default(),
    username: String::from("john_doe"),
    email: String::from("john@example.com"),
    address: String::from("123 Main St"),
};

let result = sqlite::insert(c, vec![user]);
assert!(result.is_ok());
```

**Generated SQL**

```sql
INSERT INTO users (
    username,
    email,
    address
) VALUES (
    'john_doe',
    'john@example.com',
    '123 Main St'
)
```

## Update data

```rust
let columns = vec!["username".to_string(), "address".to_string()];
let where_condition = Condition::Eq("username".to_string(), "john_doe".to_string());

let user = User {
    id: AutoIncrementPrimaryKey::<usize>::new(Some(0)),
    username: String::from("john_doe_2"),
    email: String::from("john@example.com"),
    address: String::from("1234 Main St"),
};

let mut order = HashMap::new();
order.insert(vec!["id".to_string()], "DESC".to_string());
        
let result = sqlite::update(c, user)
    .set(columns)
    .where_clause(where_condition)
    .order_by(order)
    .limit(4)
    .offset(0)
    .build();

assert!(result.is_ok());
```

**Generated SQL**

```sql
UPDATE users
SET
    username
WHERE
    username = 'john_doe'
ORDER BY
    id DESC
LIMIT 4
OFFSET 0

```

## Delete data

```rust
let where_condition = Condition::Eq("username".to_string(), "john_doe".to_string());

let mut order = HashMap::new();
order.insert(vec!["id".to_string()], "DESC".to_string());

let result = sqlite::delete(c)
    .from(User::default())
    .where_clause(where_condition)
    .order_by(order)
    .limit(20)
    .offset(0)
    .build();

assert!(result.is_ok());
```

**Generated SQL**

```sql
DELETE FROM users
WHERE
    username = 'john_doe'
ORDER BY
    id DESC
LIMIT 20
OFFSET 0
```

## Select data

```rust
let columns = vec!["id".to_string(), "username".to_string(), "email".to_string(), "address".to_string()];
let where_condition = Condition::Eq("username".to_string(), "john_doe".to_string());
let group_by = vec!["username".to_string(), "address".to_string()];

let mut order_by = HashMap::new();
order_by.insert(vec!["id".to_string()], "ASC".to_string());

let having_condition = Condition::Gt("id".to_string(), "1".to_string());

// Build the query
// We need to pass the struct User with the Default trait in .from()
let result: Result<Vec<User>> = sqlite::select(c, columns)
    .from(User::default())
    .where_clause(where_condition)
    .order_by(order_by)
    .group_by(group_by)
    .having(having_condition)
    .build();

match result {
    Ok(result) => {
        assert_eq!(result.len(), 1);
    }
    Err(error) => panic!("Failed to SELECT: {:?}", error),
};
```

**Generated SQL**

```sql
SELECT
    id,
    username,
    email,
    address
FROM
    users
WHERE
    username = 'mjovanc'
GROUP BY
    username,
    email
HAVING
    id > 1
ORDER BY
    email DESC
```
