# 📘 Oracle SQL & DBMS – Complete Notes

A complete, structured reference covering DBMS basics, Data Types, Constraints, Functions, Joins, Operators, Subqueries, and Query Clauses.

---

## 📑 Table of Contents

1. [DBMS & SQL Basics](#1-dbms--sql-basics)
2. [Data Types](#2-data-types)
3. [Constraints](#3-constraints)
4. [SQL Functions](#4-sql-functions)
5. [SQL Joins](#5-sql-joins)
6. [SQL Operators](#6-sql-operators)
7. [Subqueries & ROWNUM](#7-subqueries--rownum)
8. [WHERE, GROUP BY, HAVING & ORDER BY](#8-where-group-by-having--order-by)
9. [SQL\*Plus Commands](#9-sqlplus-commands)

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
| **DDL** | Data Definition Language | Defines/modifies structure of DB objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` |
| **DML** | Data Manipulation Language | Manipulates data inside tables | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | Retrieves/queries data | `SELECT` |
| **DCL** | Data Control Language | Controls access/permissions on the DB | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Manages transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

**Basic building blocks of SQL:**
- Datatypes / Operators
- DDL, DML, DCL, DQL, TCL
- Functions, `GROUP BY`, `WHERE`, `ORDER BY`, `HAVING`, `JOIN`, Subquery
- Normalization

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

---

## 4. SQL Functions

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

---

## 5. SQL Joins

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
A table joined with **itself**, using aliases — commonly used for employee–manager relationships (or any hierarchical data).
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

### 6) Left Outer Join
Returns **all rows from the left table** + matching rows from the right table (NULL where no match).
```sql
-- ANSI
SELECT e.ename, d.dname FROM emp e LEFT OUTER JOIN dept d ON e.deptno=d.deptno;
-- Oracle old
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno=d.deptno(+);
```

### 7) Right Outer Join
Returns **all rows from the right table** + matching rows from the left table (NULL where no match).
```sql
-- ANSI
SELECT e.ename, d.dname FROM emp e RIGHT OUTER JOIN dept d ON e.deptno=d.deptno;
-- Oracle old
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno(+)=d.deptno;
```

### 8) Full Outer Join
Returns **all rows from both tables** — matched rows merged, unmatched rows show NULL.
```sql
-- ANSI
SELECT e.ename, d.dname FROM emp e FULL OUTER JOIN dept d ON e.deptno=d.deptno;
```
Not supported using the `(+)` operator in old Oracle syntax — traditionally achieved via `LEFT OUTER JOIN UNION RIGHT OUTER JOIN`.

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

---

## 6. SQL Operators

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

---

## 7. Subqueries & ROWNUM

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

---

## 8. WHERE, GROUP BY, HAVING & ORDER BY

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

---

## 9. SQL\*Plus Commands

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
