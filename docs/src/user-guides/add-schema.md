# Add a schema file

Now we are going to define our schema file that we will create under `src/schema.rs`.  
We will store basically our structs that will map against the database. 

```rust
#[derive(Table)]
#[table_name = "users"]
pub struct User {
    id: AutoIncrementPrimaryKey<usize>,
    username: String,
    email: String,
    address: String,
}

#[derive(Table)]
#[table_name = "categories"]
pub struct Category {
    id: AutoIncrementPrimaryKey<usize>,
    name: String,
}

#[derive(Table)]
#[table_name = "products"]
pub struct Product {
    id: AutoIncrementPrimaryKey<usize>,
    name: String,
    description: String,
    price: f64,
    stock_quantity: usize,
    category: Category,     // one-to-one relationship
    discount: Option<f64>,  // allow for null 
}

#[derive(Table)]
#[table_name = "orders"]
pub struct Order {
    id: AutoIncrementPrimaryKey<usize>,
    user: User,             // one-to-one relationship
    products: Vec<Product>, // one-to-many relationship - populates from based on junction table (gets from macro attribute "table_name" and combines them for example, orders_products)
    total_cost: f64,
}
```

Now that we have that in place, we need to create the SQL for setting this up in the database and execute it.

```sql
-- users table
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    email TEXT NOT NULL,
    address TEXT NOT NULL
);

-- products table
CREATE TABLE products (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    description TEXT NOT NULL,
    price REAL NOT NULL,
    stock_quantity INTEGER NOT NULL,
    category INTEGER REFERENCES categories(id),
    discount: REAL NULL
);

-- orders table
CREATE TABLE orders (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER REFERENCES users(id),
    total_cost REAL NOT NULL
);

-- order_products table
CREATE TABLE order_products (
    order_id INTEGER REFERENCES orders(id),
    product_id INTEGER REFERENCES products(id),
    PRIMARY KEY (order_id, product_id)
);
```
