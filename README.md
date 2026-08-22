# 📘 Oracle SQL & MySQL – Complete DBMS Interview Notes

Complete merged reference: Oracle syntax throughout, with **MySQL differences called out right where they occur** (labeled 🐬 MySQL). Covers DBMS basics, Data Types, Constraints, DDL, DML, DQL, Functions, Joins, Operators, CASE, Subqueries, Grouping, TCL, DCL, Views, Sequences, Ranking Functions, Triggers, Indexing, Normalization, and interview Q&A.

---

## 📑 Table of Contents

1. [DBMS & SQL Basics](#1-dbms--sql-basics)
2. [Data Types](#2-data-types)
3. [Constraints](#3-constraints)
4. [DDL – Data Definition Language](#4-ddl--data-definition-language)
5. [DML – Data Manipulation Language](#5-dml--data-manipulation-language)
6. [DQL – Selection & Projection](#6-dql--selection--projection)
7. [SQL Functions](#7-sql-functions)
8. [SQL Joins](#8-sql-joins)
9. [Outer Joins – Practical Walkthrough](#9-outer-joins--practical-walkthrough)
10. [Self Join – Family Tree Example](#10-self-join--family-tree-example)
11. [SQL Operators](#11-sql-operators)
12. [CASE Expression](#12-case-expression)
13. [Subqueries, Correlated Subqueries & ROWNUM](#13-subqueries-correlated-subqueries--rownum)
14. [WHERE, GROUP BY, HAVING & ORDER BY](#14-where-group-by-having--order-by)
15. [TCL – Transaction Control Language](#15-tcl--transaction-control-language)
16. [DCL – Data Control Language](#16-dcl--data-control-language)
17. [Copying Tables (CTAS) & Creating New Users](#17-copying-tables-ctas--creating-new-users)
18. [SQL\*Plus Commands](#18-sqlplus-commands)
19. [Views & Materialized Views](#19-views--materialized-views)
20. [Sequences](#20-sequences)
21. [Ranking Functions – ROW_NUMBER, RANK, DENSE_RANK](#21-ranking-functions--row_number-rank-dense_rank)
22. [Triggers](#22-triggers)
23. [Indexing](#23-indexing)
24. [Normalization](#24-normalization)
25. [🎯 ROWNUM vs ROW_NUMBER vs RANK vs DENSE_RANK](#25--rownum-vs-row_number-vs-rank-vs-dense_rank)
26. [🧭 Decision Guide — When to Use What](#26--decision-guide--when-to-use-what)
27. [🔑 Interview Quick-Fire Points](#27--interview-quick-fire-points)
28. [💼 Most Asked Interview Questions](#28--most-asked-interview-questions)
29. [LPAD/RPAD, Hierarchical Queries (LEVEL), Type Conversion & DBA Concepts](#29-lpadrpad-hierarchical-queries-level-type-conversion--dba-concepts)

---

## 1. DBMS & SQL Basics

### Data
Raw facts which describe the attributes of an entity/object. Unprocessed / unorganised / unfiltered.
- Used to describe the attributes/properties of an object.
- Example: `Bus → Seat, Ticket, AC/NAC` (object → attributes)
- Attributes depend on the entity & situation.
- Data can be valid or invalid based on the situation.

### Information
Organised, processed, or filtered data.

### Database
A container used to store **interrelated data**.

### SQL
SQL is a query language used to interact with a database.
- Original name of SQL was **SEQUEL** (Structured English Query Language).
- SQL was introduced by **Raymond Boyce** and **Donald Chamberlin**.

### DBMS
Software used to manage and maintain a database using a language (SQL) by performing **CRUD** operations (Create, Read, Update, Delete).

🐬 **MySQL note:** MySQL is itself one specific RDBMS implementation (open-source, owned by Oracle Corp. since 2010), while "Oracle Database" is a separate, older, commercial RDBMS product. Both use ANSI SQL as a base but diverge in vendor-specific syntax — that's what this whole file tracks.

---

### Data Models

**1) Relational Model**
Stores data in the form of a **table**.
| Concept | Meaning |
|---|---|
| Table | Combination of rows and columns |
| Row (Tuple) | Represents all attributes of one entity |
| Column (Attribute) | Represents a single attribute of an entity |
| Cell | Intersection space between a row and a column |

> Relational model was introduced by **E.F. Codd**.

Example table:

| id | name | age |
|----|------|-----|
| 101 | dingu | 21 |
| 102 | dingi | 16 |
| 103 | raju | 25 |
| 104 | chutki | 18 |

**2) Document Model**
Stores data in the form of **JSON** or **XML**.

**JSON** (JavaScript Object Notation) – key → value pairs
```json
{
  "id": 101,
  "name": "Raju",
  "age": 21
}
```

**XML** (eXtensible Markup Language)
```xml
<student>
  <id>101</id>
  <name>Raju</name>
</student>
```

> Examples of Document DBs: **MongoDB, Redis, Cassandra**

---

### SQL Command Categories

| Language | Full Form | Purpose | Common Commands |
|---|---|---|---|
| **DDL** | Data Definition Language | Defines/modifies structure of DB objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`, `FLASHBACK`, `PURGE` |
| **DML** | Data Manipulation Language | Manipulates data inside tables | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | Retrieves/queries data | `SELECT` |
| **DCL** | Data Control Language | Controls access/permissions on the DB | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Manages transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

**Basic building blocks of SQL:**
- Datatypes / Operators
- DDL, DML, DCL, DQL, TCL
- Functions, `GROUP BY`, `WHERE`, `ORDER BY`, `HAVING`, `JOIN`, Subquery
- Normalization, Indexing, Triggers, Sequences, Views

[⬆ Back to top](#-table-of-contents)

---

## 2. Data Types

A **Data Type** specifies the type of data that can be stored in a table column. It determines what kind of data can be stored, how much memory is allocated, and what operations can be performed.

### CHAR
Character data type used to store alphabets, numbers, special characters (e.g. PAN number, IFSC code, gender M/F).
- **Fixed-length**, max size **2000 bytes**.
- Extra space is automatically padded with blanks.

```sql
CHAR(size)
-- Example
CHAR(3) → stores 'ABC' as |A|B|C|
```
```sql
CREATE TABLE student (gender CHAR(1));
```

✅ **Advantages:** Faster comparison, good for always-fixed-length data (Gender, State Code, Country Code).
❌ **Disadvantages:** Wastes memory if the stored value is shorter than the allocated size.
```text
CHAR(5), store 'A' → |A| | | | |   (unused space still reserved)
```

🐬 **MySQL:** `CHAR` works the same way; max size is **255 bytes** in MySQL (vs 2000 in Oracle).

### VARCHAR
Stores **variable-length** character data (alphabets, numbers, special characters).
- Max size **2000 bytes**, uses only the required memory — no wastage.

```sql
VARCHAR(size)
-- Example
VARCHAR(3) → store 'A' → |A|   store 'AB' → |A|B|
```
```sql
CREATE TABLE student (name VARCHAR(30));
```

✅ Saves memory, best when exact length is unknown (Employee Name, City, Address, Email).

🐬 **MySQL:** `VARCHAR` is MySQL's **primary/standard** string type (up to 65,535 bytes per row, shared across all VARCHAR columns in that row) — unlike Oracle where VARCHAR is legacy and VARCHAR2 is preferred.

### CHAR vs VARCHAR

| CHAR | VARCHAR |
|---|---|
| Fixed length | Variable length |
| Memory wastage possible | No memory wastage |
| Faster comparisons | Slightly slower |
| Use when length is fixed | Use when length is unknown |

### NUMBER
Used to store numeric values.
```sql
NUMBER(precision, scale)
```
- **Precision** → total number of digits.
- **Scale** → digits after the decimal point (optional).

| Declaration | Valid Example |
|---|---|
| `NUMBER(3)` | 374 |
| `NUMBER(4)` | 5761 |
| `NUMBER(5,2)` | 548.17 |
| `NUMBER(4,4)` | 0.7654 |

```sql
salary NUMBER(8,2)
```

🐬 **MySQL:** No single `NUMBER` type. MySQL splits numeric types by purpose:
| Oracle | MySQL Equivalent |
|---|---|
| `NUMBER(p)` (integer) | `INT`, `BIGINT`, `SMALLINT`, `TINYINT` |
| `NUMBER(p,s)` (decimal) | `DECIMAL(p,s)` or `NUMERIC(p,s)` |
| Floating point | `FLOAT`, `DOUBLE` |
```sql
-- MySQL equivalent
salary DECIMAL(8,2)
```

### DATE
Stores date values. Oracle default format: `DD-MON-YY` (e.g. `29-JUN-26`) or `DD-MON-YYYY` (e.g. `29-JUN-2026`). Oracle's `DATE` always includes both date **and** time (down to the second).
```sql
hiredate DATE
```

🐬 **MySQL:** `DATE` stores **date only** (`YYYY-MM-DD`), no time component. For date+time, MySQL uses a separate type:
| Oracle | MySQL |
|---|---|
| `DATE` (has date+time) | `DATETIME` (date+time) or `DATE` (date only) |
| — | `TIMESTAMP` (date+time, timezone-aware, auto-updates) |

### VARCHAR2
Oracle's **recommended** variable-length character data type. Stores up to **4000 bytes**. Oracle internally converts `VARCHAR` to `VARCHAR2`.
```sql
ename VARCHAR2(50)
```

| VARCHAR | VARCHAR2 |
|---|---|
| Old SQL standard | Oracle recommended |
| Up to 2000 bytes | Up to 4000 bytes |
| Rarely used | Mostly used in Oracle |

> 💡 **Interview Note:** Always use **VARCHAR2** in Oracle instead of VARCHAR.

🐬 **MySQL:** `VARCHAR2` **does not exist** in MySQL at all — this is 100% Oracle-only. Just use `VARCHAR(size)` in MySQL for everything.

### LOB (Large Object)
Used to store very large data.
| Type | Full Form | Stores |
|---|---|---|
| **CLOB** | Character Large Object | Large text, articles, documents, XML |
| **BLOB** | Binary Large Object | Images, videos, audio, PDF files |

```sql
resume CLOB
photo BLOB
```

🐬 **MySQL:** `BLOB` exists in both (same name, same purpose). For large text, MySQL uses `TEXT` (or `LONGTEXT` for very large content) instead of `CLOB`.
| Oracle | MySQL |
|---|---|
| `CLOB` | `TEXT` / `LONGTEXT` |
| `BLOB` | `BLOB` / `LONGBLOB` |

### Summary of Data Types (Oracle → MySQL quick map)

| Oracle Data Type | Used For | MySQL Equivalent |
|---|---|---|
| CHAR | Fixed-length characters | `CHAR` (same) |
| VARCHAR | Variable-length characters | `VARCHAR` (same, but this is MySQL's standard, not legacy) |
| VARCHAR2 | Oracle variable-length characters | `VARCHAR` (no VARCHAR2 in MySQL) |
| NUMBER(p) | Whole numbers | `INT` / `BIGINT` / `SMALLINT` |
| NUMBER(p,s) | Decimal numbers | `DECIMAL(p,s)` |
| DATE | Date + time | `DATE` (date only) or `DATETIME` (date+time) |
| CLOB | Large text | `TEXT` / `LONGTEXT` |
| BLOB | Images, videos, files | `BLOB` / `LONGBLOB` (same) |

[⬆ Back to top](#-table-of-contents)

---

## 3. Constraints

**Constraints** are rules applied to table columns to restrict invalid data. They improve **data integrity, accuracy, and consistency**.

### Types of Constraints
1. UNIQUE
2. NOT NULL
3. PRIMARY KEY
4. CHECK
5. DEFAULT
6. FOREIGN KEY

### 1) UNIQUE
Prevents duplicate values. **NULL values are allowed.**
```sql
email VARCHAR2(50) UNIQUE
```
✅ Allowed: `abc@gmail.com`, `xyz@gmail.com`, `NULL`
❌ Not Allowed: `abc@gmail.com` twice

### 2) NOT NULL
Does not allow NULL values — every row must contain a value.
```sql
name VARCHAR2(30) NOT NULL
```

### 3) PRIMARY KEY
Uniquely identifies every record. It is a combination of **UNIQUE + NOT NULL**.
- Duplicate values not allowed.
- NULL values not allowed.
- Every table *should* have one Primary Key (not mandatory, but highly recommended).

```sql
CREATE TABLE student (
    sid NUMBER PRIMARY KEY,
    name VARCHAR2(30)
);
```

### 4) CHECK
Provides additional validation — only allows values satisfying the given condition.
```sql
age NUMBER CHECK(age >= 18)
```
✅ Allowed: 18, 25, 40 &nbsp;&nbsp; ❌ Not Allowed: 15, 12

🐬 **MySQL note:** `CHECK` constraints were **silently ignored** in MySQL versions before 8.0.16 (parsed but not enforced!). From MySQL 8.0.16 onward, they're fully enforced — worth mentioning in interviews if asked about MySQL version quirks.

### 5) DEFAULT
Assigns a default value when the user doesn't provide one.
```sql
status VARCHAR2(20) DEFAULT 'ACTIVE'
```
```sql
INSERT INTO student(name) VALUES('Rahul');
-- Stored: Rahul | ACTIVE
```

### 6) FOREIGN KEY
Establishes a relationship between two tables by referencing the **Primary Key** of another (parent) table.
```sql
CREATE TABLE department (
    deptno NUMBER PRIMARY KEY,
    dname VARCHAR2(30)
);

CREATE TABLE employee (
    empno NUMBER PRIMARY KEY,
    ename VARCHAR2(30),
    deptno NUMBER,
    FOREIGN KEY(deptno) REFERENCES department(deptno)
);
```

🐬 **MySQL note:** Foreign keys are only enforced on the **InnoDB** storage engine — the older **MyISAM** engine silently ignores FK constraints. Always confirm engine type in MySQL interviews.

### Constraint Summary

| Constraint | Purpose |
|---|---|
| UNIQUE | Prevent duplicate values |
| NOT NULL | Prevent NULL values |
| PRIMARY KEY | Unique + Not Null |
| CHECK | Validate values |
| DEFAULT | Assign default value |
| FOREIGN KEY | Establish relationship between tables |

### Quick Interview Points
- `PRIMARY KEY = UNIQUE + NOT NULL`
- UNIQUE allows NULL; PRIMARY KEY does not.
- A table can have multiple UNIQUE constraints, but only **one** PRIMARY KEY.
- A table *can* exist without a Primary Key, though it's not recommended.
- FOREIGN KEY creates a **child–parent** relationship between two tables.
- 🔗 A **PRIMARY KEY is automatically indexed** by Oracle (and MySQL) — see [Section 23 – Indexing](#23-indexing).

[⬆ Back to top](#-table-of-contents)

---

## 4. DDL – Data Definition Language

**DDL** is a type of SQL language used to **define objects** in the database (tables, views, sequences, procedures, etc).

DDL Commands: `CREATE`, `RENAME`, `ALTER`, `TRUNCATE`, `DROP`, `FLASHBACK`, `PURGE`

### 1) CREATE
`CREATE` is a DDL command used to create a table, view, sequence, procedure, etc.

**Table Creation Syntax:**
```sql
CREATE TABLE tabname (
    coln1 datatype constraint,
    coln2 datatype(size) constraint,
    ...
);
```

```sql
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(20));
-- Table created.
```

### 2) RENAME
`RENAME` is a DDL command used to rename a table.
```sql
RENAME oldtabname TO newtabname;
```
```sql
RENAME emp TO student;
```

🐬 **MySQL:** Syntax differs — `RENAME TABLE oldname TO newname;` (also supports renaming multiple tables in one statement).

### 3) ALTER
`ALTER` is a DDL command used to alter (modify) the structure of an existing table.

| Operation | Oracle Syntax | 🐬 MySQL Syntax |
|---|---|---|
| Add column | `ALTER TABLE tabn ADD coln datatype constraint;` | Same |
| Rename column | `ALTER TABLE tabn RENAME COLUMN oldcoln TO newcoln;` | Same (8.0+) |
| Drop column | `ALTER TABLE tabn DROP COLUMN columnname;` | Same |
| Modify column | `ALTER TABLE tabname MODIFY colname datatype constraint;` | `ALTER TABLE tabname MODIFY COLUMN colname datatype;` (COLUMN keyword often required) or `CHANGE oldcol newcol datatype;` to rename+retype together |

```sql
-- Add column
ALTER TABLE stud ADD phno NUMBER(10);
-- Table altered

-- Modify column
ALTER TABLE stud MODIFY email VARCHAR2(25);
-- Table altered

-- Drop column
ALTER TABLE stud DROP COLUMN email;
-- Table altered
```

### 4) TRUNCATE
`TRUNCATE` deletes **all records** from a table **permanently**. It does **not** affect the structure of the table, and records **cannot be recovered**.
```sql
TRUNCATE TABLE tabname;
```
```sql
TRUNCATE TABLE stud;
-- Table truncated.

SELECT * FROM stud;
-- no rows selected
```

🐬 **MySQL:** Same command and behavior. Also resets `AUTO_INCREMENT` counter back to 1 (Oracle has no auto-increment counter to reset since it uses separate SEQUENCE objects).

### 5) DROP
`DROP` is a DDL command used to **delete a table**. The table gets moved to the **recycle bin** (it isn't gone forever — yet).
```sql
DROP TABLE tabname;
```
```sql
DROP TABLE student;
-- Table dropped.
```

🐬 **MySQL:** `DROP TABLE` deletes the table **immediately and permanently** — MySQL has **no recycle bin concept**. This is a key Oracle vs MySQL interview difference.

### 6) FLASHBACK
`FLASHBACK` is a DDL command used to **retrieve a table back from the recycle bin**.
```sql
FLASHBACK TABLE tabn TO BEFORE DROP;
```
```sql
FLASHBACK TABLE stud TO BEFORE DROP;
-- Flashback complete.
```

**Check what's in the recycle bin:**
```sql
SELECT original_name FROM recyclebin;
```
| original_name |
|---|
| stud |
| movie |
| Director |

🐬 **MySQL:** `FLASHBACK` **does not exist** in MySQL — this is 100% Oracle-only. In MySQL, once you drop a table it's gone; recovery relies entirely on backups or binary logs.

### 7) PURGE
`PURGE` is a DDL command used to **permanently delete a table from the recycle bin**. The table **must be present** in the recycle bin (i.e. must have been dropped first).
```sql
PURGE TABLE tabname;
```
```sql
DROP TABLE stud;
-- Table dropped.

PURGE TABLE stud;
-- Table purged.
```

⚠️ If you try to `PURGE` a table that was **never dropped**, Oracle throws:
```text
ORA-38307: object not in RECYCLEBIN
```
👉 You must `DROP` the table first (which sends it to the recycle bin), and *then* `PURGE` it.

🐬 **MySQL:** `PURGE` **does not exist** in MySQL either (Oracle-only, tied to the recycle bin concept). The closest MySQL analog, `PURGE BINARY LOGS`, is completely unrelated — it clears old replication log files, not dropped tables.

### Full DDL Walkthrough Example

```sql
-- 1) Create
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(20));
-- Table created.

-- 2) Insert some data
INSERT INTO stud VALUES (101, 'raju');
INSERT INTO stud VALUES (102, 'rani');

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | raju |
| 102 | rani |

```sql
-- 3) Truncate
TRUNCATE TABLE stud;
-- Table truncated.

SELECT * FROM stud;
-- no rows selected

-- 4) Insert again
INSERT INTO stud VALUES (101, 'raju');
INSERT INTO stud VALUES (102, 'rani');

-- 5) Drop
DROP TABLE stud;
-- Table dropped.

-- 6) Check recycle bin
SELECT original_name FROM recyclebin;
-- stud, movie, Director

-- 7) Flashback (retrieve dropped table)
FLASHBACK TABLE stud TO BEFORE DROP;
-- Flashback complete.

-- 8) Drop again, then purge permanently
DROP TABLE stud;
PURGE TABLE stud;
-- Table purged.
```

### DDL Command Summary

| Command | Purpose | Structure affected? | Recoverable? | 🐬 In MySQL? |
|---|---|---|---|---|
| `CREATE` | Create new object | — | — | ✅ Same |
| `RENAME` | Rename table | No | — | ✅ `RENAME TABLE` |
| `ALTER` | Add/Modify/Drop/Rename column | Yes | — | ✅ Same (minor syntax diff) |
| `TRUNCATE` | Delete all rows, keep structure | No | ❌ Not recoverable | ✅ Same |
| `DROP` | Delete table (→ recycle bin) | Yes | ✅ via FLASHBACK | ⚠️ Deletes permanently, no recycle bin |
| `FLASHBACK` | Restore dropped table | — | — | ❌ Doesn't exist |
| `PURGE` | Permanently remove from recycle bin | Yes | ❌ Not recoverable | ❌ Doesn't exist |

[⬆ Back to top](#-table-of-contents)

---

## 5. DML – Data Manipulation Language

**DML** is a type of SQL language used to **manipulate data** stored in tables.

DML Commands: `INSERT`, `UPDATE`, `DELETE`

### 1) INSERT
`INSERT` is a DML command used to insert records into a table.
```sql
INSERT INTO tabn VALUES (v1, v2, ...);
INSERT INTO tabn (col1, col2) VALUES (v1, v2, ...);
```
```sql
INSERT INTO stud VALUES (101, 'ramesh');
INSERT INTO stud (sid, name) VALUES (102, 'ramya');
```

🐬 **MySQL:** Identical syntax. MySQL also supports a shorthand `INSERT INTO tabn SET col1=v1, col2=v2;` which Oracle doesn't have.

### 2) UPDATE
`UPDATE` is a DML command used to update existing records.
⚠️ **Always use a `WHERE` condition** — otherwise every row in the table gets updated.
```sql
UPDATE tabn SET col=val, col=val, ...
WHERE cond;
```
```sql
-- Increase salary by 10% for clerks
UPDATE emp SET sal = sal + sal * 0.10 WHERE job = 'CLERK';
```

### 3) DELETE
`DELETE` is a DML command used to delete records.
⚠️ **Always use a `WHERE` condition** — otherwise every row is deleted.
```sql
DELETE FROM tabn WHERE cond;
```
```sql
DELETE FROM emp WHERE job = 'CLERK';
```

Identical in both Oracle and MySQL.

### DML Command Summary

| Command | Purpose | WHERE mandatory? |
|---|---|---|
| `INSERT` | Add new records | No |
| `UPDATE` | Modify existing records | ⚠️ Strongly recommended |
| `DELETE` | Remove records | ⚠️ Strongly recommended |

[⬆ Back to top](#-table-of-contents)

---

## 6. DQL – Selection & Projection

**DQL** (Data Query Language) is used to retrieve data from the database — its primary command is `SELECT`.

### Projection
**Projection** is a way of retrieving data by selecting only the **required columns**.
```sql
SELECT * / DISTINCT coln / expression, alias
FROM tabname;
```

### DISTINCT
`DISTINCT` is a SQL clause used to return a **single (unique) value** from multiple duplicate values in a column.
```sql
SELECT DISTINCT coln/expr FROM tabn;
```
```sql
SELECT DISTINCT sal FROM emp;
```
| distinct sal |
|---|
| 300 |
| 200 |
| 400 |

### Expression
An **Expression** is a combination of an **operator** and **operands**, or a statement that produces a result.
```sql
-- Example expression
a + b = c
```

### Alias
An **Alias** is an **alternative name** given to an existing column or expression.

Ways to write an alias:
```sql
coln/exp AS "alias-name"
coln/exp "alias-name"
coln/exp AS alias-name
coln/exp alias-name
```

🐬 **MySQL:** Same alias forms, but MySQL uses **backticks** ( `` ` `` ) instead of double quotes for identifiers with spaces/reserved words: `` coln AS `alias name` `` (Oracle uses `"alias name"`).

### Selection
**Selection** is a way of selecting **rows** and displaying data that satisfies a given condition.
```sql
SELECT * / DISTINCT col/expression alias
FROM tabname
WHERE condition;
```

### Order of Execution
| Step | Clause | Purpose |
|---|---|---|
| 1 | `FROM` | Checks whether the table is present or not |
| 2 | `WHERE` | Filters table records based on condition(s) |
| 3 | `SELECT` | Selects & displays rows one by one |

> Multiple conditions are allowed in the `WHERE` clause using `AND` / `OR`.

### Practice Queries

```sql
-- Q1: Display all details of emp when emp name is "Smith"
SELECT * FROM emp WHERE ename = 'SMITH';

-- Q2: Display all details when deptno is 20
SELECT * FROM emp WHERE deptno = 20;

-- Q3: Display all details when job is "clerk"
SELECT * FROM emp WHERE job = 'CLERK';
```

### Computed Column / Alias Practice

```sql
-- Quarterly salary
SELECT ename, (sal*12)/4 AS quarterly_sal FROM emp;

-- Salary with a flat bonus of 300
SELECT ename, sal + 300 AS sal_bonus FROM emp;

-- Salary with 10% increment
SELECT ename, (sal + (sal * 0.10)) AS incremented_sal FROM emp;

-- Salary with 30% decrement
SELECT ename, (sal - (sal * 0.30)) AS decreased_sal FROM emp;

-- Annual salary with 15% increment
SELECT ename, (sal*12) + (sal*12*0.15) AS annual_salary FROM emp;

-- Annual salary with 10% decrement
SELECT ename, (sal*12) - (sal*12*0.10) AS annual_salary FROM emp;
```

🐬 **MySQL:** All SELECT/WHERE/alias/computed-column syntax above is identical in MySQL — no differences here.

[⬆ Back to top](#-table-of-contents)

---

## 7. SQL Functions

A **function** is a block of code used to perform a specific task.

**Types:**
1. User Defined Function
2. Pre-defined (Built-in) Function → split into:
   - **Single Row Function** (n input → n output)
   - **Multi Row Function** (n input → 1 output)

### A. Character Single Row Function

**Case Manipulation**
| Function | Purpose | Example | Result |
|---|---|---|---|
| `UPPER(arg)` | To uppercase | `SELECT UPPER('raju') FROM dual;` | RAJU |
| `LOWER(arg)` | To lowercase | `SELECT LOWER('RAJU') FROM dual;` | raju |
| `INITCAP(arg)` | Capitalize first letter of each word | `SELECT INITCAP('manoj is kind') FROM dual;` | Manoj Is Kind |

🐬 **MySQL:** `UPPER`/`LOWER` same. `INITCAP` **doesn't exist** in MySQL — must build it manually with `CONCAT(UPPER(LEFT(str,1)), LOWER(SUBSTRING(str,2)))` or a stored function.

> 🐬 Also — Oracle's `FROM dual` (a dummy 1-row table used for expression evaluation) has **no equivalent requirement in MySQL**; MySQL allows `SELECT UPPER('raju');` directly with no FROM clause at all (though `FROM dual` is still accepted for compatibility).

**Character Manipulation**

`SUBSTR('str', pos, len)` – extracts characters from a string.
- `SUBSTR('Manu',1,3)` → `Man`
- First char: `SUBSTR(ename,1,1)` &nbsp;|&nbsp; Last letter: `SUBSTR(ename,LENGTH(ename),1)`
- First half: `SUBSTR(ename,1,FLOOR(LENGTH(ename)/2))`
- Second half: `SUBSTR(ename,CEIL(LENGTH(ename)/2))`

🐬 **MySQL:** Use `SUBSTRING()` (both `SUBSTR` and `SUBSTRING` actually work in MySQL as aliases — but `SUBSTRING` is the standard/preferred name).

`CONCAT('arg1','arg2')` – joins two strings.
- `SELECT CONCAT('Hi:',empname) FROM emp;`

🐬 **MySQL:** `CONCAT()` same, but MySQL also supports the `+`-style shortcut differently — actually MySQL does **not** support `||` by default either unless `PIPES_AS_CONCAT` SQL mode is set. Oracle uses `||` as its native string concatenation operator (e.g. `'Hi:' || empname`) — this doesn't work in default MySQL, always use `CONCAT()` there.

`REVERSE('arg')` – reverses a string.
- `SELECT REVERSE('FORD') FROM dual;` → `DORF`

🐬 **MySQL:** Same function name, same behavior.

`INSTR('str','char',pos,nth_occ)` – finds position/index of a character.
- `SELECT INSTR('NAYAN','A',1,1) FROM dual;` → 2
- `SELECT INSTR('NAYAN','A',1,2) FROM dual;` → 4
- Count occurrences of 'P': `LENGTH('Pushpa') - LENGTH(REPLACE('Pushpa','P',''))`
- Domain from email: `SUBSTR(email, INSTR(email,'@')+1)`

🐬 **MySQL:** `INSTR(str, substr)` exists but only takes **2 arguments** (no start-position or nth-occurrence params like Oracle's 4-argument version). For advanced positional search, use `LOCATE(substr, str, start_pos)` instead.

`TRIM(LEADING/TRAILING/BOTH 'char' FROM 'str')` – removes leading/trailing/both characters.
- `TRIM(LEADING 'P' FROM 'PUSHPA')` → `USHPA`
- `TRIM(TRAILING 'A' FROM 'PUSHPA')` → `PUSHP`

🐬 **MySQL:** Same `TRIM(LEADING/TRAILING/BOTH 'x' FROM str)` syntax supported identically.

`REPLACE('str','substr','newstr')` – replaces **all occurrences**.
- `REPLACE('BANGLORE','B','M')` → `MANGLORE`

🐬 **MySQL:** Identical.

### B. Number Single Row Function

| Function | Purpose | Example | Result |
|---|---|---|---|
| `MOD(m,n)` | Remainder | `MOD(11,2)` | 2 |
| `SQRT(arg)` | Square root | `SQRT(100)` | 10 |
| `POWER(m,n)` | m raised to power n | `POWER(2,3)` | 8 |
| `ROUND(n,d)` | Rounds to d decimal places | `ROUND(45.926,2)` | 45.93 |
| `TRUNC(n,d)` | Truncates (no rounding) to d places | `TRUNC(45.926,2)` | 45.92 |
| `CEIL(n)` | Smallest integer ≥ n | `CEIL(4.1)` | 5 |
| `FLOOR(n)` | Largest integer ≤ n | `FLOOR(4.9)` | 4 |
| `ABS(n)` | Absolute value | `ABS(-7)` | 7 |
| `SIGN(n)` | Returns -1, 0, or 1 | `SIGN(-45)` | -1 |

> 💡 `ROUND` vs `TRUNC`: ROUND adjusts based on the next digit, TRUNC just chops it off. `ROUND(45.926,0)=46`, `TRUNC(45.926,0)=45`.

🐬 **MySQL:** All identical **except** `TRUNC` → in MySQL it's spelled `TRUNCATE(n,d)` (extra letters, easy to confuse with the DDL `TRUNCATE TABLE` command — context tells them apart). Everything else (`MOD`, `SQRT`, `POWER`, `ROUND`, `CEIL`/`CEILING`, `FLOOR`, `ABS`, `SIGN`) works the same in both.

### C. Date Functions (Oracle)

| Function | Purpose | Example |
|---|---|---|
| `SYSDATE` | Current date & time | `SELECT SYSDATE FROM dual;` |
| `MONTHS_BETWEEN(d1,d2)` | Number of months between two dates | `MONTHS_BETWEEN(SYSDATE, hiredate)` |
| `ADD_MONTHS(date,n)` | Adds n months to a date | `ADD_MONTHS(SYSDATE, 3)` |
| `NEXT_DAY(date,'day')` | Next occurrence of given weekday | `NEXT_DAY(SYSDATE,'MONDAY')` |
| `LAST_DAY(date)` | Last day of the month | `LAST_DAY(SYSDATE)` |
| `ROUND(date,'fmt')` | Rounds a date (year/month) | `ROUND(SYSDATE,'YEAR')` |
| `TRUNC(date,'fmt')` | Truncates a date | `TRUNC(SYSDATE,'MONTH')` |

**Date Arithmetic:** In Oracle, `date + number` = adds days. `date1 - date2` = number of days between them.
```sql
SELECT hiredate + 30 FROM emp;          -- 30 days after hiredate
SELECT SYSDATE - hiredate FROM emp;     -- days employed
```

🐬 **MySQL equivalents (this whole section differs significantly):**

| Oracle | 🐬 MySQL |
|---|---|
| `SYSDATE` | `NOW()` or `SYSDATE()` |
| `MONTHS_BETWEEN(d1,d2)` | `TIMESTAMPDIFF(MONTH,d2,d1)` |
| `ADD_MONTHS(date,n)` | `DATE_ADD(date, INTERVAL n MONTH)` |
| `date + n` (add days) | `DATE_ADD(date, INTERVAL n DAY)` — MySQL does **not** allow plain `date + n` for day-arithmetic like Oracle does |
| `date1 - date2` (days between) | `DATEDIFF(date1, date2)` |
| `LAST_DAY(date)` | `LAST_DAY(date)` — same |
| `NEXT_DAY(date,'day')` | No direct equivalent — simulate with `DATE_ADD` + `WEEKDAY()` math |
| `ROUND(date,'fmt')` / `TRUNC(date,'fmt')` | No direct equivalent — use `DATE_FORMAT()` combined with string rebuilding |

### D. Conversion Functions

| Function | Purpose | Example |
|---|---|---|
| `TO_CHAR(value,'fmt')` | Converts number/date → string | `TO_CHAR(SYSDATE,'DD-MON-YYYY')` |
| `TO_DATE('str','fmt')` | Converts string → date | `TO_DATE('29-06-2026','DD-MM-YYYY')` |
| `TO_NUMBER('str')` | Converts string → number | `TO_NUMBER('123')` |

```sql
SELECT TO_CHAR(sal,'99,999.99') FROM emp;   -- formats number with commas
SELECT TO_CHAR(hiredate,'DD-MON-YYYY') FROM emp;
```

🐬 **MySQL equivalents:**
| Oracle | 🐬 MySQL |
|---|---|
| `TO_CHAR(date,'fmt')` | `DATE_FORMAT(date,'%d-%m-%Y')` |
| `TO_DATE('str','fmt')` | `STR_TO_DATE('str','%d-%m-%Y')` |
| `TO_NUMBER('str')` | `CAST(str AS SIGNED)` or `CAST(str AS DECIMAL)` |

> ⚠️ Format model codes differ too: Oracle uses `DD-MON-YYYY`, MySQL uses `%d-%b-%Y` (percent-prefixed codes) — a common source of bugs when porting queries between the two.

### E. Multi-Row (Aggregate/Group) Functions

n input rows → 1 output value. Ignore NULLs (except `COUNT(*)`).

| Function | Purpose |
|---|---|
| `SUM(col)` | Total of numeric column |
| `AVG(col)` | Average |
| `COUNT(col/*)` | Number of rows |
| `MAX(col)` | Maximum value |
| `MIN(col)` | Minimum value |

```sql
SELECT SUM(sal), AVG(sal), COUNT(*), MAX(sal), MIN(sal) FROM emp;

-- Department-wise total salary
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno;
```

> ⚠️ Any column in `SELECT` alongside an aggregate function **must** appear in `GROUP BY` — this is a very common interview trap.

🐬 **MySQL note:** Identical function names/behavior — **but** MySQL historically allowed non-aggregated, non-GROUP-BY columns in SELECT (silently picking an arbitrary row) unless `ONLY_FULL_GROUP_BY` SQL mode is enabled. Since MySQL 5.7.5+, `ONLY_FULL_GROUP_BY` is **on by default**, making it behave like Oracle (strict about this rule).

[⬆ Back to top](#-table-of-contents)

---

## 8. SQL Joins

A **JOIN** combines rows from two or more tables based on a related column.

### Types of Joins
1. **INNER JOIN** – returns only matching rows from both tables.
2. **LEFT (OUTER) JOIN** – all rows from left table + matched rows from right (unmatched → NULL).
3. **RIGHT (OUTER) JOIN** – all rows from right table + matched rows from left.
4. **FULL (OUTER) JOIN** – all rows from both tables, matched where possible.
5. **SELF JOIN** – a table joined with itself.
6. **CROSS JOIN** – Cartesian product (every row × every row).

```sql
-- INNER JOIN
SELECT e.ename, d.dname
FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno;

-- LEFT JOIN
SELECT e.ename, d.dname
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno;

-- FULL OUTER JOIN
SELECT e.ename, d.dname
FROM emp e
FULL OUTER JOIN dept d ON e.deptno = d.deptno;

-- CROSS JOIN
SELECT e.ename, d.dname FROM emp e CROSS JOIN dept d;
```

🐬 **MySQL note:** `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `CROSS JOIN` are all identical. But MySQL does **not** support `FULL OUTER JOIN` directly — must simulate with `UNION`:
```sql
SELECT e.ename, d.dname FROM emp e LEFT JOIN dept d ON e.deptno=d.deptno
UNION
SELECT e.ename, d.dname FROM emp e RIGHT JOIN dept d ON e.deptno=d.deptno;
```

### Old-style Oracle Join Syntax (pre-ANSI, still asked in interviews)
```sql
-- INNER JOIN (old style)
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno = d.deptno;

-- LEFT OUTER JOIN (old Oracle-specific syntax using (+))
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno = d.deptno(+);

-- RIGHT OUTER JOIN (old style)
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno(+) = d.deptno;
```
> The `(+)` goes on the side that can have **missing/NULL** matches.

🐬 **MySQL:** The `(+)` operator is **100% Oracle-only syntax** — doesn't exist in MySQL at all. MySQL always requires standard ANSI `JOIN ... ON` syntax.

[⬆ Back to top](#-table-of-contents)

---

## 9. Outer Joins – Practical Walkthrough

**Scenario:** `emp` table has employees, some with `deptno = NULL` (not yet assigned to a department). `dept` table has departments, some with no employees.

```sql
-- Employees with no department assigned (LEFT JOIN + IS NULL)
SELECT e.ename
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno
WHERE d.deptno IS NULL;

-- Departments with no employees (RIGHT JOIN + IS NULL)
SELECT d.dname
FROM emp e
RIGHT JOIN dept d ON e.deptno = d.deptno
WHERE e.empno IS NULL;

-- Symmetric difference: employees w/o dept UNION depts w/o employees
SELECT e.ename, NULL AS dname FROM emp e WHERE e.deptno IS NULL
UNION
SELECT NULL, d.dname FROM dept d WHERE d.deptno NOT IN (SELECT deptno FROM emp WHERE deptno IS NOT NULL);
```

This "LEFT JOIN + WHERE right.key IS NULL" pattern is a classic interview trick for finding **unmatched rows** — works identically in Oracle and MySQL, no differences here.

[⬆ Back to top](#-table-of-contents)

---

## 10. Self Join – Family Tree Example

A **Self Join** joins a table to itself, typically to model hierarchical/recursive relationships (employee–manager, parent–child).

```sql
CREATE TABLE person (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(30),
    parent_id NUMBER
);
```

| id | name | parent_id |
|---|---|---|
| 1 | Dashrath | NULL |
| 2 | Ram | 1 |
| 3 | Luv | 2 |
| 4 | Kush | 2 |

```sql
-- Find each person's parent name
SELECT c.name AS child, p.name AS parent
FROM person c
JOIN person p ON c.parent_id = p.id;
```

| child | parent |
|---|---|
| Ram | Dashrath |
| Luv | Ram |
| Kush | Ram |

**Classic employee-manager version:**
```sql
SELECT e.ename AS employee, m.ename AS manager
FROM emp e
LEFT JOIN emp m ON e.mgr = m.empno;
```

Identical syntax in both Oracle and MySQL for a basic self join. For multi-level hierarchy traversal (all descendants at any depth), the two diverge:

```sql
-- Oracle hierarchical query
SELECT name, LEVEL FROM person
START WITH parent_id IS NULL
CONNECT BY PRIOR id = parent_id;
```

🐬 **MySQL equivalent:** `CONNECT BY` **doesn't exist** in MySQL. Use a **recursive CTE** instead (MySQL 8.0+ only — no recursive CTE support before 8.0):
```sql
WITH RECURSIVE tree AS (
    SELECT id, name, parent_id, 1 AS lvl FROM person WHERE parent_id IS NULL
    UNION ALL
    SELECT p.id, p.name, p.parent_id, t.lvl+1
    FROM person p JOIN tree t ON p.parent_id = t.id
)
SELECT * FROM tree;
```

[⬆ Back to top](#-table-of-contents)

---

## 11. SQL Operators

| Category | Operators |
|---|---|
| Arithmetic | `+ - * /` |
| Comparison | `= != <> < > <= >=` |
| Logical | `AND OR NOT` |
| Range | `BETWEEN...AND` |
| Membership | `IN`, `NOT IN` |
| Pattern Matching | `LIKE` |
| NULL Check | `IS NULL`, `IS NOT NULL` |
| Set | `UNION`, `UNION ALL`, `INTERSECT`, `MINUS` (Oracle) |

```sql
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;
SELECT * FROM emp WHERE deptno IN (10,20,30);
SELECT * FROM emp WHERE ename LIKE 'S%';      -- starts with S
SELECT * FROM emp WHERE ename LIKE '%TH';     -- ends with TH
SELECT * FROM emp WHERE ename LIKE '_A%';     -- 2nd letter is A
SELECT * FROM emp WHERE comm IS NULL;
```

**LIKE wildcards:** `%` = any number of characters, `_` = exactly one character. Same in MySQL.

🐬 **MySQL note:** `LIKE` is case-**insensitive** by default in MySQL (depends on the column's collation, but most default collations are case-insensitive), while Oracle's `LIKE` is case-**sensitive** by default. Worth testing/mentioning if asked.

### Set Operators
```sql
SELECT deptno FROM emp
UNION                 -- combines + removes duplicates
SELECT deptno FROM dept;

SELECT deptno FROM emp
UNION ALL              -- combines, keeps duplicates
SELECT deptno FROM dept;

SELECT empno FROM emp
INTERSECT              -- common rows only
SELECT empno FROM emp_backup;

SELECT empno FROM emp
MINUS                   -- rows in first, not in second (Oracle only)
SELECT empno FROM emp_backup;
```

🐬 **MySQL note:** `UNION`, `UNION ALL` supported everywhere. `INTERSECT` supported only from **MySQL 8.0.31+**. `MINUS` **doesn't exist** in MySQL — use `EXCEPT` instead (also 8.0.31+), or simulate with `LEFT JOIN ... IS NULL` on older versions.

| Oracle | 🐬 MySQL |
|---|---|
| `MINUS` | `EXCEPT` (8.0.31+) |
| `INTERSECT` | `INTERSECT` (8.0.31+ only — errors on older versions) |

[⬆ Back to top](#-table-of-contents)

---

## 12. CASE Expression

`CASE` provides conditional (if-else style) logic inside SQL.

```sql
SELECT ename, sal,
  CASE
    WHEN sal >= 5000 THEN 'HIGH'
    WHEN sal >= 2000 THEN 'MEDIUM'
    ELSE 'LOW'
  END AS sal_grade
FROM emp;
```

**Simple CASE (matches exact values):**
```sql
SELECT ename, deptno,
  CASE deptno
    WHEN 10 THEN 'ACCOUNTING'
    WHEN 20 THEN 'RESEARCH'
    WHEN 30 THEN 'SALES'
    ELSE 'UNKNOWN'
  END AS dept_name
FROM emp;
```

**DECODE (Oracle-only shorthand for simple CASE):**
```sql
SELECT ename, DECODE(deptno,10,'ACCOUNTING',20,'RESEARCH',30,'SALES','UNKNOWN') AS dept_name
FROM emp;
```

🐬 **MySQL:** `CASE` syntax is fully identical (it's ANSI standard). `DECODE` **doesn't exist** in MySQL — always use `CASE` there. Also note MySQL has its own unrelated function called `IF(condition, val_if_true, val_if_false)` as a quick shorthand for simple two-way conditionals, which Oracle doesn't have (Oracle uses `DECODE` or `CASE` for that instead).

[⬆ Back to top](#-table-of-contents)

---

## 13. Subqueries, Correlated Subqueries & ROWNUM

### Subquery
A query nested inside another query. Executes first, its result is used by the outer query.

**Single-row subquery:**
```sql
SELECT ename, sal FROM emp WHERE sal > (SELECT AVG(sal) FROM emp);
```

**Multi-row subquery (use IN, ANY, ALL):**
```sql
SELECT ename FROM emp WHERE deptno IN (SELECT deptno FROM dept WHERE loc='CHICAGO');
SELECT ename FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=30);  -- greater than at least one
SELECT ename FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno=30); -- greater than all
```

**Subquery in FROM (inline view):**
```sql
SELECT deptno, avg_sal FROM (
    SELECT deptno, AVG(sal) AS avg_sal FROM emp GROUP BY deptno
) WHERE avg_sal > 3000;
```

🐬 **MySQL note:** All subquery forms above (single-row, IN/ANY/ALL, inline view) work identically in MySQL.

### Correlated Subquery
The inner query **references a column from the outer query** — executes once per outer row (unlike a normal subquery, which runs once total).

```sql
-- Employees earning more than the average of their own department
SELECT e.ename, e.sal, e.deptno
FROM emp e
WHERE e.sal > (
    SELECT AVG(sal) FROM emp WHERE deptno = e.deptno
);

-- EXISTS with correlated subquery
SELECT d.dname FROM dept d
WHERE EXISTS (SELECT 1 FROM emp e WHERE e.deptno = d.deptno);
```

| Subquery | Correlated Subquery |
|---|---|
| Runs once, independently | Runs once per outer row |
| No reference to outer query | References outer query column |
| Generally faster | Can be slower on large data |

Identical behavior/syntax in MySQL.

### ROWNUM (Oracle-only pseudo-column)
Assigns a sequential number to rows **as they're returned** (before ORDER BY is applied logically).

```sql
SELECT * FROM emp WHERE ROWNUM <= 5;          -- first 5 rows

-- Top N by salary (must wrap in subquery — ROWNUM filters BEFORE ORDER BY)
SELECT * FROM (
    SELECT * FROM emp ORDER BY sal DESC
) WHERE ROWNUM <= 3;
```
> ⚠️ Classic trap: `SELECT * FROM emp WHERE ROWNUM > 1;` returns **zero rows** — ROWNUM is assigned incrementally starting at 1, so a condition like `> 1` never matches on the first evaluated row and the sequence can't restart.

🐬 **MySQL equivalent:** MySQL has **no ROWNUM at all** — this entire concept is Oracle-only. MySQL uses `LIMIT` (much simpler, no subquery-wrapping trap):
```sql
SELECT * FROM emp ORDER BY sal DESC LIMIT 3;
SELECT * FROM emp LIMIT 5 OFFSET 10;   -- pagination (skip 10, take next 5)
```
> 💡 Note: Oracle 12c+ also added simpler pagination syntax as an alternative to ROWNUM: `SELECT * FROM emp ORDER BY sal DESC OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;` — closer to MySQL's LIMIT/OFFSET, but ROWNUM is still commonly asked about in interviews.

[⬆ Back to top](#-table-of-contents)

---

## 14. WHERE, GROUP BY, HAVING & ORDER BY

### Logical Order of Execution (important interview question!)
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```

| Clause | Filters | Can use aggregate functions? |
|---|---|---|
| `WHERE` | Individual rows (before grouping) | ❌ No |
| `GROUP BY` | Groups rows by column(s) | — |
| `HAVING` | Groups (after aggregation) | ✅ Yes |
| `ORDER BY` | Final sort of output | ✅ Yes |

```sql
-- Departments with more than 3 employees earning above 2000
SELECT deptno, COUNT(*) AS emp_count
FROM emp
WHERE sal > 2000
GROUP BY deptno
HAVING COUNT(*) > 3
ORDER BY emp_count DESC;
```

> 💡 **WHERE vs HAVING** — the #1 asked SQL question. `WHERE` filters rows *before* grouping and cannot use aggregate functions; `HAVING` filters *groups* after aggregation and can.

🐬 **MySQL note:** Identical execution order and behavior. One MySQL-specific quirk: MySQL allows referencing a `SELECT`-clause **alias** inside `GROUP BY` and `HAVING` (e.g. `GROUP BY emp_count`), which Oracle does **not** allow in `HAVING` (Oracle requires the full expression or the actual column, not the alias, in most cases).

[⬆ Back to top](#-table-of-contents)

---

## 15. TCL – Transaction Control Language

Manages **transactions** — a transaction is a set of operations treated as a single unit (all succeed, or all fail).

| Command | Purpose |
|---|---|
| `COMMIT` | Permanently saves changes made in the current transaction |
| `ROLLBACK` | Undoes changes made in the current transaction |
| `SAVEPOINT` | Sets a named point within a transaction to roll back to |

```sql
UPDATE emp SET sal = sal + 500 WHERE deptno = 10;
SAVEPOINT sp1;

DELETE FROM emp WHERE deptno = 20;
ROLLBACK TO sp1;   -- undoes the DELETE, keeps the UPDATE

COMMIT;            -- permanently saves the UPDATE
```

### ACID Properties (transaction guarantees)
| Property | Meaning |
|---|---|
| **Atomicity** | Transaction is all-or-nothing |
| **Consistency** | DB moves from one valid state to another |
| **Isolation** | Concurrent transactions don't interfere with each other |
| **Durability** | Once committed, changes persist even after a crash |

🐬 **MySQL note:** TCL syntax (`COMMIT`, `ROLLBACK`, `SAVEPOINT`, `ROLLBACK TO`) works the same way — **but only on InnoDB** (transaction-safe) tables. The older **MyISAM** engine does not support transactions/rollback at all (every statement auto-commits immediately, no undo). Also, by default MySQL runs in `autocommit=1` mode (every individual statement auto-commits unless you explicitly `START TRANSACTION`), whereas Oracle does **not** auto-commit by default — a very commonly asked difference.

[⬆ Back to top](#-table-of-contents)

---

## 16. DCL – Data Control Language

Controls **access/permissions** on database objects.

| Command | Purpose |
|---|---|
| `GRANT` | Gives privileges to a user |
| `REVOKE` | Removes privileges from a user |

```sql
GRANT SELECT, INSERT ON emp TO ramesh;
GRANT ALL PRIVILEGES ON emp TO ramesh;
REVOKE INSERT ON emp FROM ramesh;
```

🐬 **MySQL:** Syntax is nearly identical, but MySQL requires specifying the **host** as part of the username, and needs `FLUSH PRIVILEGES;` after manual grant-table edits (not needed after standard `GRANT`/`REVOKE` statements, but commonly run as a safety habit):
```sql
GRANT SELECT, INSERT ON db1.emp TO 'ramesh'@'localhost';
REVOKE INSERT ON db1.emp FROM 'ramesh'@'localhost';
FLUSH PRIVILEGES;
```

[⬆ Back to top](#-table-of-contents)

---

## 17. Copying Tables (CTAS) & Creating New Users

### CTAS – Create Table As Select
Creates a new table using the structure **and data** of a query result.
```sql
CREATE TABLE emp_backup AS SELECT * FROM emp;
CREATE TABLE clerks AS SELECT * FROM emp WHERE job='CLERK';

-- Structure only, no data
CREATE TABLE emp_empty AS SELECT * FROM emp WHERE 1=2;
```
> Constraints (PK, FK, checks) are **not** copied by CTAS — only NOT NULL is preserved. Indexes must be recreated manually.

🐬 **MySQL:** CTAS syntax identical — `CREATE TABLE ... AS SELECT ...` works the same way and has the same constraint-copying limitation.

### Creating New Users (Oracle)
```sql
CREATE USER ramesh IDENTIFIED BY password123;
GRANT CONNECT, RESOURCE TO ramesh;
ALTER USER ramesh ACCOUNT UNLOCK;
```

🐬 **MySQL equivalent:**
```sql
CREATE USER 'ramesh'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON db1.* TO 'ramesh'@'localhost';
FLUSH PRIVILEGES;
```

[⬆ Back to top](#-table-of-contents)

---

## 18. SQL*Plus Commands (Oracle client-only, not SQL itself)

| Command | Purpose |
|---|---|
| `DESC tabname` | Describes table structure (columns, types, nullability) |
| `SET LINESIZE n` | Sets output line width |
| `SET PAGESIZE n` | Rows displayed per page |
| `SPOOL filename` | Saves query output to a file |
| `SPOOL OFF` | Stops spooling |
| `/` | Re-runs the last SQL command |
| `SHOW USER` | Displays current logged-in user |

```sql
DESC emp;
SPOOL output.txt;
SELECT * FROM emp;
SPOOL OFF;
```

🐬 **MySQL equivalent:** `DESCRIBE tabname;` or `SHOW COLUMNS FROM tabname;` for structure (both work; `DESC` shorthand also works in the MySQL CLI). MySQL's CLI has no SPOOL command — use `mysql -e "query" > file.txt` from the shell instead, or `SELECT ... INTO OUTFILE 'path'` inside a query. `LINESIZE`/`PAGESIZE` are Oracle-only formatting settings with no MySQL CLI equivalent.

[⬆ Back to top](#-table-of-contents)

---

## 19. Views & Materialized Views

### View
A **virtual table** based on a SQL query — doesn't store data itself, just the query definition. Data is fetched live from underlying table(s) each time it's queried.

```sql
CREATE VIEW emp_view AS
SELECT empno, ename, sal FROM emp WHERE deptno = 10;

SELECT * FROM emp_view;

CREATE OR REPLACE VIEW emp_view AS
SELECT empno, ename, sal, deptno FROM emp WHERE deptno = 10;

DROP VIEW emp_view;
```

✅ **Advantages:** Simplifies complex queries, adds a security layer (hide sensitive columns), provides logical data independence.
❌ **Limitation:** Views with joins/aggregates/DISTINCT are often **not updatable** (can't INSERT/UPDATE/DELETE through them).

🐬 **MySQL:** `CREATE VIEW`, `CREATE OR REPLACE VIEW`, `DROP VIEW` all work identically.

### Materialized View
Like a view, but **physically stores** the query result (a snapshot). Needs manual/scheduled refresh to stay current — much faster to query since data doesn't need to be recomputed each time.

```sql
CREATE MATERIALIZED VIEW mv_emp_summary
AS SELECT deptno, SUM(sal) AS total_sal FROM emp GROUP BY deptno;

-- Refresh manually
EXEC DBMS_MVIEW.REFRESH('mv_emp_summary');
```

| View | Materialized View |
|---|---|
| Virtual, no storage | Physical, stores data |
| Always up-to-date | Needs refresh |
| Slower for complex queries | Faster (pre-computed) |

🐬 **MySQL note:** MySQL has **no native Materialized View object at all** — this is a major Oracle-only feature. Regular `VIEW` behaves the same as Oracle's, but materialized views must be simulated manually: create a real table, then populate/refresh it periodically using a scheduled `EVENT` (MySQL's cron-like scheduler) or an application job.

[⬆ Back to top](#-table-of-contents)

---

## 20. Sequences

A **Sequence** auto-generates unique numeric values, typically for primary keys.

```sql
CREATE SEQUENCE emp_seq
START WITH 1
INCREMENT BY 1
MAXVALUE 9999
NOCYCLE;

-- Use it
INSERT INTO emp(empno, ename) VALUES (emp_seq.NEXTVAL, 'Ramesh');

SELECT emp_seq.CURRVAL FROM dual;   -- current value (after NEXTVAL used at least once)
SELECT emp_seq.NEXTVAL FROM dual;   -- next value, and advances the sequence

ALTER SEQUENCE emp_seq INCREMENT BY 2;
DROP SEQUENCE emp_seq;
```

| Pseudo-column | Purpose |
|---|---|
| `NEXTVAL` | Generates & returns the next value in sequence |
| `CURRVAL` | Returns the current (last generated) value |

🐬 **MySQL note:** Standard MySQL has **no `CREATE SEQUENCE` object at all** (only MariaDB, a MySQL fork, added it). Vanilla MySQL instead uses `AUTO_INCREMENT` directly on the column — a fundamentally different approach (attached to a column, not a standalone reusable object):
```sql
CREATE TABLE emp (
    empno INT AUTO_INCREMENT PRIMARY KEY,
    ename VARCHAR(30)
);
INSERT INTO emp(ename) VALUES ('Ramesh');   -- empno generated automatically
SELECT LAST_INSERT_ID();                     -- MySQL equivalent of CURRVAL
```

| Oracle | 🐬 MySQL |
|---|---|
| `SEQUENCE` object, reusable across tables | `AUTO_INCREMENT`, tied to one column in one table |
| `.NEXTVAL` | Automatic on INSERT |
| `.CURRVAL` | `LAST_INSERT_ID()` |
| `ALTER SEQUENCE ... INCREMENT BY n` | `ALTER TABLE ... AUTO_INCREMENT = n` (sets next value, not step size) |

[⬆ Back to top](#-table-of-contents)

---

## 21. Ranking Functions – ROW_NUMBER, RANK, DENSE_RANK

All are **window (analytic) functions** — used with `OVER (PARTITION BY ... ORDER BY ...)`.

```sql
SELECT ename, deptno, sal,
  ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rn,
  RANK()       OVER (PARTITION BY deptno ORDER BY sal DESC) AS rnk,
  DENSE_RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS drnk
FROM emp;
```

**Behavior with ties** (say two employees tie for 2nd highest salary in a dept):

| ename | sal | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|---|
| A | 5000 | 1 | 1 | 1 |
| B | 4000 | 2 | 2 | 2 |
| C | 4000 | 3 | 2 | 2 |
| D | 3000 | 4 | 4 | 3 |

- **ROW_NUMBER** → always unique, sequential, no gaps, no ties.
- **RANK** → same rank for ties, but **skips** the next rank(s) (gap after tie).
- **DENSE_RANK** → same rank for ties, **no gap** — next rank continues immediately.

```sql
-- Nth highest salary per department, using ROW_NUMBER (handles ties safely)
SELECT * FROM (
    SELECT ename, deptno, sal,
           ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rn
    FROM emp
) WHERE rn = 2;
```

🐬 **MySQL note:** Window functions (`ROW_NUMBER`, `RANK`, `DENSE_RANK`, `OVER`, `PARTITION BY`) are supported with **identical syntax** — but **only from MySQL 8.0 onward**. MySQL 5.7 and earlier have **no window functions at all**; the old workaround was user-defined session variables (`@rownum := @rownum + 1`), which is a common "how did people do this before 8.0" interview question.

[⬆ Back to top](#-table-of-contents)

---

## 22. Triggers

A **Trigger** is a stored PL/SQL block that automatically executes in response to an event (`INSERT`, `UPDATE`, `DELETE`) on a table.

```sql
CREATE OR REPLACE TRIGGER trg_before_insert
BEFORE INSERT ON emp
FOR EACH ROW
BEGIN
    :NEW.hiredate := SYSDATE;
END;
/
```

| Trigger Type | Fires |
|---|---|
| `BEFORE INSERT/UPDATE/DELETE` | Before the DML event — often used for validation/defaults |
| `AFTER INSERT/UPDATE/DELETE` | After the DML event — often used for logging/auditing |
| `FOR EACH ROW` | Row-level trigger (fires once per affected row) |
| Statement-level (no `FOR EACH ROW`) | Fires once per statement, regardless of row count |

`:OLD` and `:NEW` refer to the row's values before and after the change (available in row-level triggers).

```sql
-- Audit log trigger example
CREATE OR REPLACE TRIGGER trg_audit_sal
AFTER UPDATE OF sal ON emp
FOR EACH ROW
BEGIN
    INSERT INTO sal_audit(empno, old_sal, new_sal, changed_on)
    VALUES (:OLD.empno, :OLD.sal, :NEW.sal, SYSDATE);
END;
/
```

🐬 **MySQL equivalent syntax (several real differences here):**
```sql
DELIMITER //
CREATE TRIGGER trg_before_insert
BEFORE INSERT ON emp
FOR EACH ROW
BEGIN
    SET NEW.hiredate = NOW();
END//
DELIMITER ;
```

| Oracle | 🐬 MySQL |
|---|---|
| `:OLD.col` / `:NEW.col` (colon prefix) | `OLD.col` / `NEW.col` (no colon) |
| Ends with `END; /` | Needs `DELIMITER //` switch before, `DELIMITER ;` after (since `;` is used inside the trigger body) |
| Supports statement-level triggers (no `FOR EACH ROW`) | **Every trigger is row-level** — `FOR EACH ROW` is mandatory, no statement-level option |
| `AFTER UPDATE OF sal ON emp` (column-specific trigger) | No column-specific trigger — fires on **any** UPDATE, must check `IF NEW.sal <> OLD.sal` manually inside the body |

[⬆ Back to top](#-table-of-contents)

---

## 23. Indexing

An **Index** is a database object that speeds up data retrieval by creating a fast lookup structure (commonly a B-tree) on one or more columns — at the cost of slower writes and extra storage.

```sql
CREATE INDEX idx_ename ON emp(ename);
CREATE UNIQUE INDEX idx_empno ON emp(empno);
DROP INDEX idx_ename;
```

🐬 **MySQL:** `CREATE INDEX`/`CREATE UNIQUE INDEX` identical. But `DROP INDEX` syntax differs slightly — MySQL requires specifying the table: `DROP INDEX idx_ename ON emp;` (Oracle just needs the index name, since index names are globally unique per schema in Oracle but scoped to the table in MySQL).

### Types of Indexes
| Type | Description |
|---|---|
| **B-Tree Index** | Default type; balanced tree, good for equality & range queries |
| **Bitmap Index** | Good for low-cardinality columns (e.g. gender, status); Oracle-specific, used in data warehousing |
| **Unique Index** | Enforces uniqueness (auto-created for PK/UNIQUE constraints) |
| **Composite Index** | Index on multiple columns together |

✅ **Speeds up:** `SELECT`, `WHERE`, `JOIN`, `ORDER BY` on indexed columns.
❌ **Slows down:** `INSERT`, `UPDATE`, `DELETE` (index must be updated too).

> 🔗 Recall from Section 3: a **PRIMARY KEY is automatically indexed** by Oracle (and MySQL) — no need to create one manually.

🐬 **MySQL note:** Default index type is also B-Tree. MySQL additionally supports **FULLTEXT** indexes (for natural-language text search, e.g. `MATCH() AGAINST()`) and **HASH** indexes (mainly in the Memory storage engine). **Bitmap indexes don't exist in MySQL at all** — that's Oracle-only, tied to Oracle's data-warehousing feature set.

[⬆ Back to top](#-table-of-contents)

---

## 24. Normalization

**Normalization** organizes data to reduce **redundancy** and avoid **update/insert/delete anomalies**, by breaking large tables into smaller, related ones.

### 1NF (First Normal Form)
- Each column contains **atomic (indivisible)** values.
- No repeating groups / no multi-valued columns.

❌ Violates 1NF:
| id | name | phones |
|---|---|---|
| 1 | Ravi | 9876543210, 9123456780 |

✅ Fixed (1NF):
| id | name | phone |
|---|---|---|
| 1 | Ravi | 9876543210 |
| 1 | Ravi | 9123456780 |

### 2NF (Second Normal Form)
- Must already be in 1NF.
- No **partial dependency** — every non-key column must depend on the **entire** composite primary key, not just part of it. (Only relevant when PK is composite.)

❌ Violates 2NF (PK = studentID+courseID, but `student_name` depends only on studentID):
| studentID | courseID | student_name | grade |
|---|---|---|---|

✅ Fixed → split into `student(studentID, student_name)` and `enrollment(studentID, courseID, grade)`.

### 3NF (Third Normal Form)
- Must already be in 2NF.
- No **transitive dependency** — non-key columns must depend only on the primary key, not on other non-key columns.

❌ Violates 3NF (zip determines city, but city isn't directly dependent on the PK empno):
| empno | zip | city |
|---|---|---|

✅ Fixed → split into `employee(empno, zip)` and `zip_city(zip, city)`.

### BCNF (Boyce-Codd Normal Form)
- Stricter version of 3NF — for every functional dependency `X → Y`, X must be a **super key**.

### Summary Table
| Form | Rule |
|---|---|
| 1NF | Atomic values, no repeating groups |
| 2NF | 1NF + no partial dependency (on composite key) |
| 3NF | 2NF + no transitive dependency |
| BCNF | 3NF + every determinant is a super key |

> 💡 Denormalization (opposite of normalization) is sometimes done deliberately for read-heavy systems to reduce JOINs and improve performance — a common trade-off discussion in interviews.

This concept is **pure relational theory**, identical across Oracle, MySQL, and any RDBMS — not vendor-specific at all.

[⬆ Back to top](#-table-of-contents)

---

## 25. 🎯 ROWNUM vs ROW_NUMBER vs RANK vs DENSE_RANK

| Feature | ROWNUM | ROW_NUMBER() | RANK() | DENSE_RANK() |
|---|---|---|---|---|
| Vendor | Oracle only | Oracle + 🐬 MySQL 8+ | Oracle + 🐬 MySQL 8+ | Oracle + 🐬 MySQL 8+ |
| Needs `OVER()` | No | Yes | Yes | Yes |
| Assigned before/after ORDER BY | Before (unless wrapped in subquery) | Follows the OVER's ORDER BY | Follows the OVER's ORDER BY | Follows the OVER's ORDER BY |
| Handles ties | N/A (no ordering awareness) | No ties — always unique | Ties get same rank, gap after | Ties get same rank, no gap |
| Supports PARTITION BY | No | Yes | Yes | Yes |
| Typical use | Limit rows (Oracle pagination) | Unique row numbering per group | Leaderboard-style ranking with gaps | Leaderboard-style ranking without gaps |
| 🐬 MySQL equivalent for row-limiting | `LIMIT` / `LIMIT OFFSET` | — | — | — |

[⬆ Back to top](#-table-of-contents)

---

## 26. 🧭 Decision Guide — When to Use What

| Need | Oracle | 🐬 MySQL |
|---|---|---|
| Fixed-length code (gender, state code) | `CHAR` | `CHAR` (same) |
| Variable-length text, exact length unknown | `VARCHAR2` | `VARCHAR` |
| Large text (articles, resumes) | `CLOB` | `TEXT` / `LONGTEXT` |
| Images/files | `BLOB` | `BLOB` / `LONGBLOB` |
| Auto-generated unique IDs | `SEQUENCE` | `AUTO_INCREMENT` |
| Reusable complex query, no storage | `VIEW` | `VIEW` (same) |
| Reusable complex query, pre-computed for speed | `MATERIALIZED VIEW` | Table + scheduled `EVENT` (no native support) |
| Auto-run logic on data change | `TRIGGER` | `TRIGGER` (minor syntax diffs) |
| Speed up SELECT/WHERE/JOIN | `INDEX` | `INDEX` (same) |
| Get Nth highest value safely (with ties) | `ROW_NUMBER()` in subquery | `ROW_NUMBER()` in subquery (8.0+) |
| Leaderboard ranks allowing/disallowing gaps | `RANK()` / `DENSE_RANK()` | `RANK()` / `DENSE_RANK()` (8.0+) |
| Filter individual rows before grouping | `WHERE` | `WHERE` (same) |
| Filter after aggregation/grouping | `HAVING` | `HAVING` (same) |
| Undo uncommitted changes | `ROLLBACK` (optionally to `SAVEPOINT`) | `ROLLBACK` — InnoDB only |
| Permanently save changes | `COMMIT` | `COMMIT` — autocommit is ON by default |
| Reduce data redundancy | Normalization (1NF→3NF/BCNF) | Same (vendor-neutral theory) |
| Improve read performance at cost of redundancy | Denormalization | Same |
| Recover an accidentally dropped table | `FLASHBACK` | ❌ No equivalent — rely on backups/binlogs |
| Row limiting / pagination | `ROWNUM` + subquery, or `OFFSET...FETCH` (12c+) | `LIMIT OFFSET` (much simpler) |
| Simple string concatenation | `\|\|` operator | `CONCAT()` function |

[⬆ Back to top](#-table-of-contents)

---

## 27. 🔑 Interview Quick-Fire Points

- `DELETE` is DML (logged, rollback-able, can use WHERE); `TRUNCATE` is DDL (not rollback-able*, resets storage, no WHERE); `DROP` removes the table entirely.
  *(In Oracle, TRUNCATE auto-commits and cannot be rolled back; in MySQL/InnoDB it behaves similarly — implicit commit.)*
- `WHERE` filters rows before grouping; `HAVING` filters groups after aggregation.
- `PRIMARY KEY` = `UNIQUE` + `NOT NULL`; only one PK per table, but multiple UNIQUE constraints allowed.
- A `FOREIGN KEY` enforces referential integrity between a child and parent table — **only enforced on MySQL's InnoDB engine**, not MyISAM.
- `VARCHAR2` is Oracle-only; MySQL just uses `VARCHAR` for everything.
- `UNION` removes duplicates (implicit sort/dedup — slower); `UNION ALL` keeps duplicates (faster).
- Correlated subqueries run once **per outer row**; plain subqueries run **once**.
- `RANK()` leaves gaps after ties; `DENSE_RANK()` does not; `ROW_NUMBER()` never ties. All require MySQL 8.0+.
- Views can hide sensitive columns and simplify complex joins for end users — a security & abstraction tool. Works the same in both.
- Index speeds up reads, slows down writes — don't over-index write-heavy tables.
- ACID = Atomicity, Consistency, Isolation, Durability — the four guarantees of a reliable transaction.
- Normalization reduces redundancy; over-normalizing can hurt performance due to excessive JOINs (hence denormalization in read-heavy analytics systems).
- Oracle has `SEQUENCE` + `FLASHBACK` + `MATERIALIZED VIEW` + `ROWNUM` + `(+)` outer join syntax — **none of these five exist in MySQL.**
- MySQL has `LIMIT`/`OFFSET`, `AUTO_INCREMENT`, and requires `CONCAT()` instead of `||` — simpler in places, but with real feature gaps vs Oracle in enterprise use cases.
- Default transaction behavior differs: Oracle requires explicit `COMMIT`; MySQL auto-commits every statement by default unless a transaction is explicitly started.

[⬆ Back to top](#-table-of-contents)

---

## 28. 💼 Most Asked Interview Questions

**Q1: Difference between DELETE, TRUNCATE, and DROP?**
DELETE removes specific rows (DML, uses WHERE, rollback-able), TRUNCATE removes all rows but keeps structure (DDL, faster, not rollback-able), DROP removes the entire table object including structure.

**Q2: Difference between WHERE and HAVING?**
WHERE filters rows before grouping and can't use aggregate functions; HAVING filters grouped results and can use aggregates.

**Q3: What is a Primary Key vs Foreign Key?**
Primary Key uniquely identifies each row in its own table (unique + not null); Foreign Key references a Primary Key in another table to establish a relationship.

**Q4: Difference between CHAR and VARCHAR2 (or VARCHAR in MySQL)?**
CHAR is fixed-length (pads with spaces, faster comparison); VARCHAR2/VARCHAR is variable-length (saves space, slightly slower).

**Q5: What are ACID properties?**
Atomicity, Consistency, Isolation, Durability — properties that guarantee reliable transaction processing.

**Q6: Difference between View and Materialized View?**
A View is virtual (no storage, always live), a Materialized View physically stores query results (needs manual/scheduled refresh, faster reads). Note: MySQL has no native materialized view support at all.

**Q7: What is normalization? Explain 1NF, 2NF, 3NF.**
Process of organizing data to reduce redundancy: 1NF requires atomic values, 2NF removes partial dependency on a composite key, 3NF removes transitive dependency between non-key columns.

**Q8: Difference between RANK, DENSE_RANK, and ROW_NUMBER?**
ROW_NUMBER always gives unique sequential numbers with no ties; RANK gives the same rank to ties but skips subsequent ranks; DENSE_RANK gives the same rank to ties with no gap.

**Q9: What is a correlated subquery?**
A subquery that references a column from the outer query, and therefore executes once per row processed by the outer query, unlike a regular subquery which executes once.

**Q10: What's the difference between UNION and UNION ALL?**
UNION combines result sets and removes duplicate rows (slower due to implicit sorting); UNION ALL combines and keeps all rows including duplicates (faster).

**Q11: How does Oracle's SEQUENCE differ from MySQL's AUTO_INCREMENT?**
A SEQUENCE is a standalone, reusable database object generating values via `.NEXTVAL`/`.CURRVAL`, independent of any single table; AUTO_INCREMENT is a column attribute tied to one specific table, incrementing automatically on each insert with no manual call needed.

**Q12: What happens if you PURGE a table that was never dropped (Oracle)?**
Oracle throws `ORA-38307: object not in RECYCLEBIN` — the table must be DROPped first (sending it to the recycle bin) before it can be PURGEd. This entire recycle bin concept doesn't exist in MySQL — DROP TABLE is immediate and permanent there.

**Q13: How does row-limiting/pagination differ between Oracle and MySQL?**
Oracle traditionally uses ROWNUM in a wrapped subquery (or `OFFSET...FETCH` in 12c+); MySQL uses the much simpler `LIMIT n OFFSET m` clause directly in the query with no subquery wrapping required.

**Q14: What is a trigger and when would you use one? Key syntax difference between Oracle and MySQL?**
A stored block of code that runs automatically on INSERT/UPDATE/DELETE — used for auditing, enforcing complex business rules, or auto-populating columns. Oracle references changed values as `:OLD.col`/`:NEW.col` (colon prefix); MySQL uses `OLD.col`/`NEW.col` (no colon) and requires every trigger to be row-level (no statement-level triggers, unlike Oracle).

**Q15: Why avoid over-indexing a table?**
Every index must be updated on every INSERT/UPDATE/DELETE, so too many indexes slow down writes significantly even though they speed up reads — a classic space/write-speed vs read-speed trade-off, true in both Oracle and MySQL.

[⬆ Back to top](#-table-of-contents)

---

## 29. LPAD/RPAD, Hierarchical Queries (LEVEL), Type Conversion & DBA Concepts

### A. LPAD and RPAD

Pad a string on the left/right up to a target length using a filler character (default filler is a space).

```sql
LPAD(str, total_length, 'pad_char')
RPAD(str, total_length, 'pad_char')
```

```sql
SELECT LPAD('45', 5, '0') FROM dual;    -- '00045'
SELECT RPAD('45', 5, '0') FROM dual;    -- '45000'
SELECT LPAD(ename, 10, '*') FROM emp;   -- right-aligns name, pads left with *
SELECT LPAD('', 5, '*') || ename FROM emp;  -- indent for a report

-- Common real use: zero-pad employee IDs to a fixed width
SELECT LPAD(empno, 6, '0') AS emp_code FROM emp;   -- 100 -> '000100'

-- Common real use: mask all but last 4 digits of a phone/account number
SELECT LPAD(SUBSTR(acct_no, -4), LENGTH(acct_no), '*') FROM accounts;
-- e.g. acct_no '1234567890' -> '******7890'
```

If the string is **already longer** than the target length, both functions **truncate** it to that length instead of padding.
```sql
SELECT LPAD('HELLO WORLD', 5) FROM dual;   -- 'HELLO' (truncated, not padded)
```

🐬 **MySQL:** `LPAD()` and `RPAD()` exist with **identical syntax and behavior** — no differences here, safe to use the same way in both.

**Quick Q&A:**
- **Q: LPAD vs RPAD — what's the difference?** LPAD pads/aligns on the left (adds filler before the string, right-justifying it); RPAD pads on the right (adds filler after, left-justifying it).
- **Q: What happens if the string is longer than the padded length?** Oracle and MySQL both truncate it down to that length — no error is thrown.
- **Q: Real-world use case for LPAD?** Zero-padding numeric IDs/invoice numbers to a fixed width, right-aligning columns in fixed-width report output, and masking sensitive data (e.g. showing only the last 4 digits of a card number).

---

### B. Hierarchical Queries — CONNECT BY, PRIOR, LEVEL, START WITH (Oracle)

Oracle's native way to query **tree-structured / recursive** data (org charts, category trees, bill-of-materials, folder structures) without needing a recursive CTE.

```sql
CREATE TABLE emp2 (
    empno   NUMBER PRIMARY KEY,
    ename   VARCHAR2(20),
    mgr     NUMBER
);
```

| empno | ename | mgr |
|---|---|---|
| 1 | KING | NULL |
| 2 | JONES | 1 |
| 3 | SCOTT | 2 |
| 4 | ADAMS | 3 |
| 5 | BLAKE | 1 |

**Basic hierarchical query — top-down (managers → subordinates):**
```sql
SELECT LEVEL, ename, mgr
FROM emp2
START WITH mgr IS NULL          -- root/top of the tree
CONNECT BY PRIOR empno = mgr    -- parent's empno = child's mgr
ORDER BY LEVEL;
```

| LEVEL | ename | mgr |
|---|---|---|
| 1 | KING | NULL |
| 2 | JONES | 1 |
| 2 | BLAKE | 1 |
| 3 | SCOTT | 2 |
| 4 | ADAMS | 3 |

- **`LEVEL`** — a pseudo-column that returns the depth of the current row in the tree (root = 1, its children = 2, and so on).
- **`PRIOR`** — indicates which side of the condition belongs to the **parent row** in the previous level. `PRIOR empno = mgr` means "parent's empno equals this row's mgr".
- **`START WITH`** — defines the root(s) of the tree (the condition identifying top-level rows).
- **`CONNECT BY`** — defines the parent-child relationship used to walk down the tree.

**Bottom-up traversal (subordinate → all managers above them):**
```sql
SELECT LEVEL, ename
FROM emp2
START WITH ename = 'ADAMS'
CONNECT BY PRIOR mgr = empno    -- reversed: walk UP instead of down
ORDER BY LEVEL;
```

**Indenting output to visualize the tree (classic report style):**
```sql
SELECT LPAD(' ', (LEVEL-1)*2, ' ') || ename AS org_chart
FROM emp2
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr;
```
```
KING
  JONES
    SCOTT
      ADAMS
  BLAKE
```
> This is the **LPAD + LEVEL combo** — a very commonly asked "print an org chart" interview question.

**Other hierarchical-query tools:**
| Function/Clause | Purpose |
|---|---|
| `CONNECT_BY_ROOT` | Returns the root ancestor's value for any row |
| `SYS_CONNECT_BY_PATH(col, 'sep')` | Builds the full path from root to current row |
| `CONNECT BY NOCYCLE` | Prevents infinite loops if the data has a cycle |
| `ORDER SIBLINGS BY col` | Sorts children within the same parent, without breaking the tree order (plain ORDER BY would break hierarchy order) |

```sql
SELECT ename, SYS_CONNECT_BY_PATH(ename, ' > ') AS path
FROM emp2
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr;
-- ADAMS -> KING > JONES > SCOTT > ADAMS
```

🐬 **MySQL equivalent:** None of `CONNECT BY`, `START WITH`, `PRIOR`, `LEVEL`, `SYS_CONNECT_BY_PATH` exist in MySQL — this entire syntax family is **Oracle-only**. MySQL (8.0+) uses a **recursive CTE** instead, and must build its own "level" counter manually:

```sql
WITH RECURSIVE org_chart AS (
    -- anchor: the root row(s)
    SELECT empno, ename, mgr, 1 AS lvl, CAST(ename AS CHAR(200)) AS path
    FROM emp2
    WHERE mgr IS NULL

    UNION ALL

    -- recursive step: join children to their parent's row in the CTE
    SELECT e.empno, e.ename, e.mgr, o.lvl + 1, CONCAT(o.path, ' > ', e.ename)
    FROM emp2 e
    JOIN org_chart o ON e.mgr = o.empno
)
SELECT lvl, LPAD('', (lvl-1)*2, ' ') || ename AS org_chart, path
FROM org_chart
ORDER BY lvl;
```

| Oracle | 🐬 MySQL |
|---|---|
| `START WITH ... CONNECT BY PRIOR` | `WITH RECURSIVE cte AS (anchor UNION ALL recursive-join)` |
| `LEVEL` (automatic) | Manual counter column (`lvl + 1` in the recursive branch) |
| `SYS_CONNECT_BY_PATH` | Manual string building with `CONCAT()` in the recursive branch |
| `CONNECT BY NOCYCLE` | Must add your own cycle-guard (e.g. a max-depth condition or a "visited" path check) |
| Available since Oracle 2 (very old feature) | Recursive CTEs only from MySQL **8.0+** — completely unavailable before that |

**Quick Q&A:**
- **Q: What does LEVEL represent in a hierarchical query?** The depth of the current row within the tree — the root row(s) from START WITH are LEVEL 1, their direct children are LEVEL 2, and so on.
- **Q: Difference between CONNECT BY PRIOR empno = mgr and CONNECT BY PRIOR mgr = empno?** The first walks **top-down** (parent → children); the second walks **bottom-up** (child → all its ancestors) — PRIOR marks which side is the "parent" row from the previous level.
- **Q: How do you avoid an infinite loop in a hierarchical query if the data has a cycle?** Use `CONNECT BY NOCYCLE`, which stops traversal when a cycle is detected instead of looping forever.
- **Q: How would you replicate an Oracle CONNECT BY query in MySQL?** With a recursive CTE (`WITH RECURSIVE`), available from MySQL 8.0 onward — manually tracking depth and path since MySQL has no built-in LEVEL or SYS_CONNECT_BY_PATH.

---

### C. Type Conversion — Full Picture (Number ↔ Date ↔ String)

Oracle has **three core conversion functions**, each with dozens of possible **format models**.

#### TO_CHAR — Number/Date → String

```sql
-- Number formatting
SELECT TO_CHAR(1234567.891, '9,999,999.99') FROM dual;   -- '1,234,567.89'
SELECT TO_CHAR(45, '000') FROM dual;                       -- '045'
SELECT TO_CHAR(1250, '$9,999') FROM dual;                  -- '$1,250'

-- Date formatting
SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY') FROM dual;    -- '22-AUG-2026'
SELECT TO_CHAR(SYSDATE, 'DD/MM/YYYY HH24:MI:SS') FROM dual;  -- '22/08/2026 14:35:10'
SELECT TO_CHAR(SYSDATE, 'DAY') FROM dual;            -- 'SATURDAY'
SELECT TO_CHAR(SYSDATE, 'Month YYYY') FROM dual;     -- 'August    2026'
```

| Format code | Meaning |
|---|---|
| `YYYY` / `YY` | 4-digit / 2-digit year |
| `MM` / `MON` / `MONTH` | Numeric / short / full month |
| `DD` / `DY` / `DAY` | Day of month / short weekday / full weekday |
| `HH24` / `HH` | 24-hour / 12-hour |
| `MI` / `SS` | Minutes / seconds |
| `9` | Digit, suppresses leading zeros |
| `0` | Digit, keeps leading zeros |
| `$` `,` `.` | Literal currency/thousands/decimal symbols |

#### TO_DATE — String → Date

```sql
SELECT TO_DATE('22-08-2026', 'DD-MM-YYYY') FROM dual;
SELECT TO_DATE('2026/08/22', 'YYYY/MM/DD') FROM dual;
SELECT TO_DATE('August 22, 2026', 'Month DD, YYYY') FROM dual;
```
> ⚠️ The **format mask must match the input string's actual layout** exactly, or Oracle throws `ORA-01858` (not a valid month) or `ORA-01861` (literal does not match format string).

#### TO_NUMBER — String → Number

```sql
SELECT TO_NUMBER('1,234.56', '9,999.99') FROM dual;   -- 1234.56
SELECT TO_NUMBER('$500', '$999') FROM dual;             -- 500
SELECT TO_NUMBER('123') FROM dual;                       -- 123 (format mask optional if plain digits)
```

#### CAST — ANSI-standard alternative

`CAST` is the SQL-standard conversion function, works across most databases including both Oracle and MySQL, but offers less formatting control than TO_CHAR/TO_DATE.

```sql
SELECT CAST('123' AS NUMBER) FROM dual;
SELECT CAST(sal AS VARCHAR2(20)) FROM emp;
SELECT CAST('2026-08-22' AS DATE) FROM dual;
```

#### Implicit vs Explicit Conversion

- **Implicit conversion** — Oracle/MySQL silently convert types when needed (e.g. comparing a NUMBER column to a string literal `WHERE empno = '101'`). Works, but is **risky**: it can silently return wrong results, prevent index usage (index becomes unusable when a function/conversion wraps the column), and mask data-quality bugs.
- **Explicit conversion** — using TO_CHAR/TO_DATE/TO_NUMBER/CAST yourself. **Always preferred** in production code and definitely in interviews, since it's predictable and keeps indexes usable.

```sql
-- Risky (implicit) — Oracle silently converts hiredate to a string to compare
SELECT * FROM emp WHERE hiredate = '22-AUG-26';

-- Safe (explicit) — you control exactly how the conversion happens
SELECT * FROM emp WHERE hiredate = TO_DATE('22-08-2026','DD-MM-YYYY');
```

🐬 **MySQL equivalents — full mapping:**

| Oracle | 🐬 MySQL |
|---|---|
| `TO_CHAR(date, 'fmt')` | `DATE_FORMAT(date, '%d-%b-%Y')` |
| `TO_CHAR(number, 'fmt')` | `FORMAT(number, decimals)` (limited) or manual `CONCAT` |
| `TO_DATE('str', 'fmt')` | `STR_TO_DATE('str', '%d-%m-%Y')` |
| `TO_NUMBER('str')` | `CAST(str AS SIGNED)` / `CAST(str AS DECIMAL(p,s))` or `str + 0` (quick informal cast) |
| `CAST(val AS type)` | `CAST(val AS type)` — same, but MySQL's target type list is narrower (`CHAR`, `DATE`, `DATETIME`, `DECIMAL`, `SIGNED`, `UNSIGNED`, `BINARY`, `TIME`, `JSON`) |
| Format codes: `YYYY MM DD HH24 MI SS` | Format codes: `%Y %m %d %H %i %s` (percent-prefixed, different letters) |

```sql
-- MySQL versions of the same conversions
SELECT DATE_FORMAT(NOW(), '%d-%b-%Y') ;             -- '22-Aug-2026'
SELECT STR_TO_DATE('22-08-2026', '%d-%m-%Y');
SELECT CAST('123' AS SIGNED);
SELECT '123' + 0;                                     -- quick implicit-to-explicit numeric trick, common in MySQL scripts
```

**Quick Q&A:**
- **Q: Difference between implicit and explicit conversion, and why does it matter for performance?** Implicit conversion happens automatically and can silently disable an index on the converted column (since the optimizer can't use the index once a function wraps it), and can produce wrong results if the assumed conversion isn't what you intended; explicit conversion is predictable, index-safe (when done correctly on the search value, not the column), and self-documenting.
- **Q: What error does Oracle throw for a malformed TO_DATE call?** `ORA-01858` if a numeric/text mismatch occurs (e.g. letters where digits expected) or `ORA-01861` if the literal string doesn't match the given format mask's length/pattern.
- **Q: How do you convert a string to a number in both Oracle and MySQL?** Oracle: `TO_NUMBER('123')`; MySQL: `CAST('123' AS SIGNED)` or `CAST('123' AS DECIMAL(10,2))` for decimals, or informally `'123' + 0`.
- **Q: Why should you avoid comparing a DATE column to a string literal directly?** It relies on implicit conversion whose exact format depends on session NLS/date settings (Oracle) or the SQL mode (MySQL) — it can behave differently across environments/servers, so always wrap the literal explicitly with TO_DATE/STR_TO_DATE instead.

---

### D. DBA-Level / Production Concepts

These come up in interviews once you're past pure query-writing and into "how would this run in production" territory.

#### 1) Tablespaces & Storage (Oracle)
Oracle organizes physical storage into **tablespaces** — logical containers mapped to physical datafiles on disk. Every table/index lives inside a tablespace.
```sql
CREATE TABLESPACE app_data
DATAFILE 'app_data01.dbf' SIZE 500M AUTOEXTEND ON;

CREATE TABLE emp (...) TABLESPACE app_data;
```
🐬 **MySQL:** InnoDB uses its own tablespace model (`innodb_file_per_table` — by default each table gets its own `.ibd` file), conceptually similar but far less manually managed day-to-day than Oracle's.

#### 2) Explain Plan / Query Execution Plan
Shows **how** the database will execute a query — which indexes it uses, join order, join method, estimated cost/rows. Essential for performance tuning.
```sql
-- Oracle
EXPLAIN PLAN FOR SELECT * FROM emp WHERE deptno = 10;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```
🐬 **MySQL:**
```sql
EXPLAIN SELECT * FROM emp WHERE deptno = 10;
-- or, for actual runtime stats:
EXPLAIN ANALYZE SELECT * FROM emp WHERE deptno = 10;   -- 8.0.18+
```

#### 3) Redo Logs / Undo (Oracle) vs Binary Log / Undo (MySQL InnoDB)
| Concept | Oracle | 🐬 MySQL (InnoDB) |
|---|---|---|
| Records changes for crash recovery / replay | Redo Log | Redo Log (InnoDB has its own, similar concept) |
| Lets you ROLLBACK an uncommitted transaction | Undo Segments (in the SYSTEM/UNDO tablespace) | Undo Log (InnoDB) |
| Used for replication / point-in-time recovery | Archive Log (archived redo) | Binary Log (`binlog`) |

#### 4) Locking & Concurrency
- **Row-level locking** — locks only the specific row(s) being modified (default in Oracle, and in MySQL's InnoDB engine). MyISAM only supports **table-level locking**, which is a major reason InnoDB is preferred for concurrent write-heavy workloads.
- **Deadlock** — two transactions each waiting on a lock the other holds. Both Oracle and MySQL automatically detect deadlocks and roll back one of the transactions with an error, rather than waiting forever.
```sql
-- Check current locks (Oracle)
SELECT * FROM v$lock;

-- Check current locks (MySQL, InnoDB)
SELECT * FROM information_schema.innodb_trx;
SHOW ENGINE INNODB STATUS;
```

#### 5) Backup & Recovery
| Concept | Oracle | 🐬 MySQL |
|---|---|---|
| Full logical backup tool | `expdp` / `impdp` (Data Pump) or legacy `exp`/`imp` | `mysqldump` or `mysqlpump` |
| Physical/hot backup tool | RMAN (Recovery Manager) | `mysqlbackup` / `xtrabackup` (Percona) |
| Point-in-time recovery | Uses archived redo logs | Uses binary logs |

#### 6) Partitioning
Splits a large table into smaller physical pieces (partitions) for performance and manageability, while still queryable as one logical table.
```sql
-- Oracle: range partition by year
CREATE TABLE sales (
    sale_id NUMBER,
    sale_date DATE,
    amount NUMBER
)
PARTITION BY RANGE (sale_date) (
    PARTITION p2024 VALUES LESS THAN (TO_DATE('01-JAN-2025','DD-MON-YYYY')),
    PARTITION p2025 VALUES LESS THAN (TO_DATE('01-JAN-2026','DD-MON-YYYY')),
    PARTITION p2026 VALUES LESS THAN (MAXVALUE)
);
```
🐬 **MySQL:** Also supports `PARTITION BY RANGE`, `LIST`, `HASH`, `KEY` with broadly similar syntax — but with more restrictions (e.g. the partitioning column generally must be part of every unique key, including the primary key).

#### 7) User/Role Management & Security
```sql
-- Oracle: roles group privileges for easier management
CREATE ROLE app_readonly;
GRANT SELECT ON emp TO app_readonly;
GRANT app_readonly TO ramesh;
```
🐬 **MySQL (roles only from 8.0+):**
```sql
CREATE ROLE 'app_readonly';
GRANT SELECT ON db1.emp TO 'app_readonly';
GRANT 'app_readonly' TO 'ramesh'@'localhost';
SET DEFAULT ROLE 'app_readonly' TO 'ramesh'@'localhost';
```
> MySQL roles were only introduced in **8.0** — earlier versions had no role concept at all, privileges had to be granted individually to every user.

#### 8) Data Dictionary / System Catalog Views
Every RDBMS exposes metadata about its own objects through system views.
| Purpose | Oracle | 🐬 MySQL |
|---|---|---|
| List all tables (current user) | `SELECT * FROM user_tables;` | `SHOW TABLES;` or `SELECT * FROM information_schema.tables;` |
| List all columns of a table | `SELECT * FROM user_tab_columns WHERE table_name='EMP';` | `SHOW COLUMNS FROM emp;` or `information_schema.columns` |
| List constraints | `user_constraints` | `information_schema.table_constraints` |
| List indexes | `user_indexes` | `SHOW INDEX FROM emp;` |
| Currently running sessions | `v$session` | `SHOW PROCESSLIST;` or `information_schema.processlist` |

**Quick Q&A:**
- **Q: What's the difference between a redo log and an undo log (Oracle)?** Redo logs record changes for crash recovery/replaying committed work; undo logs (segments) store the "before" image of data so an uncommitted transaction can be rolled back, and also power read consistency for other sessions.
- **Q: Why is InnoDB preferred over MyISAM for transactional MySQL applications?** InnoDB supports row-level locking, transactions (COMMIT/ROLLBACK), and foreign keys, all of which MyISAM lacks (MyISAM only does table-level locking and has no transaction support at all).
- **Q: What does EXPLAIN / EXPLAIN PLAN show you, and why does it matter?** It shows the execution plan the optimizer intends to use — join order, join method, which indexes (if any) are used, and estimated cost/row counts — which is the primary tool for diagnosing a slow query.
- **Q: What's the purpose of table partitioning?** It splits a very large table into smaller physical segments (by range, list, or hash) so queries that target a subset of the data (e.g. one date range) can skip scanning irrelevant partitions entirely, and so maintenance operations (like archiving old data) can act on just one partition.
- **Q: How would you find which queries are currently running/blocking on the server?** Oracle: query `v$session` and `v$lock`; MySQL: `SHOW PROCESSLIST;` or query `information_schema.innodb_trx` / `SHOW ENGINE INNODB STATUS;` for InnoDB-specific lock/transaction detail.

[⬆ Back to top](#-table-of-contents)
