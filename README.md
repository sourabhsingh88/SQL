# 📘 Oracle SQL & PL/SQL – Complete Interview Notes (Oracle Only)

Pure Oracle reference — no MySQL comparisons. Covers DBMS basics, Data Types, Constraints, DDL, DML, DQL, Functions, Joins, Operators, CASE, Subqueries, Grouping, TCL, DCL, Views, Sequences, Ranking Functions, Triggers, Indexing, Normalization, LPAD/RPAD, Hierarchical Queries, Type Conversion, DBA Concepts, **Stored Procedures & Functions, CTEs (WITH clause), Scheduled Jobs**, and practice questions with hidden solutions.

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
12. [CASE Expression & DECODE](#12-case-expression--decode)
13. [Subqueries, Correlated Subqueries & ROWNUM](#13-subqueries-correlated-subqueries--rownum)
14. [WHERE, GROUP BY, HAVING & ORDER BY](#14-where-group-by-having--order-by)
15. [TCL – Transaction Control Language](#15-tcl--transaction-control-language)
16. [DCL – Data Control Language](#16-dcl--data-control-language)
17. [CTAS & Creating New Users](#17-ctas--creating-new-users)
18. [SQL\*Plus Commands](#18-sqlplus-commands)
19. [Views & Materialized Views](#19-views--materialized-views)
20. [Sequences](#20-sequences)
21. [Ranking Functions – ROW_NUMBER, RANK, DENSE_RANK](#21-ranking-functions--row_number-rank-dense_rank)
22. [Triggers (Full Detail)](#22-triggers-full-detail)
23. [Indexing](#23-indexing)
24. [Normalization](#24-normalization)
25. [LPAD/RPAD](#25-lpadrpad)
26. [Hierarchical Queries — CONNECT BY, PRIOR, LEVEL](#26-hierarchical-queries--connect-by-prior-level)
27. [CTE — WITH Clause (Including Recursive CTE)](#27-cte--with-clause-including-recursive-cte)
28. [Stored Procedures & Functions (PL/SQL)](#28-stored-procedures--functions-plsql)
29. [Packages](#29-packages)
30. [Scheduled Jobs — DBMS_SCHEDULER](#30-scheduled-jobs--dbms_scheduler)
31. [Type Conversion — TO_CHAR, TO_DATE, TO_NUMBER, CAST](#31-type-conversion--to_char-to_date-to_number-cast)
32. [DBA-Level Concepts](#32-dba-level-concepts)
33. [🔑 Interview Quick-Fire Points](#33--interview-quick-fire-points)
34. [💼 Most Asked Interview Questions](#34--most-asked-interview-questions)
35. [🧪 Practice Questions — Hidden Solutions](#35--practice-questions--hidden-solutions)

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

**1) Relational Model** — stores data in the form of a **table**.

| Concept | Meaning |
|---|---|
| Table | Combination of rows and columns |
| Row (Tuple) | Represents all attributes of one entity |
| Column (Attribute) | Represents a single attribute of an entity |
| Cell | Intersection space between a row and a column |

> Relational model was introduced by **E.F. Codd**.

| id | name | age |
|----|------|-----|
| 101 | dingu | 21 |
| 102 | dingi | 16 |
| 103 | raju | 25 |
| 104 | chutki | 18 |

**2) Document Model** — stores data as **JSON** or **XML**. Examples: MongoDB, Redis, Cassandra.

### SQL Command Categories

| Language | Full Form | Purpose | Common Commands |
|---|---|---|---|
| **DDL** | Data Definition Language | Defines/modifies structure of DB objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`, `FLASHBACK`, `PURGE` |
| **DML** | Data Manipulation Language | Manipulates data inside tables | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | Retrieves/queries data | `SELECT` |
| **DCL** | Data Control Language | Controls access/permissions on the DB | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Manages transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

[⬆ Back to top](#-table-of-contents)

---

## 2. Data Types

### CHAR
Fixed-length character type, max **2000 bytes**. Extra space is padded with blanks.
```sql
CREATE TABLE student (gender CHAR(1));
```
✅ Faster comparison, good for fixed-length data. ❌ Wastes memory if value is shorter than declared size.

### VARCHAR2
Oracle's **recommended** variable-length character type, up to **4000 bytes**. Always prefer this over legacy `VARCHAR`.
```sql
ename VARCHAR2(50)
```

### NUMBER
```sql
NUMBER(precision, scale)
```
| Declaration | Valid Example |
|---|---|
| `NUMBER(3)` | 374 |
| `NUMBER(5,2)` | 548.17 |
| `NUMBER(4,4)` | 0.7654 |

### DATE
Stores date **and time** (down to the second). Default display format: `DD-MON-YY` / `DD-MON-YYYY`.
```sql
hiredate DATE
```

### TIMESTAMP
Like DATE but with fractional seconds precision — used where sub-second accuracy matters (e.g. audit logging).
```sql
created_at TIMESTAMP(6) DEFAULT SYSTIMESTAMP
```

### LOB (Large Object)
| Type | Stores |
|---|---|
| **CLOB** | Large text, documents, XML |
| **BLOB** | Images, videos, audio, PDFs |

```sql
resume CLOB,
photo BLOB
```

### Summary

| Data Type | Used For |
|---|---|
| CHAR | Fixed-length characters |
| VARCHAR2 | Variable-length characters (preferred) |
| NUMBER | Numbers (integer/decimal) |
| DATE | Date + time |
| TIMESTAMP | Date + time with fractional seconds |
| CLOB | Large text |
| BLOB | Images, videos, files |

[⬆ Back to top](#-table-of-contents)

---

## 3. Constraints

### Types
1. UNIQUE
2. NOT NULL
3. PRIMARY KEY
4. CHECK
5. DEFAULT
6. FOREIGN KEY

```sql
CREATE TABLE department (
    deptno NUMBER PRIMARY KEY,
    dname  VARCHAR2(30)
);

CREATE TABLE employee (
    empno   NUMBER PRIMARY KEY,
    ename   VARCHAR2(30) NOT NULL,
    email   VARCHAR2(50) UNIQUE,
    age     NUMBER CHECK(age >= 18),
    status  VARCHAR2(20) DEFAULT 'ACTIVE',
    deptno  NUMBER,
    FOREIGN KEY(deptno) REFERENCES department(deptno)
);
```

### Quick Interview Points
- `PRIMARY KEY = UNIQUE + NOT NULL`
- UNIQUE allows NULL; PRIMARY KEY does not.
- A table can have multiple UNIQUE constraints, but only **one** PRIMARY KEY.
- FOREIGN KEY creates a **child–parent** relationship between two tables.
- 🔗 A **PRIMARY KEY is automatically indexed** by Oracle.

[⬆ Back to top](#-table-of-contents)

---

## 4. DDL – Data Definition Language

Commands: `CREATE`, `RENAME`, `ALTER`, `TRUNCATE`, `DROP`, `FLASHBACK`, `PURGE`

```sql
-- CREATE
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(20));

-- RENAME
RENAME emp TO student;

-- ALTER
ALTER TABLE stud ADD phno NUMBER(10);
ALTER TABLE stud MODIFY email VARCHAR2(25);
ALTER TABLE stud DROP COLUMN email;
ALTER TABLE stud RENAME COLUMN name TO full_name;

-- TRUNCATE (removes all rows, keeps structure, not recoverable)
TRUNCATE TABLE stud;

-- DROP (moves table to recycle bin)
DROP TABLE stud;

-- FLASHBACK (restores from recycle bin)
FLASHBACK TABLE stud TO BEFORE DROP;

-- Check recycle bin
SELECT original_name FROM recyclebin;

-- PURGE (permanently removes from recycle bin)
DROP TABLE stud;
PURGE TABLE stud;
```

⚠️ `PURGE` on a table never dropped throws `ORA-38307: object not in RECYCLEBIN`.

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

```sql
-- INSERT
INSERT INTO stud VALUES (101, 'ramesh');
INSERT INTO stud (sid, name) VALUES (102, 'ramya');

-- UPDATE — always use WHERE
UPDATE emp SET sal = sal + sal * 0.10 WHERE job = 'CLERK';

-- DELETE — always use WHERE
DELETE FROM emp WHERE job = 'CLERK';
```

| Command | Purpose | WHERE mandatory? |
|---|---|---|
| `INSERT` | Add new records | No |
| `UPDATE` | Modify existing records | ⚠️ Strongly recommended |
| `DELETE` | Remove records | ⚠️ Strongly recommended |

[⬆ Back to top](#-table-of-contents)

---

## 6. DQL – Selection & Projection

```sql
SELECT DISTINCT sal FROM emp;

SELECT ename, (sal*12)/4 AS quarterly_sal FROM emp;
SELECT ename, sal + 300 AS sal_bonus FROM emp;
SELECT ename, (sal + (sal * 0.10)) AS incremented_sal FROM emp;
SELECT ename, (sal*12) + (sal*12*0.15) AS annual_salary FROM emp;
```

### Order of Execution
| Step | Clause | Purpose |
|---|---|---|
| 1 | `FROM` | Checks whether the table is present |
| 2 | `WHERE` | Filters rows based on condition(s) |
| 3 | `SELECT` | Selects & displays rows |

[⬆ Back to top](#-table-of-contents)

---

## 7. SQL Functions

### A. Character Functions

| Function | Purpose | Example | Result |
|---|---|---|---|
| `UPPER(arg)` | Uppercase | `UPPER('raju')` | RAJU |
| `LOWER(arg)` | Lowercase | `LOWER('RAJU')` | raju |
| `INITCAP(arg)` | Capitalize each word | `INITCAP('manoj is kind')` | Manoj Is Kind |

```sql
SUBSTR('Manu',1,3)                          -- 'Man'
SUBSTR(ename,1,1)                           -- first char
SUBSTR(ename,LENGTH(ename),1)               -- last letter
CONCAT('Hi:',empname)                       -- string join
REVERSE('FORD')                              -- 'DORF'
INSTR('NAYAN','A',1,1)                       -- 2
INSTR('NAYAN','A',1,2)                       -- 4
TRIM(LEADING 'P' FROM 'PUSHPA')              -- 'USHPA'
REPLACE('BANGLORE','B','M')                  -- 'MANGLORE'
LENGTH('Pushpa') - LENGTH(REPLACE('Pushpa','P',''))  -- count occurrences of 'P'
SUBSTR(email, INSTR(email,'@')+1)             -- domain from email
'Hi:' || empname                              -- concatenation operator
```

### B. Number Functions

| Function | Purpose | Example | Result |
|---|---|---|---|
| `MOD(m,n)` | Remainder | `MOD(11,2)` | 2 |
| `SQRT(arg)` | Square root | `SQRT(100)` | 10 |
| `POWER(m,n)` | m raised to power n | `POWER(2,3)` | 8 |
| `ROUND(n,d)` | Rounds to d places | `ROUND(45.926,2)` | 45.93 |
| `TRUNC(n,d)` | Truncates to d places | `TRUNC(45.926,2)` | 45.92 |
| `CEIL(n)` | Smallest integer ≥ n | `CEIL(4.1)` | 5 |
| `FLOOR(n)` | Largest integer ≤ n | `FLOOR(4.9)` | 4 |
| `ABS(n)` | Absolute value | `ABS(-7)` | 7 |
| `SIGN(n)` | Returns -1, 0, or 1 | `SIGN(-45)` | -1 |

> 💡 `ROUND(45.926,0)=46`, `TRUNC(45.926,0)=45` — ROUND adjusts, TRUNC chops.

### C. Date Functions

| Function | Purpose | Example |
|---|---|---|
| `SYSDATE` | Current date & time | `SELECT SYSDATE FROM dual;` |
| `SYSTIMESTAMP` | Current date & time with fractional seconds | `SELECT SYSTIMESTAMP FROM dual;` |
| `MONTHS_BETWEEN(d1,d2)` | Months between two dates | `MONTHS_BETWEEN(SYSDATE, hiredate)` |
| `ADD_MONTHS(date,n)` | Adds n months | `ADD_MONTHS(SYSDATE, 3)` |
| `NEXT_DAY(date,'day')` | Next occurrence of weekday | `NEXT_DAY(SYSDATE,'MONDAY')` |
| `LAST_DAY(date)` | Last day of the month | `LAST_DAY(SYSDATE)` |
| `ROUND(date,'fmt')` | Rounds a date | `ROUND(SYSDATE,'YEAR')` |
| `TRUNC(date,'fmt')` | Truncates a date | `TRUNC(SYSDATE,'MONTH')` |

```sql
SELECT hiredate + 30 FROM emp;          -- 30 days after hiredate
SELECT SYSDATE - hiredate FROM emp;     -- days employed
```

### D. Multi-Row (Aggregate) Functions

```sql
SELECT SUM(sal), AVG(sal), COUNT(*), MAX(sal), MIN(sal) FROM emp;
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno;
```
> ⚠️ Any non-aggregated column in `SELECT` **must** appear in `GROUP BY`.

[⬆ Back to top](#-table-of-contents)

---

## 8. SQL Joins

```sql
-- INNER JOIN
SELECT e.ename, d.dname FROM emp e INNER JOIN dept d ON e.deptno = d.deptno;

-- LEFT JOIN
SELECT e.ename, d.dname FROM emp e LEFT JOIN dept d ON e.deptno = d.deptno;

-- FULL OUTER JOIN
SELECT e.ename, d.dname FROM emp e FULL OUTER JOIN dept d ON e.deptno = d.deptno;

-- CROSS JOIN
SELECT e.ename, d.dname FROM emp e CROSS JOIN dept d;
```

### Old-style Oracle Join Syntax (pre-ANSI, still asked in interviews)
```sql
-- INNER JOIN (old style)
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno = d.deptno;

-- LEFT OUTER JOIN (old style — (+) on the side that can be missing)
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno = d.deptno(+);

-- RIGHT OUTER JOIN (old style)
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno(+) = d.deptno;
```

[⬆ Back to top](#-table-of-contents)

---

## 9. Outer Joins – Practical Walkthrough

```sql
-- Employees with no department assigned
SELECT e.ename
FROM emp e
LEFT JOIN dept d ON e.deptno = d.deptno
WHERE d.deptno IS NULL;

-- Departments with no employees
SELECT d.dname
FROM emp e
RIGHT JOIN dept d ON e.deptno = d.deptno
WHERE e.empno IS NULL;
```
This "LEFT JOIN + WHERE right.key IS NULL" pattern is a classic interview trick for finding **unmatched rows**.

[⬆ Back to top](#-table-of-contents)

---

## 10. Self Join – Family Tree Example

```sql
CREATE TABLE person (
    id NUMBER PRIMARY KEY,
    name VARCHAR2(30),
    parent_id NUMBER
);

SELECT c.name AS child, p.name AS parent
FROM person c
JOIN person p ON c.parent_id = p.id;
```

**Employee-manager version:**
```sql
SELECT e.ename AS employee, m.ename AS manager
FROM emp e
LEFT JOIN emp m ON e.mgr = m.empno;
```

For multi-level traversal, see [Section 26 – Hierarchical Queries](#26-hierarchical-queries--connect-by-prior-level).

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
| Set | `UNION`, `UNION ALL`, `INTERSECT`, `MINUS` |

```sql
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;
SELECT * FROM emp WHERE deptno IN (10,20,30);
SELECT * FROM emp WHERE ename LIKE 'S%';      -- starts with S
SELECT * FROM emp WHERE ename LIKE '%TH';     -- ends with TH
SELECT * FROM emp WHERE ename LIKE '_A%';     -- 2nd letter is A
SELECT * FROM emp WHERE comm IS NULL;
```

### Set Operators
```sql
SELECT deptno FROM emp UNION SELECT deptno FROM dept;        -- combines, dedups
SELECT deptno FROM emp UNION ALL SELECT deptno FROM dept;    -- combines, keeps dups
SELECT empno FROM emp INTERSECT SELECT empno FROM emp_backup; -- common rows
SELECT empno FROM emp MINUS SELECT empno FROM emp_backup;     -- in first, not second
```

[⬆ Back to top](#-table-of-contents)

---

## 12. CASE Expression & DECODE

```sql
SELECT ename, sal,
  CASE
    WHEN sal >= 5000 THEN 'HIGH'
    WHEN sal >= 2000 THEN 'MEDIUM'
    ELSE 'LOW'
  END AS sal_grade
FROM emp;

-- Simple CASE
SELECT ename, deptno,
  CASE deptno
    WHEN 10 THEN 'ACCOUNTING'
    WHEN 20 THEN 'RESEARCH'
    WHEN 30 THEN 'SALES'
    ELSE 'UNKNOWN'
  END AS dept_name
FROM emp;

-- DECODE (shorthand for simple CASE)
SELECT ename, DECODE(deptno,10,'ACCOUNTING',20,'RESEARCH',30,'SALES','UNKNOWN') AS dept_name
FROM emp;
```

[⬆ Back to top](#-table-of-contents)

---

## 13. Subqueries, Correlated Subqueries & ROWNUM

```sql
-- Single-row subquery
SELECT ename, sal FROM emp WHERE sal > (SELECT AVG(sal) FROM emp);

-- Multi-row subquery
SELECT ename FROM emp WHERE deptno IN (SELECT deptno FROM dept WHERE loc='CHICAGO');
SELECT ename FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=30);
SELECT ename FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno=30);

-- Subquery in FROM (inline view)
SELECT deptno, avg_sal FROM (
    SELECT deptno, AVG(sal) AS avg_sal FROM emp GROUP BY deptno
) WHERE avg_sal > 3000;
```

### Correlated Subquery
```sql
-- Employees earning more than the average of their own department
SELECT e.ename, e.sal, e.deptno
FROM emp e
WHERE e.sal > (SELECT AVG(sal) FROM emp WHERE deptno = e.deptno);

-- EXISTS
SELECT d.dname FROM dept d
WHERE EXISTS (SELECT 1 FROM emp e WHERE e.deptno = d.deptno);
```

| Subquery | Correlated Subquery |
|---|---|
| Runs once, independently | Runs once per outer row |
| No reference to outer query | References outer query column |

### ROWNUM
```sql
SELECT * FROM emp WHERE ROWNUM <= 5;

-- Top N by salary (must wrap in subquery)
SELECT * FROM (
    SELECT * FROM emp ORDER BY sal DESC
) WHERE ROWNUM <= 3;
```
> ⚠️ Classic trap: `SELECT * FROM emp WHERE ROWNUM > 1;` returns **zero rows**.

> 💡 Oracle 12c+ pagination alternative: `SELECT * FROM emp ORDER BY sal DESC OFFSET 10 ROWS FETCH NEXT 5 ROWS ONLY;`

[⬆ Back to top](#-table-of-contents)

---

## 14. WHERE, GROUP BY, HAVING & ORDER BY

### Logical Order of Execution
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```

```sql
SELECT deptno, COUNT(*) AS emp_count
FROM emp
WHERE sal > 2000
GROUP BY deptno
HAVING COUNT(*) > 3
ORDER BY emp_count DESC;
```
> 💡 `WHERE` filters rows *before* grouping and cannot use aggregates; `HAVING` filters *groups* after aggregation and can.

[⬆ Back to top](#-table-of-contents)

---

## 15. TCL – Transaction Control Language

```sql
UPDATE emp SET sal = sal + 500 WHERE deptno = 10;
SAVEPOINT sp1;

DELETE FROM emp WHERE deptno = 20;
ROLLBACK TO sp1;   -- undoes the DELETE, keeps the UPDATE

COMMIT;            -- permanently saves the UPDATE
```

### ACID Properties
| Property | Meaning |
|---|---|
| **Atomicity** | Transaction is all-or-nothing |
| **Consistency** | DB moves from one valid state to another |
| **Isolation** | Concurrent transactions don't interfere |
| **Durability** | Committed changes persist after a crash |

[⬆ Back to top](#-table-of-contents)

---

## 16. DCL – Data Control Language

```sql
GRANT SELECT, INSERT ON emp TO ramesh;
GRANT ALL PRIVILEGES ON emp TO ramesh;
REVOKE INSERT ON emp FROM ramesh;
```

[⬆ Back to top](#-table-of-contents)

---

## 17. CTAS & Creating New Users

```sql
-- Create Table As Select
CREATE TABLE emp_backup AS SELECT * FROM emp;
CREATE TABLE clerks AS SELECT * FROM emp WHERE job='CLERK';
CREATE TABLE emp_empty AS SELECT * FROM emp WHERE 1=2;   -- structure only
```
> Constraints (PK, FK, checks) are **not** copied by CTAS — only NOT NULL is preserved.

```sql
-- Creating new users
CREATE USER ramesh IDENTIFIED BY password123;
GRANT CONNECT, RESOURCE TO ramesh;
ALTER USER ramesh ACCOUNT UNLOCK;
```

[⬆ Back to top](#-table-of-contents)

---

## 18. SQL*Plus Commands

| Command | Purpose |
|---|---|
| `DESC tabname` | Describes table structure |
| `SET LINESIZE n` | Sets output line width |
| `SET PAGESIZE n` | Rows displayed per page |
| `SPOOL filename` | Saves query output to a file |
| `SPOOL OFF` | Stops spooling |
| `/` | Re-runs the last SQL command |
| `SHOW USER` | Displays current logged-in user |

[⬆ Back to top](#-table-of-contents)

---

## 19. Views & Materialized Views

```sql
CREATE VIEW emp_view AS
SELECT empno, ename, sal FROM emp WHERE deptno = 10;

CREATE OR REPLACE VIEW emp_view AS
SELECT empno, ename, sal, deptno FROM emp WHERE deptno = 10;

DROP VIEW emp_view;
```
✅ Simplifies complex queries, hides sensitive columns. ❌ Views with joins/aggregates/DISTINCT are often **not updatable**.

```sql
CREATE MATERIALIZED VIEW mv_emp_summary
AS SELECT deptno, SUM(sal) AS total_sal FROM emp GROUP BY deptno;

EXEC DBMS_MVIEW.REFRESH('mv_emp_summary');
```

| View | Materialized View |
|---|---|
| Virtual, no storage | Physical, stores data |
| Always up-to-date | Needs refresh |
| Slower for complex queries | Faster (pre-computed) |

[⬆ Back to top](#-table-of-contents)

---

## 20. Sequences

```sql
CREATE SEQUENCE emp_seq
START WITH 1
INCREMENT BY 1
MAXVALUE 9999
NOCYCLE;

INSERT INTO emp(empno, ename) VALUES (emp_seq.NEXTVAL, 'Ramesh');

SELECT emp_seq.CURRVAL FROM dual;
SELECT emp_seq.NEXTVAL FROM dual;

ALTER SEQUENCE emp_seq INCREMENT BY 2;
DROP SEQUENCE emp_seq;
```

| Pseudo-column | Purpose |
|---|---|
| `NEXTVAL` | Generates & returns the next value, advances the sequence |
| `CURRVAL` | Returns the current (last generated) value |

[⬆ Back to top](#-table-of-contents)

---

## 21. Ranking Functions – ROW_NUMBER, RANK, DENSE_RANK

```sql
SELECT ename, deptno, sal,
  ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rn,
  RANK()       OVER (PARTITION BY deptno ORDER BY sal DESC) AS rnk,
  DENSE_RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS drnk
FROM emp;
```

| ename | sal | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|---|
| A | 5000 | 1 | 1 | 1 |
| B | 4000 | 2 | 2 | 2 |
| C | 4000 | 3 | 2 | 2 |
| D | 3000 | 4 | 4 | 3 |

- **ROW_NUMBER** → always unique, no ties.
- **RANK** → same rank for ties, skips next rank (gap).
- **DENSE_RANK** → same rank for ties, no gap.

```sql
-- Nth highest salary per department
SELECT * FROM (
    SELECT ename, deptno, sal,
           ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rn
    FROM emp
) WHERE rn = 2;
```

Other useful window functions: `LAG()`, `LEAD()`, `FIRST_VALUE()`, `LAST_VALUE()`, `NTILE(n)`.

```sql
-- Compare each employee's salary to the previous employee's salary (by hiredate)
SELECT ename, sal, hiredate,
       LAG(sal) OVER (ORDER BY hiredate) AS prev_sal,
       LEAD(sal) OVER (ORDER BY hiredate) AS next_sal
FROM emp;
```

[⬆ Back to top](#-table-of-contents)

---

## 22. Triggers (Full Detail)

A **Trigger** is a stored PL/SQL block that automatically executes in response to an event (`INSERT`, `UPDATE`, `DELETE`, or even DDL/database events) on a table.

### Row-level trigger
```sql
CREATE OR REPLACE TRIGGER trg_before_insert
BEFORE INSERT ON emp
FOR EACH ROW
BEGIN
    :NEW.hiredate := SYSDATE;
END;
/
```

### Statement-level trigger (no `FOR EACH ROW`)
```sql
CREATE OR REPLACE TRIGGER trg_log_bulk_delete
AFTER DELETE ON emp
BEGIN
    INSERT INTO audit_log(action, action_time) VALUES ('BULK DELETE ON EMP', SYSDATE);
END;
/
```
Fires **once per statement**, regardless of how many rows were affected — unlike a row-level trigger, which fires once per row.

### Column-specific trigger
```sql
CREATE OR REPLACE TRIGGER trg_audit_sal
AFTER UPDATE OF sal ON emp   -- only fires if 'sal' column is part of the UPDATE
FOR EACH ROW
BEGIN
    INSERT INTO sal_audit(empno, old_sal, new_sal, changed_on)
    VALUES (:OLD.empno, :OLD.sal, :NEW.sal, SYSDATE);
END;
/
```

### INSTEAD OF trigger (used on non-updatable views)
```sql
CREATE OR REPLACE TRIGGER trg_instead_of_emp_view
INSTEAD OF INSERT ON emp_view
FOR EACH ROW
BEGIN
    INSERT INTO emp(empno, ename, sal) VALUES (:NEW.empno, :NEW.ename, :NEW.sal);
END;
/
```
Lets you INSERT/UPDATE/DELETE through a view that would otherwise be non-updatable (e.g. one with a join or aggregate).

### Compound trigger (single trigger covering all timing points)
```sql
CREATE OR REPLACE TRIGGER trg_compound_emp
FOR UPDATE ON emp
COMPOUND TRIGGER

    BEFORE STATEMENT IS
    BEGIN
        DBMS_OUTPUT.PUT_LINE('Update starting');
    END BEFORE STATEMENT;

    BEFORE EACH ROW IS
    BEGIN
        :NEW.hiredate := :OLD.hiredate; -- prevent hiredate change
    END BEFORE EACH ROW;

    AFTER EACH ROW IS
    BEGIN
        NULL;
    END AFTER EACH ROW;

    AFTER STATEMENT IS
    BEGIN
        DBMS_OUTPUT.PUT_LINE('Update finished');
    END AFTER STATEMENT;

END trg_compound_emp;
/
```
Useful when you need to avoid the "mutating table" error that occurs when a row-level trigger tries to query/modify the same table it's firing on.

### Trigger types summary

| Trigger Type | Fires |
|---|---|
| `BEFORE INSERT/UPDATE/DELETE` | Before the DML event — validation/defaults |
| `AFTER INSERT/UPDATE/DELETE` | After the DML event — logging/auditing |
| `FOR EACH ROW` | Row-level — once per affected row |
| Statement-level (no `FOR EACH ROW`) | Once per statement |
| `INSTEAD OF` | Replaces the DML on a non-updatable view |
| `COMPOUND TRIGGER` | Combines BEFORE/AFTER STATEMENT/ROW timing in one object |
| DDL trigger (`BEFORE/AFTER CREATE/DROP/ALTER`) | Fires on schema changes |
| Database-event trigger (`LOGON`, `LOGOFF`, `SERVERERROR`) | Fires on system-level events |

```sql
-- DDL trigger example: prevent accidental DROP TABLE in production schema
CREATE OR REPLACE TRIGGER trg_prevent_drop
BEFORE DROP ON SCHEMA
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'DROP TABLE is not allowed in this schema.');
END;
/
```

### Disabling / Enabling triggers
```sql
ALTER TRIGGER trg_before_insert DISABLE;
ALTER TRIGGER trg_before_insert ENABLE;
ALTER TABLE emp DISABLE ALL TRIGGERS;
```

[⬆ Back to top](#-table-of-contents)

---

## 23. Indexing

```sql
CREATE INDEX idx_ename ON emp(ename);
CREATE UNIQUE INDEX idx_empno ON emp(empno);
DROP INDEX idx_ename;
```

| Type | Description |
|---|---|
| **B-Tree Index** | Default; balanced tree, good for equality & range queries |
| **Bitmap Index** | Good for low-cardinality columns (gender, status); used in data warehousing |
| **Unique Index** | Enforces uniqueness (auto-created for PK/UNIQUE) |
| **Composite Index** | Index on multiple columns together |
| **Function-Based Index** | Index on the result of an expression/function, e.g. `UPPER(ename)` |

```sql
-- Function-based index — speeds up case-insensitive searches
CREATE INDEX idx_upper_ename ON emp(UPPER(ename));

SELECT * FROM emp WHERE UPPER(ename) = 'SMITH';  -- now uses the index
```

✅ Speeds up SELECT/WHERE/JOIN/ORDER BY. ❌ Slows down INSERT/UPDATE/DELETE.

[⬆ Back to top](#-table-of-contents)

---

## 24. Normalization

### 1NF — atomic values, no repeating groups.
### 2NF — 1NF + no partial dependency on a composite key.
### 3NF — 2NF + no transitive dependency between non-key columns.
### BCNF — every determinant must be a super key.

| Form | Rule |
|---|---|
| 1NF | Atomic values, no repeating groups |
| 2NF | 1NF + no partial dependency (composite key) |
| 3NF | 2NF + no transitive dependency |
| BCNF | 3NF + every determinant is a super key |

> 💡 Denormalization is sometimes done deliberately for read-heavy systems to reduce JOINs.

[⬆ Back to top](#-table-of-contents)

---

## 25. LPAD/RPAD

```sql
LPAD(str, total_length, 'pad_char')
RPAD(str, total_length, 'pad_char')
```

```sql
SELECT LPAD('45', 5, '0') FROM dual;    -- '00045'
SELECT RPAD('45', 5, '0') FROM dual;    -- '45000'

-- Zero-pad employee IDs
SELECT LPAD(empno, 6, '0') AS emp_code FROM emp;   -- 100 -> '000100'

-- Mask all but last 4 digits of an account number
SELECT LPAD(SUBSTR(acct_no, -4), LENGTH(acct_no), '*') FROM accounts;
-- '1234567890' -> '******7890'
```
If the string is already longer than the target length, both functions **truncate** instead of padding:
```sql
SELECT LPAD('HELLO WORLD', 5) FROM dual;   -- 'HELLO'
```

[⬆ Back to top](#-table-of-contents)

---

## 26. Hierarchical Queries — CONNECT BY, PRIOR, LEVEL

Oracle's native way to query **tree-structured / recursive** data (org charts, category trees, bill-of-materials).

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

**Top-down (managers → subordinates):**
```sql
SELECT LEVEL, ename, mgr
FROM emp2
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr
ORDER BY LEVEL;
```

| LEVEL | ename | mgr |
|---|---|---|
| 1 | KING | NULL |
| 2 | JONES | 1 |
| 2 | BLAKE | 1 |
| 3 | SCOTT | 2 |
| 4 | ADAMS | 3 |

- **`LEVEL`** — depth of the row in the tree (root = 1).
- **`PRIOR`** — marks which side belongs to the parent from the previous level.
- **`START WITH`** — defines the root(s).
- **`CONNECT BY`** — defines the parent-child link used to walk the tree.

**Bottom-up (subordinate → all ancestors):**
```sql
SELECT LEVEL, ename
FROM emp2
START WITH ename = 'ADAMS'
CONNECT BY PRIOR mgr = empno
ORDER BY LEVEL;
```

**Indented org chart (LPAD + LEVEL combo):**
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

**Other hierarchical tools:**
| Function/Clause | Purpose |
|---|---|
| `CONNECT_BY_ROOT` | Returns the root ancestor's value for any row |
| `SYS_CONNECT_BY_PATH(col, 'sep')` | Builds the full path from root to current row |
| `CONNECT BY NOCYCLE` | Prevents infinite loops if data has a cycle |
| `ORDER SIBLINGS BY col` | Sorts children within a parent without breaking tree order |
| `CONNECT_BY_ISLEAF` | 1 if the row has no children, else 0 |

```sql
SELECT ename, SYS_CONNECT_BY_PATH(ename, ' > ') AS path
FROM emp2
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr;
-- ADAMS -> KING > JONES > SCOTT > ADAMS

SELECT ename, CONNECT_BY_ROOT ename AS top_manager
FROM emp2
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr;
```

[⬆ Back to top](#-table-of-contents)

---

## 27. CTE — WITH Clause (Including Recursive CTE)

### Basic CTE
A **CTE (Common Table Expression)** is a named temporary result set defined with `WITH`, usable like a table within the same query — improves readability for complex/nested subqueries.

```sql
WITH dept_totals AS (
    SELECT deptno, SUM(sal) AS total_sal
    FROM emp
    GROUP BY deptno
)
SELECT * FROM dept_totals WHERE total_sal > 5000;
```

### Multiple CTEs in one query
```sql
WITH high_earners AS (
    SELECT * FROM emp WHERE sal > 3000
),
dept_avg AS (
    SELECT deptno, AVG(sal) AS avg_sal FROM emp GROUP BY deptno
)
SELECT h.ename, h.sal, d.avg_sal
FROM high_earners h
JOIN dept_avg d ON h.deptno = d.deptno;
```

### Recursive CTE (Oracle 11g Release 2+)
Oracle's ANSI-standard alternative to `CONNECT BY` — same tree-walking result, different syntax.

```sql
WITH emp_tree (empno, ename, mgr, lvl) AS (
    -- anchor member: the root row(s)
    SELECT empno, ename, mgr, 1
    FROM emp2
    WHERE mgr IS NULL

    UNION ALL

    -- recursive member: joins children to their parent's row in the CTE
    SELECT e.empno, e.ename, e.mgr, t.lvl + 1
    FROM emp2 e
    JOIN emp_tree t ON e.mgr = t.empno
)
SEARCH DEPTH FIRST BY empno SET order_col
CYCLE empno SET is_cycle TO 'Y' DEFAULT 'N'
SELECT lvl, LPAD(' ', (lvl-1)*2) || ename AS org_chart
FROM emp_tree
ORDER BY order_col;
```

| Part | Purpose |
|---|---|
| Anchor member | The base case — first `SELECT`, defines the root(s) |
| `UNION ALL` | Combines anchor with recursive member |
| Recursive member | References the CTE name itself (`emp_tree`), joins to walk down a level |
| `SEARCH DEPTH FIRST BY` | Controls traversal order (depth-first vs breadth-first) |
| `CYCLE ... SET ... TO ... DEFAULT` | Detects and flags cycles instead of looping forever |

### CONNECT BY vs Recursive CTE

| Feature | CONNECT BY | Recursive CTE (`WITH`) |
|---|---|---|
| Syntax style | Oracle proprietary | ANSI SQL standard |
| `LEVEL` pseudo-column | Automatic | Must be built manually (`lvl + 1`) |
| Path building | `SYS_CONNECT_BY_PATH` | Manual string concatenation |
| Portability | Oracle-only | Works across most modern RDBMS |
| Readability for complex trees | Can get unwieldy with several conditions | Often clearer, especially with multiple recursive members |

### Interview point
> Both achieve the same goal — Oracle DBAs are expected to know **both**, since `CONNECT BY` is still heavily used in legacy Oracle codebases, while recursive `WITH` CTEs are the modern, portable, ANSI-standard approach favored in new code.

[⬆ Back to top](#-table-of-contents)

---

## 28. Stored Procedures & Functions (PL/SQL)

### Stored Procedure
A **named PL/SQL block** stored in the database that performs an action; does **not** need to return a value directly (though it can via `OUT` parameters).

```sql
CREATE OR REPLACE PROCEDURE give_raise (
    p_empno IN emp.empno%TYPE,
    p_pct   IN NUMBER
) IS
BEGIN
    UPDATE emp
    SET sal = sal + (sal * p_pct / 100)
    WHERE empno = p_empno;

    IF SQL%ROWCOUNT = 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'No employee found with that empno');
    END IF;

    COMMIT;
END give_raise;
/
```

**Calling it:**
```sql
EXEC give_raise(101, 10);
-- or
BEGIN
    give_raise(101, 10);
END;
/
```

### Procedure with OUT parameter
```sql
CREATE OR REPLACE PROCEDURE get_emp_details (
    p_empno IN  emp.empno%TYPE,
    p_ename OUT emp.ename%TYPE,
    p_sal   OUT emp.sal%TYPE
) IS
BEGIN
    SELECT ename, sal INTO p_ename, p_sal
    FROM emp WHERE empno = p_empno;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        p_ename := 'NOT FOUND';
        p_sal := 0;
END get_emp_details;
/
```

```sql
DECLARE
    v_name emp.ename%TYPE;
    v_sal  emp.sal%TYPE;
BEGIN
    get_emp_details(101, v_name, v_sal);
    DBMS_OUTPUT.PUT_LINE(v_name || ' earns ' || v_sal);
END;
/
```

### Function
A **named PL/SQL block that always returns exactly one value**, usable directly inside SQL expressions.

```sql
CREATE OR REPLACE FUNCTION get_annual_salary (
    p_empno IN emp.empno%TYPE
) RETURN NUMBER IS
    v_sal emp.sal%TYPE;
BEGIN
    SELECT sal INTO v_sal FROM emp WHERE empno = p_empno;
    RETURN v_sal * 12;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 0;
END get_annual_salary;
/
```

**Calling it — from PL/SQL or directly in SQL:**
```sql
SELECT ename, get_annual_salary(empno) AS annual_sal FROM emp;

DECLARE
    v_annual NUMBER;
BEGIN
    v_annual := get_annual_salary(101);
    DBMS_OUTPUT.PUT_LINE(v_annual);
END;
/
```

### Procedure vs Function

| Feature | Procedure | Function |
|---|---|---|
| Returns a value | Optional (via `OUT`/`IN OUT` params) | Mandatory (exactly one via `RETURN`) |
| Callable directly in SQL (`SELECT`) | No | Yes |
| Can perform DML and COMMIT | Yes | Yes, but risky if called from SQL (side-effect restrictions apply) |
| Typical use | Perform an action/business operation | Compute and return a value |

### Cursors (explicit)
Used to process query results row by row inside PL/SQL.

```sql
DECLARE
    CURSOR emp_cur IS SELECT empno, ename, sal FROM emp WHERE deptno = 10;
    v_empno emp.empno%TYPE;
    v_ename emp.ename%TYPE;
    v_sal   emp.sal%TYPE;
BEGIN
    OPEN emp_cur;
    LOOP
        FETCH emp_cur INTO v_empno, v_ename, v_sal;
        EXIT WHEN emp_cur%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE(v_ename || ' - ' || v_sal);
    END LOOP;
    CLOSE emp_cur;
END;
/

-- Simpler: cursor FOR loop (implicitly opens/fetches/closes)
BEGIN
    FOR rec IN (SELECT empno, ename, sal FROM emp WHERE deptno = 10) LOOP
        DBMS_OUTPUT.PUT_LINE(rec.ename || ' - ' || rec.sal);
    END LOOP;
END;
/
```

### Exception Handling
```sql
BEGIN
    UPDATE emp SET sal = sal * 1.1 WHERE empno = 9999;
    IF SQL%ROWCOUNT = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No matching employee.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

[⬆ Back to top](#-table-of-contents)

---

## 29. Packages

A **Package** bundles related procedures, functions, variables, and cursors into one named unit — a **specification** (public interface) and a **body** (implementation).

```sql
-- Package specification (what's visible/callable from outside)
CREATE OR REPLACE PACKAGE emp_pkg IS
    PROCEDURE give_raise(p_empno NUMBER, p_pct NUMBER);
    FUNCTION get_annual_salary(p_empno NUMBER) RETURN NUMBER;
END emp_pkg;
/

-- Package body (actual implementation, can also have private helpers)
CREATE OR REPLACE PACKAGE BODY emp_pkg IS

    PROCEDURE give_raise(p_empno NUMBER, p_pct NUMBER) IS
    BEGIN
        UPDATE emp SET sal = sal + (sal * p_pct / 100) WHERE empno = p_empno;
        COMMIT;
    END give_raise;

    FUNCTION get_annual_salary(p_empno NUMBER) RETURN NUMBER IS
        v_sal NUMBER;
    BEGIN
        SELECT sal INTO v_sal FROM emp WHERE empno = p_empno;
        RETURN v_sal * 12;
    END get_annual_salary;

END emp_pkg;
/
```

**Calling package members:**
```sql
EXEC emp_pkg.give_raise(101, 10);
SELECT emp_pkg.get_annual_salary(101) FROM dual;
```

### Interview point
> Packages group logically related procedures/functions together (better organization, one compile unit), allow **private** helper procedures/functions hidden in the body but not declared in the spec, and improve performance since the whole package is loaded into memory together on first use.

[⬆ Back to top](#-table-of-contents)

---

## 30. Scheduled Jobs — DBMS_SCHEDULER

Oracle's tool for running procedures/PL/SQL blocks automatically on a schedule (e.g. nightly reports, materialized view refresh, cleanup jobs) — the DBA-level equivalent of a cron job.

```sql
BEGIN
    DBMS_SCHEDULER.CREATE_JOB (
        job_name        => 'REFRESH_SALES_SUMMARY',
        job_type        => 'PLSQL_BLOCK',
        job_action      => 'BEGIN DBMS_MVIEW.REFRESH(''MV_EMP_SUMMARY''); END;',
        start_date      => SYSTIMESTAMP,
        repeat_interval => 'FREQ=DAILY; BYHOUR=2; BYMINUTE=0',  -- every day at 2 AM
        enabled         => TRUE
    );
END;
/
```

```sql
-- Enable / disable / drop a job
EXEC DBMS_SCHEDULER.DISABLE('REFRESH_SALES_SUMMARY');
EXEC DBMS_SCHEDULER.ENABLE('REFRESH_SALES_SUMMARY');
EXEC DBMS_SCHEDULER.DROP_JOB('REFRESH_SALES_SUMMARY');

-- Run a job immediately, outside its schedule
EXEC DBMS_SCHEDULER.RUN_JOB('REFRESH_SALES_SUMMARY');

-- Check job status/history
SELECT job_name, state, next_run_date FROM user_scheduler_jobs;
SELECT job_name, status, actual_start_date FROM user_scheduler_job_run_details;
```

### Legacy alternative: DBMS_JOB (older, simpler, largely superseded)
```sql
DECLARE
    v_job NUMBER;
BEGIN
    DBMS_JOB.SUBMIT(v_job, 'give_raise(101, 5);', SYSDATE, 'SYSDATE + 1');  -- daily
    COMMIT;
END;
/
```

### Interview point
> `DBMS_SCHEDULER` (introduced in Oracle 10g) is the modern, feature-rich replacement for the older `DBMS_JOB` package — it supports more flexible calendar-style scheduling (`FREQ=DAILY; BYHOUR=2`), job chains, windows, and resource management, and is what's expected in current DBA/developer interviews.

[⬆ Back to top](#-table-of-contents)

---

## 31. Type Conversion — TO_CHAR, TO_DATE, TO_NUMBER, CAST

### TO_CHAR — Number/Date → String
```sql
SELECT TO_CHAR(1234567.891, '9,999,999.99') FROM dual;   -- '1,234,567.89'
SELECT TO_CHAR(45, '000') FROM dual;                       -- '045'
SELECT TO_CHAR(1250, '$9,999') FROM dual;                  -- '$1,250'

SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY') FROM dual;
SELECT TO_CHAR(SYSDATE, 'DD/MM/YYYY HH24:MI:SS') FROM dual;
SELECT TO_CHAR(SYSDATE, 'DAY') FROM dual;
SELECT TO_CHAR(SYSDATE, 'Month YYYY') FROM dual;
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

### TO_DATE — String → Date
```sql
SELECT TO_DATE('22-08-2026', 'DD-MM-YYYY') FROM dual;
SELECT TO_DATE('2026/08/22', 'YYYY/MM/DD') FROM dual;
```
> ⚠️ Mismatched format mask throws `ORA-01858` (not a valid month) or `ORA-01861` (literal does not match format string).

### TO_NUMBER — String → Number
```sql
SELECT TO_NUMBER('1,234.56', '9,999.99') FROM dual;   -- 1234.56
SELECT TO_NUMBER('$500', '$999') FROM dual;             -- 500
```

### CAST — ANSI-standard alternative
```sql
SELECT CAST('123' AS NUMBER) FROM dual;
SELECT CAST(sal AS VARCHAR2(20)) FROM emp;
SELECT CAST('2026-08-22' AS DATE) FROM dual;
```

### Implicit vs Explicit Conversion
```sql
-- Risky (implicit) — silently converts hiredate to a string for comparison
SELECT * FROM emp WHERE hiredate = '22-AUG-26';

-- Safe (explicit)
SELECT * FROM emp WHERE hiredate = TO_DATE('22-08-2026','DD-MM-YYYY');
```
> Implicit conversion can silently disable index usage on the converted column and can produce wrong results depending on session NLS date settings — always prefer explicit conversion in production code.

[⬆ Back to top](#-table-of-contents)

---

## 32. DBA-Level Concepts

### Tablespaces & Storage
```sql
CREATE TABLESPACE app_data
DATAFILE 'app_data01.dbf' SIZE 500M AUTOEXTEND ON;

CREATE TABLE emp (...) TABLESPACE app_data;
```

### Explain Plan
```sql
EXPLAIN PLAN FOR SELECT * FROM emp WHERE deptno = 10;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

### Redo Logs & Undo
| Concept | Purpose |
|---|---|
| Redo Log | Records changes for crash recovery/replay of committed work |
| Undo Segments | Store the "before" image so an uncommitted transaction can be rolled back; also power read consistency |
| Archive Log (archived redo) | Used for replication & point-in-time recovery |

### Locking & Concurrency
```sql
SELECT * FROM v$lock;
SELECT * FROM v$session WHERE blocking_session IS NOT NULL;
```
- **Row-level locking** — Oracle's default, locks only the specific rows being modified.
- **Deadlock** — Oracle automatically detects and rolls back one of the transactions.

### Backup & Recovery
| Tool | Purpose |
|---|---|
| `expdp` / `impdp` (Data Pump) | Logical export/import |
| RMAN (Recovery Manager) | Physical/hot backup, point-in-time recovery |

### Partitioning
```sql
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

### Roles
```sql
CREATE ROLE app_readonly;
GRANT SELECT ON emp TO app_readonly;
GRANT app_readonly TO ramesh;
```

### Data Dictionary Views
| Purpose | View |
|---|---|
| List all tables (current user) | `user_tables` |
| List all columns of a table | `user_tab_columns` |
| List constraints | `user_constraints` |
| List indexes | `user_indexes` |
| Currently running sessions | `v$session` |

[⬆ Back to top](#-table-of-contents)

---

## 33. 🔑 Interview Quick-Fire Points

- `DELETE` is DML (logged, rollback-able, uses WHERE); `TRUNCATE` is DDL (auto-commits, not rollback-able, no WHERE); `DROP` removes the table entirely.
- `WHERE` filters rows before grouping; `HAVING` filters groups after aggregation.
- `PRIMARY KEY` = `UNIQUE` + `NOT NULL`; only one PK per table.
- `VARCHAR2` is always preferred over legacy `VARCHAR` in Oracle.
- `UNION` removes duplicates (slower, implicit sort); `UNION ALL` keeps duplicates (faster).
- Correlated subqueries run once **per outer row**; plain subqueries run **once**.
- `RANK()` leaves gaps after ties; `DENSE_RANK()` does not; `ROW_NUMBER()` never ties.
- A `FUNCTION` must return exactly one value and can be called from SQL; a `PROCEDURE` is optional-return and cannot be called directly in a SELECT.
- `CONNECT BY ... START WITH` is Oracle's classic hierarchical query syntax; recursive `WITH` CTEs are the ANSI-standard modern alternative.
- `DBMS_SCHEDULER` is the modern replacement for the older `DBMS_JOB` package.
- Packages bundle related procedures/functions into one compiled unit with a public spec and private body.
- ACID = Atomicity, Consistency, Isolation, Durability.
- Normalization reduces redundancy; over-normalizing can hurt performance due to excessive JOINs.

[⬆ Back to top](#-table-of-contents)

---

## 34. 💼 Most Asked Interview Questions

**Q1: Difference between DELETE, TRUNCATE, and DROP?**
DELETE removes specific rows (DML, uses WHERE, rollback-able), TRUNCATE removes all rows but keeps structure (DDL, faster, not rollback-able), DROP removes the entire table object including structure.

**Q2: Difference between WHERE and HAVING?**
WHERE filters rows before grouping and can't use aggregate functions; HAVING filters grouped results and can use aggregates.

**Q3: Difference between a Procedure and a Function?**
A Function must return exactly one value via RETURN and can be called directly inside a SQL SELECT statement; a Procedure's return is optional (via OUT parameters) and cannot be called directly in SQL — only from PL/SQL or EXEC.

**Q4: What is a Package and why use one?**
A Package bundles related procedures, functions, cursors, and variables into one named unit with a public specification and a private body — improving organization, allowing private helper logic, and loading the whole unit into memory together for performance.

**Q5: Difference between CONNECT BY and recursive WITH CTE?**
CONNECT BY is Oracle's proprietary hierarchical query syntax with an automatic LEVEL pseudo-column; a recursive CTE is the ANSI-standard alternative using an anchor member UNION ALL a recursive member, requiring the level counter to be built manually.

**Q6: What is DBMS_SCHEDULER used for?**
It's Oracle's tool for automatically running procedures or PL/SQL blocks on a schedule (like a cron job) — supporting flexible calendar-style repeat intervals, job chains, and better management than the older DBMS_JOB package.

**Q7: What's a mutating table error and how do you avoid it?**
It occurs when a row-level trigger tries to query or modify the same table that fired it, which Oracle disallows to prevent inconsistent reads; a compound trigger (with separate BEFORE/AFTER STATEMENT and ROW sections) is one common way to work around it.

**Q8: What is normalization? Explain 1NF, 2NF, 3NF.**
Process of organizing data to reduce redundancy: 1NF requires atomic values, 2NF removes partial dependency on a composite key, 3NF removes transitive dependency between non-key columns.

**Q9: Difference between RANK, DENSE_RANK, and ROW_NUMBER?**
ROW_NUMBER always gives unique sequential numbers with no ties; RANK gives the same rank to ties but skips subsequent ranks; DENSE_RANK gives the same rank to ties with no gap.

**Q10: What happens if you PURGE a table that was never dropped?**
Oracle throws `ORA-38307: object not in RECYCLEBIN` — the table must be DROPped first (sending it to the recycle bin) before it can be PURGEd.

[⬆ Back to top](#-table-of-contents)

---

## 35. 🧪 Practice Questions — Hidden Solutions

> Click **"Show Solution"** under each question to reveal the theory answer and query. Try to answer first before expanding.

---

### Question 1 (Theory + Query)

**A company table `emp(empno, ename, sal, deptno, hiredate)` exists. Write a query to find the 2nd highest salary in the entire company, without using ROWNUM, and explain what happens if there are duplicate salaries at that rank.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** Using `ROW_NUMBER()` over the whole table (no PARTITION BY) numbers every row uniquely by salary, so filtering for `rn = 2` gives exactly one row for the 2nd highest salary — even if two employees are tied for 1st place, only one of them gets `rn = 1` (arbitrary among ties unless you add a secondary ORDER BY tiebreaker) and the other gets `rn = 2`, which would incorrectly report a tied 1st-place salary as "2nd highest." For salary-ranking questions where ties should be respected, `DENSE_RANK()` is usually the safer choice instead of `ROW_NUMBER()`.

**Query:**
```sql
SELECT sal AS second_highest_salary
FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS drnk
    FROM emp
)
WHERE drnk = 2;
```
</details>

---

### Question 2 (Theory)

**What is the difference between a `BEFORE INSERT` trigger and an `AFTER INSERT` trigger? Give one real scenario for each where using the wrong one would cause a bug.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** A `BEFORE INSERT` trigger fires before the row is physically written, so it can still modify `:NEW.column` values (e.g. auto-populate a default, validate/reject the insert with `RAISE_APPLICATION_ERROR`) before they're committed to the table. An `AFTER INSERT` trigger fires after the row already exists, so it's used for actions that depend on the row being final — like writing an audit log entry that references the row's already-assigned values, or cascading an insert into a related table.

**Scenario where using the wrong one causes a bug:**
- If you try to set `:NEW.hiredate := SYSDATE` inside an **AFTER INSERT** trigger, it has **no effect** — the row is already written, so modifying `:NEW` at that point is silently ignored (or in strict PL/SQL, raises an error depending on context) — this logic belongs in a `BEFORE INSERT` trigger.
- If you try to insert an audit log row referencing `:NEW.empno` inside a **BEFORE INSERT** trigger before validating that the row will actually succeed, you could end up logging an audit entry for an insert that later fails due to a constraint violation — audit logging is safer in `AFTER INSERT`, once the insert has actually succeeded.
</details>

---

### Question 3 (Query)

**Given `emp2(empno, ename, mgr)` representing an org chart, write a query that lists every employee along with the name of their top-most manager (the ultimate root of the hierarchy), using CONNECT BY.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** `CONNECT_BY_ROOT` is a special Oracle operator used inside a `CONNECT BY` hierarchical query — it returns the value of the specified column from the **root row** of the current row's hierarchy path, regardless of how many levels deep the current row is.

**Query:**
```sql
SELECT ename, CONNECT_BY_ROOT ename AS top_manager, LEVEL
FROM emp2
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr
ORDER BY LEVEL;
```

For the sample data (KING at the top, JONES/BLAKE reporting to KING, SCOTT reporting to JONES, ADAMS reporting to SCOTT), this returns ADAMS → KING, SCOTT → KING, JONES → KING, BLAKE → KING, and KING → KING (itself, since it is its own root).
</details>

---

### Question 4 (Theory + Query)

**Write a stored PROCEDURE that gives every employee in a given department a raise by a given percentage, but only commits the change if every single update in that department succeeds — if any row fails validation (e.g. resulting salary exceeds 100000), the entire batch should roll back. Explain which PL/SQL construct makes this possible.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** This requires wrapping the whole operation in a single transaction and using a `SAVEPOINT` combined with exception handling — if any row's validation fails inside the loop, you `RAISE` a custom exception, catch it in the `EXCEPTION` block, and `ROLLBACK` (or `ROLLBACK TO` a savepoint) instead of committing, so partial updates never persist. This gives the whole procedure atomic, all-or-nothing behavior even though it's looping over multiple rows individually.

**Query:**
```sql
CREATE OR REPLACE PROCEDURE bulk_raise (
    p_deptno IN emp.deptno%TYPE,
    p_pct    IN NUMBER
) IS
    v_new_sal emp.sal%TYPE;
    salary_too_high EXCEPTION;
BEGIN
    FOR rec IN (SELECT empno, sal FROM emp WHERE deptno = p_deptno) LOOP
        v_new_sal := rec.sal + (rec.sal * p_pct / 100);

        IF v_new_sal > 100000 THEN
            RAISE salary_too_high;
        END IF;

        UPDATE emp SET sal = v_new_sal WHERE empno = rec.empno;
    END LOOP;

    COMMIT;
    DBMS_OUTPUT.PUT_LINE('All raises applied successfully.');

EXCEPTION
    WHEN salary_too_high THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Raise batch rolled back: a salary would exceed the cap.');
    WHEN OTHERS THEN
        ROLLBACK;
        DBMS_OUTPUT.PUT_LINE('Raise batch rolled back due to unexpected error: ' || SQLERRM);
END bulk_raise;
/
```
</details>

---

### Question 5 (Theory)

**A junior developer complains that their recursive CTE query against a large hierarchy table never finishes running. What is the most likely cause, and how would you fix it using Oracle's recursive CTE syntax?**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** The most likely cause is a **cycle** in the data — somewhere in the hierarchy, a row's ancestor chain loops back on itself (e.g. row A points to row B as its parent, and row B's parent, several levels up, incorrectly points back to row A). A plain recursive CTE has no built-in protection against this and will loop indefinitely (or until it hits `ORA-32044: cycle detected while executing recursive WITH query`, depending on Oracle version and settings), unlike `CONNECT BY`, which has the explicit `NOCYCLE` keyword. The fix is to add a `CYCLE` clause to the recursive CTE, which detects when a row would revisit an ancestor already seen on its path and stops recursing further down that branch instead of looping forever.

**Query:**
```sql
WITH emp_tree (empno, ename, mgr, lvl) AS (
    SELECT empno, ename, mgr, 1
    FROM emp2
    WHERE mgr IS NULL

    UNION ALL

    SELECT e.empno, e.ename, e.mgr, t.lvl + 1
    FROM emp2 e
    JOIN emp_tree t ON e.mgr = t.empno
)
CYCLE empno SET is_cycle TO 'Y' DEFAULT 'N'
SELECT lvl, ename, is_cycle
FROM emp_tree;
```
The `CYCLE empno SET is_cycle TO 'Y' DEFAULT 'N'` clause flags any row where `empno` reappears in its own ancestor path with `is_cycle = 'Y'` and stops recursing past that point, instead of looping indefinitely.
</details>

---

[⬆ Back to top](#-table-of-contents)
