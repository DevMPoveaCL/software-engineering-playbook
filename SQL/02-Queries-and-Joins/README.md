# 🔗 02 — Queries and Joins

> *"JOINs are where data comes alive — and where developers go to suffer."*

---

## 📌 The Two Worlds of SQL

SQL operates in two distinct modes:

1. **DQL (Data Query Language):** `SELECT` — asking questions
2. **DML (Data Manipulation Language):** `INSERT`, `UPDATE`, `DELETE` — changing data

Most of the power — and complexity — lives in `SELECT` and `JOIN`.

---

## 🔀 JOIN Types: The Venn Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│  INNER JOIN: The Overlap                                        │
│  ┌──────────────────┐      ┌──────────────────┐                │
│  │    Table A       │      │    Table B       │                │
│  │  ┌──────────┐    │      │  ┌──────────┐    │                │
│  │  │ INNER   │    │      │  │ INNER   │    │                │
│  │  │ (both)  │    │      │  │ (both)  │    │                │
│  │  └──────────┘    │      │  └──────────┘    │                │
│  └──────────────────┘      └──────────────────┘                │
│  Result: Only rows that exist in BOTH tables                    │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  LEFT JOIN: All of A + matching B                               │
│  ┌──────────────────┐      ┌──────────────────┐                │
│  │    Table A       │      │    Table B       │                │
│  │  ┌──────────┐────│──────│────┬──────────┐ │                │
│  │  │ ALL A    │    │      │    │ match    │ │                │
│  │  └──────────┘────│──────│────┴──────────┘ │                │
│  └──────────────────┘      └──────────────────┘                │
│  Result: Every A row, with B data if available (NULL if not)  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  RIGHT JOIN: All of B + matching A                              │
│  ┌──────────────────┐      ┌──────────────────┐                │
│  │    Table A       │      │    Table B       │                │
│  │  ┌──────────┐────│──────│────┬──────────┐ │                │
│  │  │  match   │    │      │    │  ALL B   │ │                │
│  │  └──────────┘────│──────│────┴──────────┘ │                │
│  └──────────────────┘      └──────────────────┘                │
│  Result: Every B row, with A data if available (NULL if not)   │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  FULL JOIN: Everything from both tables                         │
│  ┌──────────────────┐      ┌──────────────────┐                │
│  │    Table A       │      │    Table B       │                │
│  │  ┌──────────┐    │      │  ┌──────────┐    │                │
│  │  │ ALL A    │────│──────│────│ ALL B   │ │                │
│  │  └──────────┘    │      │  └──────────┘    │                │
│  └──────────────────┘      └──────────────────┘                │
│  Result: ALL A + ALL B, matched where possible (NULL otherwise)│
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  CROSS JOIN: Every possible combination                         │
│  ┌──────────────────┐      ┌──────────────────┐                │
│  │    Table A       │      │    Table B       │                │
│  │  Row 1           │──────│────Row 1         │                │
│  │  Row 2           │──────│────Row 2         │                │
│  │  Row 3           │──────│────Row 3         │                │
│  └──────────────────┘      └──────────────────┘                │
│  Result: A×B rows (3×3 = 9). Dangerous if tables are large!    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 JOIN Syntax Reference

```sql
-- INNER JOIN: Only matching rows
SELECT c.name, o.total
FROM customers c
INNER JOIN orders o ON c.id = o.customer_id;

-- LEFT JOIN: All customers, with orders if they exist
SELECT c.name, o.total
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id;

-- RIGHT JOIN: All orders, with customer info if available
SELECT c.name, o.total
FROM customers c
RIGHT JOIN orders o ON c.id = o.customer_id;

-- FULL JOIN: Everything
SELECT c.name, o.total
FROM customers c
FULL JOIN orders o ON c.id = o.customer_id;

-- CROSS JOIN: Every combination (use with caution)
SELECT c.name, p.product_name
FROM customers c
CROSS JOIN products p;
```

---

## ✅ What TO Do

### DO: Use Table Aliases
```sql
-- Readable: abbreviations make queries scannable
SELECT c.name, o.total, o.created_at
FROM customers c
INNER JOIN orders o ON c.id = o.customer_id
WHERE o.total > 100;
```

### DO: Filter in WHERE Before JOINing
```sql
-- Efficient: reduce rows BEFORE the join
SELECT c.name, o.total
FROM customers c
INNER JOIN orders o ON c.id = o.customer_id
WHERE o.total > 100 AND c.country = 'USA';
```

### DO: Use ON for Join Conditions
```sql
-- Explicit: ON clause for relationship
SELECT *
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```

---

## 🚫 What NOT to Do

### DON'T: Forget NULL in LEFT/RIGHT JOINs
```sql
-- Bad: Returns customers with NULL orders (orders don't exist)
SELECT c.name, o.total
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
WHERE o.total > 100;  -- Filters OUT NULL orders!

-- Good: Use COALESCE or check for NULL
SELECT c.name, o.total
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
WHERE o.total > 100 OR o.total IS NULL;
```

### DON'T: Use Implicit Join Syntax (Comma + WHERE)
```sql
-- Old, error-prone style (avoids in modern SQL)
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.id;  -- WHERE hides the relationship

-- Modern style (explicit relationship)
SELECT *
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```

### DON'T: Use CROSS JOIN Unless Intended
```sql
-- Dangerous: 1000 customers × 1000 products = 1,000,000 rows
-- Usually a mistake, not intentional
SELECT c.name, p.product_name
FROM customers c
CROSS JOIN products p;
```

---

## 🎯 Why This Matters

### In the Workplace: Reporting
Most business reports are JOINs. "Show all customers and their total orders" requires a LEFT JOIN. If you don't understand JOIN direction, you'll miss customers with zero orders — or double-count those without.

### In the Workplace: Performance
A CROSS JOIN on two million-row tables crashes databases. Always know your cardinality before writing a JOIN.

---

## 🧠 Mental Model: The School Dance

| Scenario | JOIN Type |
|----------|-----------|
| Only students who brought a date attend | INNER JOIN |
| All students attend, those with dates bring them | LEFT JOIN |
| All dates attend, students who came alone are listed as "unknown" | RIGHT JOIN |
| Every possible boy/girl pairing for a mixer | CROSS JOIN |

---

## 📚 Technical Glossary

- **INNER JOIN:** Returns rows where the join condition is true in BOTH tables.
- **LEFT JOIN:** Returns ALL rows from the left table, with NULL for right-side matches.
- **RIGHT JOIN:** Returns ALL rows from the right table, with NULL for left-side matches.
- **FULL JOIN:** Returns ALL rows from both tables, NULL where no match exists.
- **CROSS JOIN:** Cartesian product — every row from A paired with every row from B.
- **ON vs USING:** `ON` specifies the condition; `USING` assumes same-named column exists.

---

[⬅️ Back to Parent](../README.md)
