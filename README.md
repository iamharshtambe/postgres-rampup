# PostgreSQL Quick Reference Guide 🐘

A comprehensive cheat sheet for PostgreSQL commands and queries.

## Table of Contents

- [Database Operations](#database-operations)
- [Table Operations](#table-operations)
- [Data Insertion](#data-insertion)
- [Data Selection & Querying](#data-selection--querying)
- [Filtering & Conditions](#filtering--conditions)
- [Limiting & Pagination](#limiting--pagination)
- [Pattern Matching](#pattern-matching)
- [Aggregation & Grouping](#aggregation--grouping)
- [Functions & Calculations](#functions--calculations)
- [Date & Time Operations](#date--time-operations)
- [Constraints & Keys](#constraints--keys)
- [Data Modification](#data-modification)
- [Conflict Resolution](#conflict-resolution)
- [Joins](#joins)
- [Extensions & UUIDs](#extensions--uuids)

---

## Database Operations

### Create Database

```sql
CREATE DATABASE database_name;
```

Creates a new database with the specified name.

### Connect to Database

```sql
\c database_name
```

Connects to the specified database (psql command).

### Drop Database

```sql
DROP DATABASE database_name;
```

⚠️ **Warning**: Permanently deletes the database and all its data.

---

## Table Operations

### Create Table

```sql
CREATE TABLE table_name (
    column_name data_type constraints
);
```

Creates a new table with specified columns and constraints.

**Example:**

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE,
    created_at TIMESTAMP DEFAULT NOW()
);
```

### View Table Information

```sql
\d                    -- List all tables, views, and sequences
\d table_name         -- Describe specific table structure
\dt                   -- List tables only
```

### Drop Table

```sql
DROP TABLE table_name;
```

⚠️ **Warning**: Permanently deletes the table and all its data.

---

## Data Insertion

### Basic Insert

```sql
INSERT INTO table_name (
    column_name1,
    column_name2
)
VALUES (
    'value1',
    'value2'
);
```

### Multiple Row Insert

```sql
INSERT INTO table_name (column1, column2)
VALUES
    ('value1a', 'value2a'),
    ('value1b', 'value2b'),
    ('value1c', 'value2c');
```

---

## Data Selection & Querying

### Basic Select

```sql
SELECT * FROM table_name;                    -- Select all columns
SELECT column_name FROM table_name;          -- Select specific column
SELECT column1, column2 FROM table_name;     -- Select multiple columns
```

### Import SQL File

```sql
\i /path/to/file.sql
```

Executes SQL commands from a file.

---

## Filtering & Conditions

### Basic WHERE Clause

```sql
SELECT * FROM table_name WHERE column_name = 'condition';
```

### Multiple Conditions

```sql
-- AND condition (both must be true)
SELECT * FROM table_name WHERE column1 = 'value1' AND column2 = 'value2';

-- OR condition (either can be true)
SELECT * FROM table_name WHERE column1 = 'value1' OR column2 = 'value2';
```

### Comparison Operators

```sql
SELECT * FROM table_name WHERE column_name = 'value';      -- Equal
SELECT * FROM table_name WHERE column_name <> 'value';     -- Not equal
SELECT * FROM table_name WHERE column_name > 100;         -- Greater than
SELECT * FROM table_name WHERE column_name < 100;         -- Less than
SELECT * FROM table_name WHERE column_name >= 100;        -- Greater than or equal
SELECT * FROM table_name WHERE column_name <= 100;        -- Less than or equal
```

### IN Operator

```sql
SELECT * FROM table_name WHERE column_name IN ('value1', 'value2', 'value3');
```

Checks if column value matches any value in the list.

### BETWEEN Operator

```sql
SELECT * FROM table_name WHERE column_name BETWEEN 'start_date' AND 'end_date';
SELECT * FROM table_name WHERE age BETWEEN 18 AND 65;
```

---

## Limiting & Pagination

### LIMIT and OFFSET

```sql
SELECT * FROM table_name LIMIT 10;                    -- First 10 rows
SELECT * FROM table_name LIMIT 10 OFFSET 20;          -- Skip 20, take 10
SELECT * FROM table_name FETCH FIRST 10 ROWS ONLY;    -- Standard SQL way
```

---

## Pattern Matching

### LIKE (Case Sensitive)

```sql
SELECT * FROM table_name WHERE column_name LIKE 'pattern%';   -- Starts with 'pattern'
SELECT * FROM table_name WHERE column_name LIKE '%pattern';   -- Ends with 'pattern'
SELECT * FROM table_name WHERE column_name LIKE '%pattern%';  -- Contains 'pattern'
SELECT * FROM table_name WHERE column_name LIKE '_pattern';   -- Single character + 'pattern'
SELECT * FROM table_name WHERE email LIKE '%@gmail.com';      -- Gmail addresses
```

### ILIKE (Case Insensitive)

```sql
SELECT * FROM table_name WHERE column_name ILIKE '%PATTERN%';
```

**Pattern Wildcards:**

- `%` - Matches zero or more characters
- `_` - Matches exactly one character

---

## Aggregation & Grouping

### Sorting

```sql
SELECT * FROM table_name ORDER BY column_name ASC;     -- Ascending (default)
SELECT * FROM table_name ORDER BY column_name DESC;    -- Descending
SELECT DISTINCT column_name FROM table_name ORDER BY column_name;  -- Unique values only
```

### GROUP BY

```sql
SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name;
```

Groups rows by column value and counts occurrences.

### HAVING Clause

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 5;
```

Filters grouped results (like WHERE but for groups).

---

## Functions & Calculations

### Aggregate Functions

```sql
SELECT MAX(column_name) FROM table_name;        -- Maximum value
SELECT MIN(column_name) FROM table_name;        -- Minimum value
SELECT AVG(column_name) FROM table_name;        -- Average value
SELECT ROUND(column_name, 2) FROM table_name;   -- Round to 2 decimal places
SELECT SUM(column_name) FROM table_name;        -- Sum of all values
```

### Grouped Aggregations

```sql
SELECT category, MAX(price), MIN(price), AVG(price)
FROM products
GROUP BY category;
```

### Column Aliases

```sql
SELECT column_name AS new_column_name FROM table_name;
SELECT COUNT(*) AS total_count FROM table_name;
```

---

## Date & Time Operations

### Current Date/Time

```sql
SELECT NOW();                    -- Current timestamp
SELECT NOW()::DATE;              -- Current date only
SELECT NOW()::TIME;              -- Current time only
SELECT CURRENT_DATE;             -- Current date (alternative)
SELECT CURRENT_TIME;             -- Current time (alternative)
```

### Date Arithmetic

```sql
SELECT NOW() + INTERVAL '1 YEAR';     -- Add 1 year
SELECT NOW() + INTERVAL '3 MONTHS';   -- Add 3 months
SELECT NOW() + INTERVAL '7 DAYS';     -- Add 7 days
SELECT NOW() - INTERVAL '1 HOUR';     -- Subtract 1 hour
```

### Extract Date Parts

```sql
SELECT EXTRACT(YEAR FROM NOW());      -- Extract year
SELECT EXTRACT(MONTH FROM NOW());     -- Extract month
SELECT EXTRACT(DAY FROM NOW());       -- Extract day
SELECT EXTRACT(DOW FROM NOW());       -- Day of week (0=Sunday)
```

### Calculate Age

```sql
SELECT column_name, AGE(NOW(), birth_date) FROM table_name;
SELECT AGE('2023-01-15', '1990-05-20');  -- Age between two dates
```

---

## NULL Handling

### COALESCE Function

```sql
SELECT COALESCE(NULL, NULL, 'default', 'backup');  -- Returns 'default'
SELECT COALESCE(column_name, 'N/A') FROM table_name;  -- Replace NULL with 'N/A'
```

Returns the first non-NULL value from the list.

### NULLIF Function

```sql
SELECT NULLIF(10, 10);    -- Returns NULL (values are equal)
SELECT NULLIF(10, 5);     -- Returns 10 (values are different)
```

Returns NULL if both values are equal, otherwise returns the first value.

---

## Constraints & Keys

### Drop Primary Key

```sql
ALTER TABLE table_name DROP CONSTRAINT table_name_pkey;
```

### Add Primary Key

```sql
ALTER TABLE table_name ADD PRIMARY KEY (id);
```

### Add Unique Constraint

```sql
ALTER TABLE table_name ADD UNIQUE (email);
```

### Add Check Constraint

```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name CHECK (age >= 18);
ALTER TABLE products ADD CONSTRAINT positive_price CHECK (price > 0);
```

### Foreign Key

```sql
-- When creating table
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id)
);

-- Add to existing table
ALTER TABLE orders ADD CONSTRAINT fk_user
FOREIGN KEY (user_id) REFERENCES users(id);
```

---

## Data Modification

### Delete Records

```sql
DELETE FROM table_name WHERE column_name = 'condition';
DELETE FROM table_name WHERE id IN (1, 2, 3);
```

⚠️ **Warning**: Always use WHERE clause to avoid deleting all records.

### Update Records

```sql
UPDATE table_name
SET
    column1 = 'new_value1',
    column2 = 'new_value2'
WHERE id = 1;
```

⚠️ **Warning**: Always use WHERE clause to avoid updating all records.

---

## Conflict Resolution

### Handle Insert Conflicts

```sql
-- Do nothing on conflict
INSERT INTO table_name (column1, column2)
VALUES ('value1', 'value2')
ON CONFLICT (unique_column) DO NOTHING;

-- Update on conflict (UPSERT)
INSERT INTO table_name (id, name, email)
VALUES (1, 'John', 'john@example.com')
ON CONFLICT (id) DO UPDATE SET
    name = EXCLUDED.name,
    email = EXCLUDED.email;
```

---

## Joins

### Inner Join

```sql
SELECT *
FROM table1
JOIN table2 ON table1.foreign_key = table2.primary_key;
```

Returns only rows that have matching values in both tables.

### Left Join

```sql
SELECT *
FROM table1
LEFT JOIN table2 ON table1.foreign_key = table2.primary_key;
```

Returns all rows from left table, with matching rows from right table (NULL if no match).

### Find Orphaned Records

```sql
SELECT *
FROM table1
LEFT JOIN table2 ON table1.foreign_key = table2.primary_key
WHERE table2.primary_key IS NULL;
```

Finds records in table1 that don't have corresponding records in table2.

**Join Examples:**

```sql
-- Get users with their orders
SELECT u.name, o.order_date, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

-- Get only users who have orders
SELECT u.name, COUNT(o.id) as order_count
FROM users u
JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.name;
```

---

## Extensions & UUIDs

### List Available Extensions

```sql
SELECT * FROM pg_available_extensions;
```

### Install Extension

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pgcrypto";
```

### List Installed Extensions

```sql
\dx
```

### List Functions

```sql
\df                              -- List all functions
\df *uuid*                       -- List UUID-related functions
```

### Generate UUID

```sql
SELECT uuid_generate_v4();       -- Generate random UUID
SELECT gen_random_uuid();        -- Alternative (requires pgcrypto)
```

**UUID Column Example:**

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## Best Practices & Tips

### Performance Tips

- Always use indexes on frequently queried columns
- Use `EXPLAIN ANALYZE` to understand query performance
- Avoid `SELECT *` in production code
- Use appropriate data types (don't use TEXT for everything)

### Security Tips

- Use parameterized queries to prevent SQL injection
- Grant minimal necessary permissions
- Regularly backup your databases
- Use constraints to ensure data integrity

### Common Data Types

```sql
-- Numeric
INTEGER, BIGINT, DECIMAL(precision, scale), NUMERIC
SERIAL, BIGSERIAL  -- Auto-incrementing

-- Text
VARCHAR(length), TEXT, CHAR(length)

-- Date/Time
DATE, TIME, TIMESTAMP, TIMESTAMPTZ

-- Boolean
BOOLEAN

-- JSON
JSON, JSONB  -- JSONB is binary, faster for queries

-- UUID
UUID
```

### Useful psql Commands

```sql
\l                    -- List all databases
\dt                   -- List tables
\du                   -- List users/roles
\q                    -- Quit psql
\h command            -- Help for specific SQL command
\?                    -- List all psql commands
\timing on            -- Show query execution time
```

---

## Common Query Patterns

### Pagination

```sql
SELECT * FROM products
ORDER BY id
LIMIT 20 OFFSET 40;  -- Page 3 (assuming 20 items per page)
```

### Top N per Group

```sql
WITH ranked_sales AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC) as rank
    FROM products
)
SELECT * FROM ranked_sales WHERE rank <= 3;
```

### Conditional Aggregation

```sql
SELECT
    category,
    COUNT(*) as total,
    COUNT(*) FILTER (WHERE price > 100) as expensive_items
FROM products
GROUP BY category;
```

### Window Functions

```sql
SELECT
    name,
    salary,
    AVG(salary) OVER (PARTITION BY department) as dept_avg,
    salary - AVG(salary) OVER (PARTITION BY department) as diff_from_avg
FROM employees;
```

---

## Error Prevention Checklist

- ✅ Always backup before running DROP or major ALTER statements
- ✅ Test queries on a small dataset first
- ✅ Use transactions for multiple related operations
- ✅ Include WHERE clauses in UPDATE and DELETE statements
- ✅ Validate foreign key relationships before inserting
- ✅ Check for NULL values when designing queries

---

**Remember**: PostgreSQL is case-insensitive for identifiers but case-sensitive for string data. Use single quotes for string literals and double quotes for identifiers that need case preservation.
