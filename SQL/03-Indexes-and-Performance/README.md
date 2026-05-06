# ⚡ 03 — Indexes and Performance

> *"A database without indexes is like a library without a catalog."*

---

## 📌 Why Performance Matters

Imagine a **library with one million books** — and no index, no catalog, no alphabetical order. To find every book written by "Alice Wonder," a librarian would walk shelf by shelf, checking every single book.

That's what a database does **without indexes**: Full Table Scans. Slow. Expensive.

An index is the **alphabetical shortcut** at the back of an encyclopedia — the database builds it once, and queries become orders of magnitude faster.

---

## 🏗️ How Indexes Work

```
┌─────────────────────────────────────────────────────────────────┐
│  WITHOUT INDEX (Full Table Scan):                               │
│  Finding name='Alice' in 1,000,000 rows:                        │
│  Row 1: 'Bob' → No                                              │
│  Row 2: 'Charlie' → No                                          │
│  Row 3: 'Alice' → Yes! ✓ (but we checked 3 million times first)│
│  ... continues until end                                        │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  WITH INDEX on 'name' column:                                  │
│  The database builds a sorted lookup tree (B-Tree):            │
│                                                                 │
│                    [D]                                          │
│              ┌─────┴─────┐                                      │
│          [B]             [M]                                    │
│        ┌───┴───┐     ┌───┴───┐                                  │
│     [A]       [C]  [J]       [Z]                               │
│                                                                 │
│  Finding 'Alice': [A] → immediate. O(log n) not O(n).          │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Index Types

| Index Type | When to Use | Example |
|------------|-------------|---------|
| **B-Tree (default)** | Equality (`=`) and range (`>`, `<`) queries on sorted data | `CREATE INDEX idx_name ON users(name)` |
| **Hash Index** | Fast equality lookups only (`=`) | Memory-engine tables |
| **Composite Index** | Multi-column queries (most selective first) | `INDEX (country, created_at)` |
| **Unique Index** | Enforce uniqueness + speed up lookups | `UNIQUE INDEX idx_email ON users(email)` |
| **Full-Text Index** | Text search ("contains" queries) | `MATCH(content) AGAINST('search term')` |

---

## ✅ What TO Do

### DO: Index Foreign Keys
```sql
-- Almost always add an index on FK columns
CREATE INDEX idx_orders_customer ON orders(customer_id);

-- Why? JOINs filter by this column constantly
SELECT *
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
WHERE c.country = 'USA';
```

### DO: Index Columns in WHERE Clauses
```sql
-- If you frequently filter by 'status'
CREATE INDEX idx_orders_status ON orders(status);

-- Now this query is fast:
SELECT * FROM orders WHERE status = 'pending';
```

### DO: Use Composite Indexes for Multi-Column Queries
```sql
-- If queries often filter by (country, city)
CREATE INDEX idx_customers_location ON customers(country, city);

-- This benefits from the composite index:
SELECT * FROM customers WHERE country = 'USA' AND city = 'NYC';
```

---

## 🚫 What NOT to Do

### DON'T: Index Everything
```sql
-- Bad: Every column indexed — slows down INSERT/UPDATE/DELETE
-- Each write must update all indexes (disk I/O overhead)
CREATE INDEX idx_users_name ON users(name);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created ON users(created_at);
CREATE INDEX idx_users_phone ON users(phone);  -- rarely queried!
```

### DON'T: Index Low-Cardinality Columns
```sql
-- Bad: Boolean or low-variance columns don't benefit from B-Tree
-- 'is_active' has only 2 values: 0 or 1
-- The index would be almost as large as the table itself
CREATE INDEX idx_users_active ON users(is_active);  -- useless
```

### DON'T: Use SELECT * in Production
```sql
-- Bad: Pulls all columns, uses more memory, prevents index-only scans
SELECT * FROM orders WHERE id = 123;

-- Good: Only select what you need
SELECT id, total, status FROM orders WHERE id = 123;
```

---

## 🎯 Why This Matters

### In the Workplace: Scale
At 10,000 rows, a full table scan takes 10ms. At 10 million rows, it takes 10 seconds — and your API times out. A properly indexed query at 10 million rows? 2ms. That's the difference between a product that scales and one that collapses under load.

### In the Workplace: Cost
Cloud databases charge per query execution time. Slow queries = higher bills. A $50/month database handling 100 queries/second is cheaper than a $500/month database handling 10 queries/second because of unindexed tables.

---

## 🧠 Mental Model: The Catalog vs. The Shelf

| Library Without Catalog | Library With Catalog |
|------------------------|---------------------|
| Must check every book | Jump directly to shelf |
| 10 million books = hours | 10 million books = seconds |
| Librarian hates you | Librarian loves you |
| **Database without index** | **Database with index** |

The catalog (index) is smaller than the full shelf, but it tells you exactly where everything is.

---

## 📚 Technical Glossary

- **B-Tree (Balanced Tree):** The default index structure. Keeps data sorted and enables O(log n) lookups.
- **Full Table Scan:** Reading every row to find matches. The villain of SQL performance.
- **Composite Index:** An index on multiple columns. Benefits queries using the leftmost prefix.
- **Cardinality:** The number of distinct values in a column. High cardinality = good candidate for indexing.
- **Index Selectivity:** How selective the index is. `UNIQUE` is the most selective (1:1).
- **Covering Index:** A composite index that includes all columns needed by a query — the query never touches the table.

---

[⬅️ Previous: 02 Queries and Joins](../02-Queries-and-Joins/README.md) | [⬅️ Back to Parent](../README.md)
