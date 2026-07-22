# 📘 Oracle SQL & DBMS – Complete Notes

A complete, structured reference covering DBMS basics, Data Types, Constraints, DDL, DML, DCL, TCL, Functions, Joins, Operators, Subqueries, and Query Clauses.

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
12. [Subqueries & ROWNUM](#12-subqueries--rownum)
13. [WHERE, GROUP BY, HAVING & ORDER BY](#13-where-group-by-having--order-by)
14. [TCL – Transaction Control Language](#14-tcl--transaction-control-language)
15. [DCL – Data Control Language](#15-dcl--data-control-language)
16. [Copying Tables (CTAS) & Creating New Users](#16-copying-tables-ctas--creating-new-users)
17. [SQL\*Plus Commands](#17-sqlplus-commands)
18. [🔑 Interview Quick-Fire Points](#-interview-quick-fire-points)
19. [💼 Most Asked Interview Questions](#-most-asked-interview-questions)

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
- Normalization

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

### DATE
Stores date values. Oracle default format: `DD-MON-YY` (e.g. `29-JUN-26`) or `DD-MON-YYYY` (e.g. `29-JUN-2026`).
```sql
hiredate DATE
```

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

### Summary of Data Types

| Data Type | Used For |
|---|---|
| CHAR | Fixed-length characters |
| VARCHAR | Variable-length characters |
| VARCHAR2 | Oracle variable-length characters |
| NUMBER | Numbers |
| DATE | Date values |
| CLOB | Large text |
| BLOB | Images, videos, files |

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

### 3) ALTER
`ALTER` is a DDL command used to alter (modify) the structure of an existing table.

| Operation | Syntax |
|---|---|
| Add column | `ALTER TABLE tabn ADD coln datatype constraint;` |
| Rename column | `ALTER TABLE tabn RENAME COLUMN oldcoln TO newcoln;` |
| Drop column | `ALTER TABLE tabn DROP COLUMN columnname;` |
| Modify column | `ALTER TABLE tabname MODIFY colname datatype constraint;` |

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

### 5) DROP
`DROP` is a DDL command used to **delete a table**. The table gets moved to the **recycle bin** (it isn't gone forever — yet).
```sql
DROP TABLE tabname;
```
```sql
DROP TABLE student;
-- Table dropped.
```

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

| Command | Purpose | Structure affected? | Recoverable? |
|---|---|---|---|
| `CREATE` | Create new object | — | — |
| `RENAME` | Rename table | No | — |
| `ALTER` | Add/Modify/Drop/Rename column | Yes | — |
| `TRUNCATE` | Delete all rows, keep structure | No | ❌ Not recoverable |
| `DROP` | Delete table (→ recycle bin) | Yes | ✅ via FLASHBACK |
| `FLASHBACK` | Restore dropped table | — | — |
| `PURGE` | Permanently remove from recycle bin | Yes | ❌ Not recoverable |

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

**Character Manipulation**

`SUBSTR('str', pos, len)` – extracts characters from a string.
- `SUBSTR('Manu',1,3)` → `Man`
- First char: `SUBSTR(ename,1,1)` &nbsp;|&nbsp; Last letter: `SUBSTR(ename,LENGTH(ename),1)`
- First half: `SUBSTR(ename,1,FLOOR(LENGTH(ename)/2))`
- Second half: `SUBSTR(ename,CEIL(LENGTH(ename)/2))`

`CONCAT('arg1','arg2')` – joins two strings.
- `SELECT CONCAT('Hi:',empname) FROM emp;`

`REVERSE('arg')` – reverses a string.
- `SELECT REVERSE('FORD') FROM dual;` → `DORF`

`INSTR('str','char',pos,nth_occ)` – finds position/index of a character.
- `SELECT INSTR('NAYAN','A',1,1) FROM dual;` → 2
- `SELECT INSTR('NAYAN','A',1,2) FROM dual;` → 4
- Count occurrences of 'P': `LENGTH('Pushpa') - LENGTH(REPLACE('Pushpa','P',''))`
- Domain from email: `SUBSTR(email, INSTR(email,'@')+1)`

`TRIM(LEADING/TRAILING/BOTH 'char' FROM 'str')` – removes leading/trailing/both characters.
- `TRIM(LEADING 'P' FROM 'PUSHPA')` → `USHPA`
- `TRIM(TRAILING 'A' FROM 'PUSHPA')` → `PUSHP`

`REPLACE('str','substr','newstr')` – replaces **all occurrences**.
- `REPLACE('BANGLORE','B','M')` → `MANGLORE`

### B. Number Single Row Function

| Function | Purpose | Example | Result |
|---|---|---|---|
| `MOD(m,n)` | Remainder | `MOD(11,2)` | 2 |
| `SQRT(arg)` | Square root | `SQRT(100)` | 10 |
| `POWER(m,n)` | Exponent | `POWER(2,5)` | 32 |
| `ABS(arg)` | Positive value | `ABS(-10)` | 10 |
| `ROUND(num,scale)` | Round to nearest value | see below | — |

**ROUND rule:** decimal 0–4 → rounds down, decimal 5–9 → rounds up.
- `ROUND(37.7494,1)` → 37.7
- `ROUND(789.12,-2)` → 800 *(negative scale rounds left of decimal — tens/hundreds/thousands)*

### C. Date Single Row Function

| Function | Purpose | Example | Result |
|---|---|---|---|
| `ADD_MONTHS('date',n)` | Adds n months | `ADD_MONTHS('07-JUL-2026',12)` | 07-JUL-2027 |
| `MONTHS_BETWEEN('d1','d2')` | Diff in months | `MONTHS_BETWEEN('07-JUL-2027','07-JUL-2026')` | 12 |
| `EXTRACT(YEAR/MONTH/DAY FROM col)` | Extracts date part | `EXTRACT(MONTH FROM hiredate)` | month number |
| `LAST_DAY('date')` | Last day of month | `LAST_DAY('12-FEB-2020')` | 29-FEB-2020 |
| `SYSDATE` | Current system date | `SELECT SYSDATE FROM dual;` | — |

### D. General Single Row Function
Used to handle `NULL` values.

- `NVL(arg1, arg2)` → if arg1 is null, returns arg2; else returns arg1.
- `NVL2(arg1, arg2, arg3)` → if arg1 is null, returns arg3; else returns arg2.

### Multi Row / Group / Aggregate Function

| Function | Purpose |
|---|---|
| `MAX(arg)` | Returns max value |
| `MIN(arg)` | Returns min value |
| `AVG(arg)` | Returns average value |
| `SUM(arg)` | Returns sum of values |
| `COUNT(*/col)` | Returns count of rows |

```sql
SELECT MAX(sal) FROM emp;
SELECT SUM(sal) FROM emp WHERE job='SALESMAN';
```

⚠️ We **cannot** use a normal column with an aggregate function without `GROUP BY`.
```sql
-- Needs GROUP BY
SELECT ename, SUM(sal) FROM emp GROUP BY ename;
```

[⬆ Back to top](#-table-of-contents)

---

## 8. SQL Joins

A **JOIN** combines data from two or more tables based on a common column/relationship. Since normalization stores related data across different tables, joins are used to retrieve meaningful combined information.

**Example — why we need joins:**

EMP table + DEPT table → combine to get `ENAME, DNAME, LOCATION` together.

### Types of Joins
1. Cross Join (Cartesian Join)
2. Inner Join (Equi Join)
3. Non-Equi Join
4. Natural Join
5. Self Join
6. Left Outer Join
7. Right Outer Join
8. Full Outer Join

### 1) Cross Join (Cartesian Join)
Returns every row of table A combined with every row of table B. **No join condition.**
```text
Rows(A) × Rows(B)  →  e.g. EMP(14) × DEPT(4) = 56 rows
```
```sql
-- ANSI
SELECT * FROM emp CROSS JOIN dept;
-- Oracle old (no WHERE)
SELECT * FROM emp, dept;
```
Used rarely — mostly for generating combinations.

### 2) Inner Join (Equi Join)
Returns only rows that satisfy the join condition — **matched records only**.
```sql
-- ANSI
SELECT e.ename, d.dname FROM emp e INNER JOIN dept d ON e.deptno=d.deptno;
-- Oracle old
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno=d.deptno;
```
- To apply an Inner Join, a relationship must exist (**direct** or **indirect** via intermediate tables).
- For indirect/chained joins (e.g. `employees → departments → locations → countries → regions`), you must join through every table in the chain even if you don't display all its columns.
- Works with **aggregate functions** too (`COUNT`, `SUM`, etc.) — remember `GROUP BY` rules apply.

### 3) Non-Equi Join
Uses operators **other than `=`**: `BETWEEN`, `>`, `<`, `>=`, `<=`.
```sql
-- ANSI
SELECT e.ename, e.sal, s.grade FROM emp e JOIN salgrade s ON e.sal BETWEEN s.losal AND s.hisal;
```
Used whenever the join condition is a **range**, not an exact match (e.g. matching salary against `SALGRADE` ranges).

### 4) Natural Join
Automatically joins two tables based on columns having the **same name** — no join condition written.
```sql
SELECT ename, dname FROM emp NATURAL JOIN dept;
```
- Available **only in ANSI syntax** (no Oracle old-style equivalent).
- If tables share a real relationship column → behaves like **INNER JOIN**.
- If tables have no common column → behaves like **CROSS JOIN** (n × m).
- ⚠️ Risky if multiple columns unintentionally share the same name — can cause wrong/unexpected joins. Used less often in practice for this reason.

### 5) Self Join
A table joined with **itself**, using aliases — commonly used for employee–manager relationships (or any hierarchical data). See a full worked example in [Section 10](#10-self-join--family-tree-example).
```sql
-- ANSI
SELECT e.ename Employee, m.ename Manager FROM emp e JOIN emp m ON e.mgr=m.empno;
-- Oracle old
SELECT e.ename Employee, m.ename Manager FROM emp e, emp m WHERE e.mgr=m.empno;
```
- Can be extended to multiple levels — e.g. employee → manager → manager's manager (3 self-joins).
- Also useful to find **duplicate values** (e.g. employees sharing the same salary):
```sql
SELECT DISTINCT e.sal FROM emp e, emp e1 WHERE e.sal = e1.sal AND e.empno != e1.empno;
```

### 6), 7), 8) Outer Joins — Introduction

There are **three types of Outer Joins**:
- **Left Outer Join**
- **Right Outer Join**
- **Full Outer Join**

An **Outer Join** is a type of SQL join that returns the **matched records** as well as the **unmatched records with NULL values**.

**Left Outer Join** → returns matched records from the **left table & right table**, plus unmatched records from the **left table** with NULL values.
```sql
-- ANSI
SELECT * FROM tab1 LEFT OUTER JOIN tab2 ON cond;
-- Oracle old
SELECT * FROM tab1, tab2 WHERE tab1.col = tab2.col(+);
```

**Right Outer Join** → returns matched records from the **right table & left table**, plus unmatched records from the **right table** with NULL values.
```sql
-- ANSI
SELECT * FROM tab1 RIGHT OUTER JOIN tab2 ON cond;
-- Oracle old
SELECT * FROM tab1, tab2 WHERE tab1.col(+) = tab2.col;
```

**Full Outer Join** → returns matched records from **both tables**, plus unmatched records from **both** the left table and the right table with NULL values.
```sql
-- ANSI
SELECT * FROM tab1 FULL JOIN tab2 ON cond;
-- Oracle old syntax: not supported — use LEFT JOIN UNION RIGHT JOIN instead
```

> 🧠 **Memory trick:** The `(+)` symbol goes on the side that is *missing/deficient* — i.e. it marks the table that gets NULL-padded for unmatched rows.

Full detailed examples with sample data are worked through in [Section 9](#9-outer-joins--practical-walkthrough).

### ANSI vs Oracle Old Syntax

| ANSI SQL | Oracle Old Syntax |
|---|---|
| Uses `JOIN` keyword | Uses commas |
| Uses `ON` clause | Uses `WHERE` clause |
| Easier to read | Older style |
| Recommended | Legacy code |

### Join Summary Table

| Join Type | Matching Records | Non-Matching Records |
|---|---|---|
| Cross Join | No condition | Every combination |
| Inner Join | Yes | No |
| Non-Equi Join | Range/comparison | Depends on condition |
| Natural Join | Automatic matching | No |
| Self Join | Same table | Depends on condition |
| Left Outer Join | Yes | All left rows |
| Right Outer Join | Yes | All right rows |
| Full Outer Join | Yes | All rows from both tables |

[⬆ Back to top](#-table-of-contents)

---

## 9. Outer Joins – Practical Walkthrough

Sample data used throughout this section:

**Student table**

| sid | name | subid |
|---|---|---|
| 1 | indu | s3 |
| 2 | bindu | — |
| 3 | dindu | s1 |
| 4 | chondu | s3 |
| 5 | nendu | — |

**Subject table**

| subid | sname |
|---|---|
| s1 | Java |
| s2 | sql |
| s3 | web |
| s4 | Python |

### Left Outer Join

Keeps **every student**, even one not registered for any subject.
```sql
-- ANSI
SELECT name, sname FROM student s LEFT OUTER JOIN subject sb ON s.subid = sb.subid;

-- Oracle old
SELECT name, sname FROM student s, subject sb WHERE s.subid = sb.subid(+);
```

| name | sname |
|---|---|
| indu | web |
| bindu | — |
| dindu | Java |
| chondu | web |
| nendu | — |

### Right Outer Join

Keeps **every subject**, even one that has no students registered for it.
```sql
-- ANSI
SELECT name, sname FROM student s RIGHT OUTER JOIN subject sb ON s.subid = sb.subid;

-- Oracle old
SELECT name, sname FROM student s, subject sb WHERE s.subid(+) = sb.subid;
```

| name | sname |
|---|---|
| indu | web |
| dindu | Java |
| chondu | web |
| — | sql |
| — | Python |

### Full Outer Join

Keeps **every student and every subject**, matched or not.
```sql
-- ANSI
SELECT name, sname FROM student s FULL OUTER JOIN subject sb ON s.subid = sb.subid;
```

| name | sname |
|---|---|
| indu | web |
| dindu | Java |
| chondu | web |
| — | sql |
| — | Python |
| bindu | — |
| nendu | — |

### Practice Questions (with NULL filtering)

```sql
-- Q1: Find student name, subject name, considering the student
--     even if they are not registered to any subject.
SELECT s.name, sb.sname FROM student s LEFT JOIN subject sb ON s.subid = sb.subid;

-- Q2: Find name, subject name, considering the subject
--     even if no student is registered for it.
SELECT s.name, sb.sname FROM student s RIGHT JOIN subject sb ON s.subid = sb.subid;

-- Q3: Find student name when the student is NOT registered to any subject.
SELECT s.name FROM student s LEFT JOIN subject sb ON s.subid = sb.subid
WHERE s.subid IS NULL;

-- Q4: Find subject name when NO student is registered to it.
SELECT sb.sname FROM student s RIGHT JOIN subject sb ON s.subid = sb.subid
WHERE s.subid IS NULL;
```

> 💡 **Pattern:** `LEFT/RIGHT JOIN + WHERE <joined-key> IS NULL` is the classic trick to find **unmatched / orphan records** (students with no subject, subjects with no students, etc).

[⬆ Back to top](#-table-of-contents)

---

## 10. Self Join – Family Tree Example

A **Self Join** joins a table to itself — perfect for hierarchical / recursive relationships such as a family tree.

**Family table**

| Son | Father |
|---|---|
| Kalia | Raju |
| Jaggu | dolu |
| bheem | Chutki |
| Chutki | Kalia |
| Rahul | bheem |
| dolu | abhi |

### Q1: Find the Grandfather

A grandfather is the **father of the father**. Self-join the table once, matching `son.father = father.son`.

```sql
SELECT s.son, gf.father AS grandfather
FROM family s
LEFT JOIN family gf ON s.father = gf.son;
```

| Grand-Son | Grandfather |
|---|---|
| Kalia | — |
| Jaggu | abhi |
| bheem | Kalia |
| Chutki | Raju |
| Rahul | Chutki |
| dolu | — |

*(LEFT JOIN is used so grandsons whose grandfather isn't in the table still appear, with NULL.)*

### Q2: Find the Great-Grandfather

A great-grandfather is the **father of the grandfather**. Self-join the table **twice**.

```sql
SELECT s.son, ggf.father AS great_grandfather
FROM family s
LEFT JOIN family gf  ON s.father  = gf.son
LEFT JOIN family ggf ON gf.father = ggf.son;
```

| Grand-Son | Great-Grandfather |
|---|---|
| bheem | Raju |
| Rahul | Kalia |

> 💡 **Pattern:** each extra "generation" you need to trace back requires **one more self-join**. This is a very common interview question testing self-joins + multi-level relationships.

[⬆ Back to top](#-table-of-contents)

---

## 11. SQL Operators

An **Operator** is a symbol/keyword used to perform an operation on one or more operands.

### Types of Operators
1. Arithmetic
2. Logical
3. Relational
4. Concatenation
5. Set
6. Special
7. Subquery

### 1) Arithmetic Operator: `+ - * /`
```sql
SELECT ename, sal*12 AS annual_salary FROM emp;
```

### 2) Logical Operator: `AND`, `OR`, `NOT`
```sql
SELECT * FROM emp WHERE deptno=30 AND sal>1500;
SELECT * FROM emp WHERE deptno=10 OR deptno=20;
```

### 3) Relational Operator: `= > < >= <= <> !=`
Always returns TRUE or FALSE.
```sql
SELECT * FROM emp WHERE sal>2000;
```

### 4) Concatenation Operator: `||`
```sql
SELECT ename||' works as '||job FROM emp;
```

### 5) Set Operators
Combine results of two or more `SELECT` statements. Both queries need the **same number of columns, same datatypes, same order**.

| Operator | Purpose |
|---|---|
| `UNION` | Unique rows from both queries |
| `UNION ALL` | All rows, including duplicates |
| `INTERSECT` | Only common rows |
| `MINUS` | Rows in query 1 not present in query 2 |

```sql
SELECT deptno FROM emp
UNION
SELECT deptno FROM dept;
```

### 6) Special Operators

| Operator | Purpose | Example |
|---|---|---|
| `IN` | Match any value in a list | `WHERE deptno IN(10,20,30)` |
| `NOT IN` | Exclude values in a list | `WHERE deptno NOT IN(10,20)` |
| `LIKE` | Pattern matching (`%` any chars, `_` one char) | `WHERE ename LIKE 'S%'` |
| `NOT LIKE` | Exclude matching pattern | `WHERE ename NOT LIKE 'S%'` |
| `BETWEEN` | Range (inclusive) | `WHERE sal BETWEEN 1000 AND 3000` |
| `NOT BETWEEN` | Outside range | `WHERE sal NOT BETWEEN 1000 AND 3000` |
| `IS NULL` | Check for NULL | `WHERE comm IS NULL` |
| `IS NOT NULL` | Check for non-NULL | `WHERE comm IS NOT NULL` |

**LIKE wildcard examples:**
- Starts with S: `'S%'` &nbsp;|&nbsp; Ends with H: `'%H'` &nbsp;|&nbsp; Contains A: `'%A%'`
- Exactly 5 characters: `'_____'` &nbsp;|&nbsp; Starts M ends R: `'M%R'`

### 7) Subquery Operators

| Operator | Meaning |
|---|---|
| `IN` | Matches any value returned by subquery |
| `ANY` | TRUE if condition satisfied by **at least one** value |
| `ALL` | TRUE only if condition satisfied by **every** value |
| `EXISTS` | TRUE if subquery returns **at least one row** |
| `NOT EXISTS` | TRUE if subquery returns **no rows** |

```sql
SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=30);
SELECT * FROM dept d WHERE EXISTS (SELECT * FROM emp e WHERE e.deptno=d.deptno);
```

### Operator Summary

| Operator Type | Operators |
|---|---|
| Arithmetic | `+ - * /` |
| Logical | `AND OR NOT` |
| Relational | `= > < >= <= <> !=` |
| Concatenation | `\|\|` |
| Set | `UNION UNION ALL INTERSECT MINUS` |
| Special | `IN NOT IN LIKE NOT LIKE BETWEEN NOT BETWEEN IS NULL IS NOT NULL` |
| Subquery | `IN ANY ALL EXISTS NOT EXISTS` |

[⬆ Back to top](#-table-of-contents)

---

## 12. Subqueries & ROWNUM

### Subquery
A query written **inside another SQL query** — also called an Inner Query or Nested Query. The inner query executes first; its result feeds the outer query.

**Used when:**
- Required value isn't known in advance.
- Data depends on another query's result.
- Comparing data against values from another query.
- Solving complex problems step by step.

```sql
SELECT * FROM emp WHERE sal = (SELECT MAX(sal) FROM emp);
```

### Types of Subqueries

**1) Single Row Subquery** — returns only **one value**. Uses `= > < >= <= <>`.
```sql
SELECT * FROM emp WHERE sal = (SELECT MAX(sal) FROM emp);
SELECT * FROM emp WHERE deptno = (SELECT deptno FROM dept WHERE dname='SALES');
```

**2) Multi Row Subquery** — returns **more than one row**. Uses `IN ANY ALL EXISTS`.
```sql
SELECT * FROM emp WHERE deptno IN (SELECT deptno FROM dept);
SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=30);
SELECT * FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno=30);
```

**3) Nested Subquery** — a subquery **inside another subquery** (2+ levels). Innermost query executes first.
```sql
SELECT * FROM emp WHERE deptno = (
    SELECT deptno FROM dept WHERE loc = (
        SELECT loc FROM dept WHERE dname='ACCOUNTING'
    )
);
```

**4) Inline Subquery (Inline View)** — a subquery written in the **FROM** clause; behaves like a temporary table. Commonly used with `ROWNUM` for Top-N, Bottom-N, Nth salary, pagination, and ranking.
```sql
-- Third record
SELECT * FROM (SELECT emp.*, ROWNUM r FROM emp) WHERE r=3;

-- Top 5 highest salaries
SELECT * FROM (SELECT * FROM emp ORDER BY sal DESC) WHERE ROWNUM<=5;

-- Second highest salary
SELECT * FROM (
    SELECT e.*, ROWNUM r FROM (SELECT * FROM emp ORDER BY sal DESC) e
) WHERE r=2;
```

**5) Correlated Subquery** — depends on the outer query; executed **once per row** of the outer query.
```sql
-- Employees earning more than their department average
SELECT * FROM emp e WHERE sal > (SELECT AVG(sal) FROM emp WHERE deptno=e.deptno);

-- Highest paid employee in each department
SELECT * FROM emp e WHERE sal = (SELECT MAX(sal) FROM emp WHERE deptno=e.deptno);
```

### ROWNUM
A **Pseudo Column** in Oracle — not stored in the table. Oracle auto-assigns a row number to every returned row, starting from **1**.

**Execution order:** `FROM → WHERE → ROWNUM assigned → SELECT → ORDER BY`
> ⚠️ `ROWNUM` is assigned **before** `ORDER BY` — this is why Inline Views are needed to sort first, then apply `ROWNUM`.

**Rules:**
| Works ✅ | Does NOT work ❌ |
|---|---|
| `WHERE rownum=1` | `WHERE rownum=2` |
| `WHERE rownum<=5` | `WHERE rownum>5` |
| `WHERE rownum<10` | — |

```sql
SELECT * FROM emp WHERE rownum=1;      -- first employee
SELECT * FROM emp WHERE rownum<=5;     -- first 5 employees
SELECT * FROM emp WHERE rownum=2;      -- ❌ No rows selected
```

[⬆ Back to top](#-table-of-contents)

---

## 13. WHERE, GROUP BY, HAVING & ORDER BY

### SQL Query Execution Order
```text
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```

### 1) WHERE Clause
Filters rows **before grouping** — only rows satisfying the condition are returned.
```sql
SELECT * FROM emp WHERE deptno=10;
SELECT * FROM emp WHERE hiredate>'01-JAN-82';
SELECT * FROM emp WHERE comm IS NULL;
```

### 2) GROUP BY Clause
Groups rows sharing the same value in one/more columns — used with aggregate functions (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`).
```sql
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
SELECT deptno, AVG(sal) FROM emp GROUP BY deptno;
```
**Rules:**
- Executed **after** WHERE, **before** HAVING.
- Every non-aggregate column in `SELECT` must be in `GROUP BY`.
- Aggregate functions cannot be used directly inside `GROUP BY`.

### 3) HAVING Clause
Filters **groups** created by `GROUP BY` (WHERE filters rows, HAVING filters groups).

| WHERE | HAVING |
|---|---|
| Filters rows | Filters groups |
| Executed before GROUP BY | Executed after GROUP BY |
| Cannot use aggregate functions | Aggregate functions commonly used |

```sql
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*)>3;
SELECT deptno, AVG(sal) FROM emp GROUP BY deptno HAVING AVG(sal)>2000;

-- WHERE + GROUP BY + HAVING together
SELECT deptno, COUNT(*) FROM emp
WHERE job='MANAGER'
GROUP BY deptno
HAVING COUNT(*)>1;
```

### 4) ORDER BY Clause
Sorts the result set — **Ascending (`ASC`, default)** or **Descending (`DESC`)**.
```sql
SELECT * FROM emp ORDER BY ename;
SELECT * FROM emp ORDER BY sal DESC;
SELECT * FROM emp ORDER BY deptno ASC, sal DESC;   -- multi-column sort
```
**Rules:**
- Always the **last** clause in a `SELECT` statement.
- `ASC` is default.
- Multiple columns can each have their own sort direction.

### Clause Summary

| Clause | Purpose |
|---|---|
| `WHERE` | Filter rows |
| `GROUP BY` | Group similar rows |
| `HAVING` | Filter grouped rows |
| `ORDER BY` | Sort final output |

[⬆ Back to top](#-table-of-contents)

---

## 14. TCL – Transaction Control Language

**TCL** is a type of SQL language used to **control the transactions** done on the database.

TCL consists of: **`COMMIT`**, **`ROLLBACK`**, **`SAVEPOINT`**

### 1) COMMIT
`COMMIT` is a TCL command used to **save data changes permanently**.
```sql
COMMIT;
```

### 2) ROLLBACK
`ROLLBACK` is a TCL command used to **undo transactions** back to the last `COMMIT` or a specific `SAVEPOINT` address.
```sql
ROLLBACK;                 -- undo everything since last COMMIT
ROLLBACK TO savepointname; -- undo everything since that SAVEPOINT
```

### 3) SAVEPOINT
`SAVEPOINT` is a **temporary memory address** used to mark a point in a transaction — you can `ROLLBACK` to that exact point later.
```sql
SAVEPOINT savepointname;
```

### Example 1 — Basic Commit & Rollback

```sql
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(10));

INSERT INTO stud VALUES (101, 'ramesh');
INSERT INTO stud VALUES (102, 'ramya');
COMMIT;
-- Commit complete.

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |

```sql
INSERT INTO stud VALUES (103, 'mahesh');
INSERT INTO stud VALUES (104, 'manisha');

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |

```sql
ROLLBACK;
-- Rollback complete.

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |

> The two uncommitted inserts (103, 104) disappeared because `ROLLBACK` undid everything since the last `COMMIT`.

### Example 2 — Multiple Savepoints

```sql
INSERT INTO stud VALUES (103, 'mahesh');
SAVEPOINT s1;
-- Savepoint created.

INSERT INTO stud VALUES (104, 'manisha');
SAVEPOINT s2;

INSERT INTO stud VALUES (105, 'suresh');
SAVEPOINT s3;

INSERT INTO stud VALUES (106, 'sunitha');
SAVEPOINT s4;

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |
| 105 | suresh |
| 106 | sunitha |

```sql
-- Roll back everything after savepoint s3 (undoes s4's insert only)
ROLLBACK TO s3;

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |
| 105 | suresh |

```sql
-- Roll back everything after savepoint s2 (undoes s3's insert too)
ROLLBACK TO s2;

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |

> 💡 `ROLLBACK TO <savepoint>` undoes everything done **after** that savepoint, but keeps everything done **up to and including** it.

### TCL Command Summary

| Command | Purpose |
|---|---|
| `COMMIT` | Permanently save all changes |
| `ROLLBACK` | Undo changes since last COMMIT/SAVEPOINT |
| `SAVEPOINT` | Mark a point to roll back to later |

[⬆ Back to top](#-table-of-contents)

---

## 15. DCL – Data Control Language

**DCL** is a type of SQL language used to **control the flow of data** or **grant/manage permissions**.

DCL has two commands: **`GRANT`** and **`REVOKE`**

### 1) GRANT
Used to **give permission** to a user.
```sql
GRANT sql-statement ON tabname TO username;
```
```sql
GRANT SELECT ON emp TO hr;         -- read-only permission
GRANT UPDATE ON emp TO hr;         -- update permission
GRANT ALL ON emp TO hr;            -- all permissions
```

### 2) REVOKE
Used to **take back** a permission that was granted.
```sql
REVOKE sql-statement ON tabname FROM username;
```

### Practical Walkthrough

```sql
-- Logged in as SCOTT
SHOW USER;
-- USER is "SCOTT"

-- Grant SELECT permission on emp to hr
GRANT SELECT ON emp TO hr;
-- Grant succeeded.

-- Connect as hr
CONN hr/tiger
-- Connected.

-- hr can't query "emp" directly — must qualify with owner
SELECT * FROM emp;
```
```text
ORA-00942: table or view does not exist
```
```sql
-- Must reference the owning schema
SELECT * FROM scott.emp;
-- 14 rows selected
```
```sql
-- hr only has SELECT, not UPDATE — this fails
UPDATE scott.emp SET ename = 'KING' WHERE empno = 7839;
```
```text
-- No UPDATE permission granted yet
```
```sql
-- Back on SCOTT's session, grant UPDATE too
CONN scott/tiger
GRANT ALL ON emp TO hr;
-- Grant succeeded.

-- Back on hr's session
CONN hr/tiger
UPDATE scott.emp SET ename = 'KING' WHERE empno = 7839;
-- 1 row updated.

SELECT * FROM scott.emp;
-- 14 rows selected (with the update reflected)
```

### DCL Command Summary

| Command | Purpose |
|---|---|
| `GRANT` | Give a permission to a user |
| `REVOKE` | Take back a previously granted permission |

> 💡 A granted user must reference another user's table using `schema.tablename` syntax (e.g. `scott.emp`) unless a synonym is created.

[⬆ Back to top](#-table-of-contents)

---

## 16. Copying Tables (CTAS) & Creating New Users

### CTAS — Create Table As Select

**Copy of a table WITH records:**
```sql
CREATE TABLE tabn AS (SELECT stmt);
```
```sql
CREATE TABLE empc AS (SELECT * FROM emp);
```

**Copy of a table WITHOUT records (structure only):**
```sql
CREATE TABLE tabn AS (SELECT * FROM tabn WHERE false-condition);
```
```sql
CREATE TABLE empnorec AS (SELECT * FROM emp WHERE 1=2);
```
> The condition `1=2` is always **FALSE**, so **no records** are copied — only the table **structure** is created.

### Creating a New User (3 Steps)

**Step 1 — Connect to the SYSTEM account**
```sql
CONN system
-- password: tiger
```

**Step 2 — Create the user**
```sql
CREATE USER username IDENTIFIED BY pwd;
```
```sql
CREATE USER sourabh IDENTIFIED BY soura;
```

**Step 3 — Grant permissions**
```sql
GRANT CONNECT, RESOURCE TO username;
```
```sql
GRANT CONNECT, RESOURCE TO sourabh;
```

| Step | Command |
|---|---|
| 1 | `CONN system` |
| 2 | `CREATE USER username IDENTIFIED BY pwd;` |
| 3 | `GRANT CONNECT, RESOURCE TO username;` |

[⬆ Back to top](#-table-of-contents)

---

## 17. SQL\*Plus Commands

**Login example used in class:**
- username: `hr`
- password: `tiger`

| # | Command | Purpose |
|---|---|---|
| 1 | `show user` | Display current username |
| 2 | `show pagesize` | Display page size |
| 3 | `show linesize` | Display line size |
| 4 | `select * from tab;` | Display all table names |
| 5 | `select * from dept;` | Display all details/data of table `dept` |
| 6 | `desc dept;` | Describe table `dept` (column name & datatype) |
| 7 | `set pagesize 100 linesize 100` | Set page size & line size |
| 8 | `cl scr` | Clear screen |
| 9 | `exit` | Exit SQL*Plus (shortcut: `Ctrl+Z`) |
| 10 | `conn` | Connect as another user (enter username or username/password) |

[⬆ Back to top](#-table-of-contents)

---

## 🔑 Interview Quick-Fire Points

- `SEQUEL` was SQL's original name; introduced by Raymond Boyce & Donald Chamberlin.
- Relational model → E.F. Codd. Document model → JSON/XML (MongoDB, Redis, Cassandra).
- Always prefer **VARCHAR2** over VARCHAR in Oracle.
- `PRIMARY KEY = UNIQUE + NOT NULL`.
- Single Row Function → n input, n output. Multi Row Function → n input, 1 output.
- Inner Join → matched rows only. Outer Joins → keep unmatched rows from one/both sides.
- Natural Join has **no Oracle syntax**, ANSI only.
- `ROWNUM` is assigned **before** `ORDER BY` — always use an Inline View to sort-then-rank.
- Query execution order: `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY`.
- WHERE filters rows; HAVING filters groups.
- `TRUNCATE` = no rollback, resets storage, keeps structure. `DROP` = removes table entirely (recoverable via FLASHBACK until purged). `DELETE` = DML, row-by-row, fully rollback-able.
- A dropped table can be restored using `FLASHBACK TABLE ... TO BEFORE DROP` as long as it hasn't been `PURGE`d.
- `ROLLBACK TO savepoint` keeps everything up to that savepoint but undoes everything after it.
- `GRANT`/`REVOKE` require the target table to be accessed as `schema.tablename` by the grantee.

[⬆ Back to top](#-table-of-contents)

---

## 💼 Most Asked Interview Questions

**1. What is the difference between DELETE, TRUNCATE and DROP?**
| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| Type | DML | DDL | DDL |
| Removes | Selected rows (WHERE) | All rows | Entire table (structure + data) |
| Rollback | Yes | No (auto-commits) | Recoverable via FLASHBACK until purged |
| Speed | Slower (row by row, logged) | Faster | Fastest |

**2. What is the difference between WHERE and HAVING?**
`WHERE` filters rows **before** grouping and cannot use aggregate functions. `HAVING` filters **groups** after `GROUP BY` and is commonly used with aggregate functions.

**3. What is the difference between UNION and UNION ALL?**
`UNION` removes duplicate rows (does an implicit sort/dedup, slower); `UNION ALL` keeps all rows including duplicates (faster).

**4. What is a Primary Key vs a Unique Key?**
Both enforce uniqueness, but a Primary Key does **not allow NULL** and a table can have only **one**. A Unique Key **allows one NULL** (some DBs allow more) and a table can have **multiple** Unique Keys.

**5. What is the difference between CHAR and VARCHAR2?**
`CHAR` is fixed-length (pads with spaces, wastes memory); `VARCHAR2` is variable-length (uses only the required space, Oracle-recommended, up to 4000 bytes).

**6. What is a Self Join? Give a real use case.**
A join of a table with itself using aliases. Common use case: employee–manager hierarchy, or finding a grandfather/great-grandfather in a family tree (see [Section 10](#10-self-join--family-tree-example)).

**7. Difference between Inner Join and Outer Join?**
Inner Join returns only matched rows from both tables. Outer Join (Left/Right/Full) additionally returns unmatched rows with NULLs from one or both sides.

**8. How do you find the 2nd highest / Nth highest salary?**
Using an Inline View with `ROWNUM` (after sorting):
```sql
SELECT * FROM (
    SELECT e.*, ROWNUM r FROM (SELECT * FROM emp ORDER BY sal DESC) e
) WHERE r = 2;
```

**9. Why can't you use `WHERE ROWNUM = 2` directly?**
Because `ROWNUM` is assigned **before** `ORDER BY` executes and row numbering starts fresh with each query, so a filter like `ROWNUM = 2` never matches (Oracle checks row 1 first, fails, and never gets to a "row 2"). You must first sort in an inline view, then apply `ROWNUM` in the outer query.

**10. What is a Correlated Subquery? How is it different from a Nested Subquery?**
A Correlated Subquery references a column from the **outer query** and executes **once per outer row**. A Nested Subquery is fully independent, executes **once**, and its result is used by the outer query.

**11. What is the difference between HAVING and Subquery filtering?**
`HAVING` filters **aggregated/grouped** results; a subquery in `WHERE` filters based on values computed from another query, before or independent of grouping.

**12. What does `(+)` mean in Oracle old-style joins?**
It marks the "deficient" side of an outer join — the side that should be padded with NULLs for unmatched rows. `a.col = b.col(+)` is a LEFT OUTER JOIN; `a.col(+) = b.col` is a RIGHT OUTER JOIN.

**13. Can a dropped table be recovered? How?**
Yes — as long as it hasn't been purged, using `FLASHBACK TABLE tabname TO BEFORE DROP;`. Once `PURGE`d (or purged automatically by policy), it cannot be recovered.

**14. What is the difference between `IN` and `EXISTS`?**
`IN` compares a value against a **list** returned by the subquery (works best with smaller result sets). `EXISTS` merely checks whether the subquery returns **any row at all** (often faster for large/correlated subqueries since it can short-circuit).

**15. What is Normalization? Why do we need Joins because of it?**
Normalization splits data into multiple related tables to avoid redundancy and maintain data integrity. Because related data now lives in separate tables, **Joins** are required to bring that data back together meaningfully.

**16. What's the difference between `NVL` and `NVL2`?**
`NVL(a, b)` → returns `b` if `a` is NULL, else returns `a`.
`NVL2(a, b, c)` → returns `c` if `a` is NULL, else returns `b`.

**17. What is a Savepoint used for, and how is it different from Commit?**
`SAVEPOINT` marks an intermediate point within an ongoing transaction that you can roll back to, without discarding the entire transaction. `COMMIT` permanently ends the transaction and makes all changes permanent — you can no longer roll back past it.

**18. What is the difference between a Single Row and Multi Row Subquery?**
A Single Row Subquery returns exactly one value and uses operators like `=`, `>`, `<`. A Multi Row Subquery returns multiple values and must use `IN`, `ANY`, `ALL`, or `EXISTS`.

**19. What happens if you `GRANT` a permission and the grantee queries the table directly without the schema prefix?**
It throws `ORA-00942: table or view does not exist`, because the table belongs to another user's schema. The grantee must use `schema.tablename` (e.g. `scott.emp`) unless a synonym is created.

**20. What is the purpose of `PURGE`, and what error do you get if you purge a table that was never dropped?**
`PURGE` permanently removes a table from the recycle bin so it can no longer be flashed back. If the table was never dropped (i.e., not in the recycle bin), Oracle throws `ORA-38307: object not in RECYCLEBIN`.

[⬆ Back to top](#-table-of-contents)
