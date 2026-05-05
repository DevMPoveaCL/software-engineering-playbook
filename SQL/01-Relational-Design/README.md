# 🗄️ 01 — Relational Design

> *"A well-designed database doesn't just store data — it tells a story."*

---

## 📌 What Is Relational Design?

A relational database is like a **filing cabinet where every drawer knows about every other drawer**. Not through magic, but through carefully designed **relationships** between tables.

The difference between a good database and a chaotic spreadsheet is:
- **Normalization:** Data lives in one place (no duplication)
- **Keys:** Tables talk to each other through identifiers
- **Constraints:** The database enforces rules automatically

---

## 🏗️ Anatomy of a Relational Table

```
┌─────────────────────────────────────────────────────────────┐
│  CUSTOMERS                                                   │
├──────────────────┬──────────────────┬─────────────────────┤
│  id (PK) 🔑      │  name             │  email              │
├──────────────────┼──────────────────┼─────────────────────┤
│  1               │  Jane Doe         │  jane@example.com   │
│  2               │  John Smith       │  john@example.com   │
│  3               │  Alice Wonder     │  alice@example.com   │
└──────────────────┴──────────────────┴─────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  ORDERS                                                      │
├──────────────────┬──────────────────┬─────────────────────┤
│  id (PK) 🔑      │  customer_id(FK) │  total              │
├──────────────────┼──────────────────┼─────────────────────┤
│  101             │  1                │  59.99              │
│  102             │  2                │  120.00             │
│  103             │  1                │  34.50              │
└──────────────────┴──────────────────┴─────────────────────┘
```

**The relationship:** `orders.customer_id → customers.id`

One customer has many orders. Each order knows WHO made it.

---

## 🔑 Keys: The connectors

| Key Type | What It Does | Example |
|----------|-------------|---------|
| **Primary Key (PK)** | Uniquely identifies each row. One per table. Never NULL. Never repeats. | `customer.id = 1, 2, 3...` |
| **Foreign Key (FK)** | Points to a Primary Key in another table. Creates the relationship. | `orders.customer_id = 1` |
| **Unique Key** | Ensures a column value never duplicates within the same table | `users.email` must be unique |
| **Composite Key** | Two or more columns together form a unique key | Order line: (order_id, product_id) |

---

## 📋 Core Constraints

| Constraint | What It Does | Example |
|------------|-------------|---------|
| `NOT NULL` | Column must have a value | `name VARCHAR NOT NULL` |
| `UNIQUE` | No duplicate values in this column | `email VARCHAR UNIQUE` |
| `CHECK` | Validates a condition | `age INT CHECK (age >= 18)` |
| `DEFAULT` | Value if none provided | `created_at DEFAULT NOW()` |
| `FOREIGN KEY` | References another table's PK | `customer_id REFERENCES customers(id)` |

---

## ✅ What TO Do

### DO: Use IDs as Primary Keys
```sql
-- Always. Never use email, name, or "natural" keys.
CREATE TABLE customers (
    id INT PRIMARY KEY,  -- 1, 2, 3... no business meaning
    name VARCHAR NOT NULL,
    email VARCHAR UNIQUE
);
```
**Why?** If Jane changes her email, you don't want to update a dozen tables. The ID is meaningless to the business — that's the point.

### DO: Normalize (DRY for Data)
```sql
-- Bad: Customer name repeated in every order row
CREATE TABLE bad_orders (
    id INT PRIMARY KEY,
    customer_name VARCHAR,  -- ❌ Duplicated data!
    total DECIMAL
);

-- Good: Name lives in ONE place
CREATE TABLE customers (id INT PRIMARY KEY, name VARCHAR);
CREATE TABLE orders (id INT PRIMARY KEY, customer_id INT REFERENCES customers(id), total DECIMAL);
```

### DO: Plan Relationships Before Coding
```
Customer 1───∞ Orders
     ∞
     │
     └──∞ OrderItems ∞───1 Product
```

---

## 🚫 What NOT to Do

### DON'T: Store Aggregates in the Parent Table
```sql
-- Bad: Storing calculated totals in orders table
CREATE TABLE bad_orders (
    id INT PRIMARY KEY,
    item_count INT,      -- Just derive this with COUNT()
    subtotal DECIMAL,    -- Just use SUM()
    total DECIMAL        -- This is derivable!
);
```

### DON'T: Use Float for Money
```sql
-- Bad: Float introduces rounding errors (0.1 + 0.2 ≠ 0.3)
CREATE TABLE bad_items (price FLOAT);

-- Good: Use DECIMAL for exact decimal arithmetic
CREATE TABLE items (price DECIMAL(10,2));
```

### DON'T: Create Tables Without Considering Reuse
Ask: *"Will another part of the system need this data?"* If yes, it belongs in its own table.

---

## 🎯 Why This Matters

### In the Workplace: Data Integrity
A database without constraints is like a company where anyone can alter anyone else's records. Foreign keys prevent **orphaned records** (an order referencing a customer that was deleted). CHECK constraints prevent invalid data (negative prices, future birthdays).

### In the Workplace: Change Flexibility
When requirements change (and they will), a well-normalized database adapts. Adding a new column to the Customer table updates all related data through relationships, not through hunting down duplicated values across 15 tables.

---

## 🧠 Mental Model: The Filing Cabinet

| Filing Cabinet | Relational Database |
|----------------|---------------------|
| Each drawer is a table | Tables store related data |
| Folders have ID tabs | Primary Keys identify rows |
| Cross-reference labels on folders | Foreign Keys link tables |
| Rules: "No duplicate IDs" | UNIQUE, CHECK constraints |
| If you delete a customer, remove their folder first | Cascading deletes |

---

## 📚 Technical Glossary

- **Normalization:** The process of organizing data to eliminate redundancy and ensure dependencies make sense.
- **FK (Foreign Key):** A column (or set of columns) that references the Primary Key of another table.
- **Constraint:** A rule enforced by the database (NOT NULL, UNIQUE, CHECK, FOREIGN KEY).
- **Cardinality:** The numeric relationship between tables (1:1, 1:N, N:N).
- **Orphaned Record:** A row that references another row that no longer exists.

---

[⬅️ Back to Parent](../README.md)
