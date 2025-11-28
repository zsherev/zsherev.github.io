---
title: SQL Fundamentals — Key Takeaways and Notes
layout: default
---
# **SQL Fundamentals — Key Takeaways and Notes**

### **Intro**

I recently completed the _SQL Fundamentals_ room on TryHackMe. To reinforce what I learned—and to contribute a clear, beginner-friendly reference for others—I put together these notes in my own words. These fundamentals are essential for anyone moving toward offensive security, especially when learning SQL injection.

---

## **What Is SQL & Why It Matters in Security**

SQL (Structured Query Language) is the language used to interact with relational databases. It allows you to store, retrieve, modify, and delete data using simple, readable commands.

Because relational databases are everywhere, SQL is deeply connected to security:

- Applications rely on SQL queries to authenticate users.
- Poorly handled queries can expose sensitive information.
- Many critical vulnerabilities, including **SQL injection (SQLi)**, arise from improper query handling.

Understanding SQL is therefore a core skill for pentesters. The stronger your fundamentals, the easier SQLi labs, payload crafting, and real-world testing will be later.

---

## **Database & Table Management**

### **Database Commands**

```sql
CREATE DATABASE database_name;
SHOW DATABASES;
USE database_name;
DROP DATABASE database_name;
```

These commands are straightforward, but the biggest beginner mistake is forgetting the required semicolon at the end of each query.

---

### **Creating and Understanding Tables**

**General structure:**

```sql
CREATE TABLE example_table_name (
  example_column1 data_type,
  example_column2 data_type,
  example_column3 data_type
);
```

**Example from the room:**

```sql
CREATE TABLE book_inventory (
  book_id INT AUTO_INCREMENT PRIMARY KEY,
  book_name VARCHAR(255) NOT NULL,
  publication_date DATE
);
```

**What this does:**

- `book_id INT` — creates a column storing integer values
- `AUTO_INCREMENT` — automatically increases the value for new records
- `PRIMARY KEY` — uniquely identifies each row
- `book_name VARCHAR(255) NOT NULL` — variable-length text that cannot be empty
- `publication_date DATE` — stores date values

You can inspect table structure using:

```sql
DESC table_name;
```

---

### **Editing Tables**

Add a new column:

```sql
ALTER TABLE book_inventory
ADD page_count INT;
```

Remove a table:

```sql
DROP TABLE table_name;
```

---

## **CRUD Operations**

CRUD represents the four essential actions you can perform on data: **Create, Read, Update, Delete**. These are also the operations attackers manipulate during SQL injection.

---

### **Create — `INSERT`**

Used to add new records into a table.

```sql
INSERT INTO books (id, name, published_date, description)
VALUES (1, "Android Security Internals", "2014-10-14", "An In-Depth Guide to Android's Security Architecture");
```

---

### **Read — `SELECT`**

Retrieves data from a table.

All columns:

```sql
SELECT * FROM books;
```

Specific columns:

```sql
SELECT name, description FROM books;
```

---

### **Update — `UPDATE`**

Modifies existing records.

```sql
UPDATE books
SET description = "An In-Depth Guide to Android's Security Architecture."
WHERE id = 1;
```

---

### **Delete — `DELETE`**

Removes records from a table.

```sql
DELETE FROM books WHERE id = 1;
```

---

## **Clauses**

Clauses help refine, filter, and sort results. These are extremely important in SQL injection, where attackers manipulate clauses to control the application's logic.

---

### **ORDER BY**

Sorts results in ascending or descending order:

```sql
SELECT * FROM books ORDER BY published_date ASC;
```

---

### **HAVING**

Filters groups after a `GROUP BY` operation:

```sql
SELECT name, COUNT(*) 
FROM books 
GROUP BY name 
HAVING name LIKE '%Hack%';
```

---

## **Logical Operators**

Logical operators evaluate boolean conditions and determine how data is filtered.

- `LIKE` — pattern matching
- `AND` — true if both conditions are true
- `OR` — true if at least one condition is true
- `NOT` — reverses a condition
- `BETWEEN` — checks for values within a range

**Example:**

```sql
SELECT * FROM books
WHERE category = "Web Security"
AND publication_year BETWEEN 2020 AND 2024;
```

---

## **Comparison Operators**

Common comparison operators include:

- `=` (equal)
- `!=` (not equal)
- `<` (less than)
- `>` (greater than)
- `<=` (less than or equal to)
- `>=` (greater than or equal to)

**Example:**

```sql
SELECT * FROM books WHERE category != "Offensive Security";
```

---

## **Functions**

Functions help transform or summarize data. They often play an important role in SQL injection payloads, especially enumeration or data extraction.

---

### **String Functions**

- `CONCAT()` — combines strings
- `GROUP_CONCAT()` — merges multiple rows into one string
- `SUBSTRING()` — extracts characters from a string
- `LENGTH()` — counts characters in a string

**Example:**

```sql
SELECT category, GROUP_CONCAT(name SEPARATOR ", ") AS books
FROM books
GROUP BY category;
```

---

### **Aggregate Functions**

Useful for calculations and summarization:

- `COUNT()` — number of records
- `SUM()` — total value
- `MAX()` — highest value
- `MIN()` — lowest value

**Example:**

```sql
SELECT SUM(price) AS total_price FROM books;
```

---

## **Conclusion**

Understanding SQL fundamentals is the first major step toward mastering SQL injection and real-world database testing. This TryHackMe room helped me finally connect the dots between basic SQL commands and how they are abused in offensive security.

As I continue my journey toward pentesting, I plan to refer back to these notes often. For anyone following a similar path, I highly recommend building your own reference notes—writing concepts out in your own words makes them stick.
