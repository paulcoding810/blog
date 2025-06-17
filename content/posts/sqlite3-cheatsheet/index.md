---
date: '2025-06-16T15:19:51+07:00'
draft: false
title: 'SQLite3 Cheatsheet'
summary: 'A concise cheatsheet for SQLite3 commands and features.'
categories:
- Databases
tags:
- sqlite
- cheatsheet
- sql
- chatgpt
---

{{< gpt >}}

Here's a handy **SQLite3 cheatsheet** covering the most commonly used commands and features.

---

## 🗄️ **Database Basics**

```sql
-- Open or create a database (CLI)
sqlite3 mydatabase.db

-- Show tables
.tables

-- Show schema of a table
.schema tablename

-- Exit CLI
.quit
```

---

## 📁 **Creating Tables**

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT UNIQUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

---

## ✏️ **Insert Data**

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
```

---

## 🔍 **Query Data**

```sql
-- Basic SELECT
SELECT * FROM users;

-- WHERE clause
SELECT * FROM users WHERE name = 'Alice';

-- LIKE and wildcards
SELECT * FROM users WHERE name LIKE 'A%';

-- ORDER BY
SELECT * FROM users ORDER BY created_at DESC;

-- LIMIT
SELECT * FROM users LIMIT 10;
```

---

## 🛠️ **Update and Delete**

```sql
-- UPDATE
UPDATE users SET email = 'new@example.com' WHERE id = 1;

-- DELETE
DELETE FROM users WHERE id = 1;
```

---

## 🔄 **Joins**

```sql
-- INNER JOIN
SELECT u.name, o.total
FROM users u
JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN
SELECT u.name, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

---

## ⚙️ **Indexes**

```sql
CREATE INDEX idx_users_email ON users(email);
```

---

## ⚡ **Transactions**

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT; -- or ROLLBACK;
```

---

## 🧰 **Useful Commands (SQLite CLI)**

```sh
.headers ON
.mode column
.timer ON
```

---
