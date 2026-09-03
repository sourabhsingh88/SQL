# 🐬 MySQL – Complete Interview Notes

Concise MySQL-only reference, mirrored against the Oracle notes structure. Where MySQL behavior is identical to Oracle, it's marked **(same as Oracle)** with no repeat explanation — full detail is given only where MySQL differs or needs its own coverage (especially **Window Functions**, which get a complete dedicated section).

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)
2. [Data Types](#2-data-types)
3. [Constraints](#3-constraints)
4. [DDL](#4-ddl)
5. [DML](#5-dml)
6. [DQL — Selection & Projection](#6-dql--selection--projection)
7. [Functions](#7-functions)
8. [Joins](#8-joins)
9. [Operators](#9-operators)
10. [CASE & IF](#10-case--if)
11. [Subqueries, Correlated Subqueries & LIMIT](#11-subqueries-correlated-subqueries--limit)
12. [WHERE, GROUP BY, HAVING, ORDER BY](#12-where-group-by-having-order-by)
13. [TCL — Transactions](#13-tcl--transactions)
14. [DCL](#14-dcl)
15. [CTAS & Users](#15-ctas--users)
16. [CLI Commands](#16-cli-commands)
17. [Views](#17-views)
18. [AUTO_INCREMENT](#18-auto_increment)
19. [🪟 Window Functions — Complete](#19--window-functions--complete)
20. [Triggers](#20-triggers)
21. [Indexing](#21-indexing)
22. [Normalization](#22-normalization)
23. [LPAD/RPAD](#23-lpadrpad)
24. [Hierarchical Data — Recursive CTE](#24-hierarchical-data--recursive-cte)
25. [Stored Procedures & Functions](#25-stored-procedures--functions)
26. [Events (Scheduler)](#26-events-scheduler)
27. [Type Conversion](#27-type-conversion)
28. [DBA-Level Concepts](#28-dba-level-concepts)
29. [🔑 Interview Quick-Fire Points](#29--interview-quick-fire-points)
30. [💼 Most Asked Interview Questions](#30--most-asked-interview-questions)
31. [🧪 Practice Questions — Hidden Solutions](#31--practice-questions--hidden-solutions)

---

## 1. Introduction

**MySQL** is a free, open-source Relational Database Management System (RDBMS), owned by Oracle Corporation since 2010. It uses standard SQL with its own vendor-specific extensions — similar in core concepts to Oracle Database, but simpler in licensing/setup and widely used in web applications (the "M" in the LAMP stack).

- Uses **storage engines** — most important: **InnoDB** (default, supports transactions, foreign keys, row-level locking) and **MyISAM** (older, no transactions, table-level locking only).
- **autocommit is ON by default** — every statement commits immediately unless you explicitly `START TRANSACTION`. (Oracle requires explicit COMMIT.)
- No recycle bin, no FLASHBACK/PURGE — `DROP TABLE` is immediate and permanent.

[⬆ Back to top](#-table-of-contents)

---

## 2. Data Types

| Category | Types |
|---|---|
| Fixed-length string | `CHAR(n)` — max 255 |
| Variable-length string | `VARCHAR(n)` — main string type, up to 65,535 bytes shared per row |
| Whole numbers | `TINYINT`, `SMALLINT`, `INT`, `BIGINT` |
| Decimal numbers | `DECIMAL(p,s)` / `NUMERIC(p,s)` |
| Floating point | `FLOAT`, `DOUBLE` |
| Date only | `DATE` (`YYYY-MM-DD`) |
| Date + time | `DATETIME`, `TIMESTAMP` (timezone-aware, auto-updates) |
| Large text | `TEXT`, `LONGTEXT` |
| Binary large object | `BLOB`, `LONGBLOB` |
| Structured data | `JSON` (native type, no Oracle equivalent) |

```sql
CREATE TABLE emp (
    empno   INT AUTO_INCREMENT PRIMARY KEY,
    ename   VARCHAR(30) NOT NULL,
    sal     DECIMAL(8,2),
    hiredate DATE,
    metadata JSON
);
```

> No `VARCHAR2` in MySQL — just use `VARCHAR`. No `NUMBER` type either — use `INT`/`DECIMAL` depending on need.

[⬆ Back to top](#-table-of-contents)

---

## 3. Constraints

`UNIQUE`, `NOT NULL`, `PRIMARY KEY`, `CHECK`, `DEFAULT`, `FOREIGN KEY` — **(same as Oracle)** in purpose and basic syntax.

```sql
CREATE TABLE department (
    deptno INT PRIMARY KEY,
    dname  VARCHAR(30)
);

CREATE TABLE employee (
    empno  INT PRIMARY KEY,
    ename  VARCHAR(30) NOT NULL,
    email  VARCHAR(50) UNIQUE,
    age    INT CHECK(age >= 18),
    status VARCHAR(20) DEFAULT 'ACTIVE',
    deptno INT,
    FOREIGN KEY (deptno) REFERENCES department(deptno)
);
```

⚠️ **Two real differences:**
- `CHECK` constraints were silently ignored before MySQL 8.0.16 — fully enforced from 8.0.16 onward.
- `FOREIGN KEY` is only enforced on the **InnoDB** engine — MyISAM ignores it silently.

[⬆ Back to top](#-table-of-contents)

---

## 4. DDL

`CREATE`, `ALTER`, `TRUNCATE`, `DROP` — **(same as Oracle)** in purpose.

```sql
CREATE TABLE stud (sid INT PRIMARY KEY, name VARCHAR(20));

RENAME TABLE stud TO student;             -- MySQL syntax (not just RENAME oldname TO newname)

ALTER TABLE stud ADD phno VARCHAR(10);
ALTER TABLE stud MODIFY COLUMN email VARCHAR(25);
ALTER TABLE stud CHANGE email email_addr VARCHAR(25);  -- rename + retype in one step
ALTER TABLE stud DROP COLUMN email;

TRUNCATE TABLE stud;   -- also resets AUTO_INCREMENT back to 1
DROP TABLE stud;       -- immediate, permanent — no recycle bin, no FLASHBACK, no PURGE
```

> ❌ No `FLASHBACK`, no `PURGE`, no recycle bin at all — once dropped, recovery relies entirely on backups/binary logs.

[⬆ Back to top](#-table-of-contents)

---

## 5. DML

**(same as Oracle)** for `INSERT`, `UPDATE`, `DELETE` — always use `WHERE` for UPDATE/DELETE.

```sql
INSERT INTO stud VALUES (101, 'ramesh');
INSERT INTO stud SET sid = 102, name = 'ramya';   -- MySQL-only shorthand, no Oracle equivalent

UPDATE emp SET sal = sal * 1.10 WHERE job = 'CLERK';
DELETE FROM emp WHERE job = 'CLERK';
```

[⬆ Back to top](#-table-of-contents)

---

## 6. DQL — Selection & Projection

**(same as Oracle)** — `SELECT`, `DISTINCT`, aliasing, computed columns, execution order `FROM → WHERE → SELECT`.

```sql
SELECT DISTINCT sal FROM emp;
SELECT ename, (sal*12)/4 AS quarterly_sal FROM emp;
SELECT ename AS `Employee Name` FROM emp;   -- backticks for identifiers with spaces (Oracle uses double quotes)
```

[⬆ Back to top](#-table-of-contents)

---

## 7. Functions

### String — mostly **(same as Oracle)**, key differences noted:

```sql
UPPER('raju'); LOWER('RAJU');                  -- same as Oracle
SUBSTRING('Manu',1,3);                          -- Oracle's SUBSTR also works, SUBSTRING is standard
CONCAT('Hi:', ename);                            -- same name — but || does NOT work by default in MySQL, must use CONCAT()
REVERSE('FORD');                                  -- same as Oracle
LOCATE('A','NAYAN',1);                            -- Oracle's 4-arg INSTR only supports 2 args here; use LOCATE for position+nth
TRIM(LEADING 'P' FROM 'PUSHPA');                  -- same as Oracle
REPLACE('BANGLORE','B','M');                      -- same as Oracle
```
> ❌ `INITCAP` doesn't exist — build manually: `CONCAT(UPPER(LEFT(str,1)), LOWER(SUBSTRING(str,2)))`.
> No `FROM dual` requirement — `SELECT UPPER('raju');` works directly.

### Number — **(same as Oracle)** except:
```sql
TRUNCATE(45.926, 2);   -- Oracle's TRUNC(n,d) is spelled TRUNCATE(n,d) here (extra letters)
MOD(11,2); SQRT(100); POWER(2,3); ROUND(45.926,2); CEIL(4.1); FLOOR(4.9); ABS(-7); SIGN(-45);  -- all same
```

### Date — differs significantly from Oracle:
```sql
NOW();                                    -- like SYSDATE
CURDATE();                                -- date only, no time
DATE_ADD(hiredate, INTERVAL 30 DAY);      -- like hiredate + 30 in Oracle
DATE_SUB(hiredate, INTERVAL 1 MONTH);
DATEDIFF(NOW(), hiredate);                -- like SYSDATE - hiredate (days between)
TIMESTAMPDIFF(MONTH, hiredate, NOW());    -- like MONTHS_BETWEEN
LAST_DAY(NOW());                          -- same name as Oracle
DAYNAME(NOW()); MONTHNAME(NOW());
```

### Conversion — different function names, see [Section 27](#27-type-conversion).

### Aggregate — **(same as Oracle)**: `SUM`, `AVG`, `COUNT`, `MAX`, `MIN`. `ONLY_FULL_GROUP_BY` mode (on by default since 5.7.5) enforces the same "non-aggregated column must be in GROUP BY" rule Oracle always had.

[⬆ Back to top](#-table-of-contents)

---

## 8. Joins

`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `CROSS JOIN` — **(same as Oracle)**.

```sql
SELECT e.ename, d.dname FROM emp e INNER JOIN dept d ON e.deptno = d.deptno;
SELECT e.ename, d.dname FROM emp e LEFT JOIN dept d ON e.deptno = d.deptno;
```

❌ **No `FULL OUTER JOIN`** — simulate with `UNION`:
```sql
SELECT e.ename, d.dname FROM emp e LEFT JOIN dept d ON e.deptno=d.deptno
UNION
SELECT e.ename, d.dname FROM emp e RIGHT JOIN dept d ON e.deptno=d.deptno;
```

❌ **No `(+)` outer-join operator** — always use ANSI `JOIN ... ON` syntax, no old-style equivalent exists.

Self joins — **(same as Oracle)** for a single level:
```sql
SELECT e.ename AS employee, m.ename AS manager
FROM emp e LEFT JOIN emp m ON e.mgr = m.empno;
```
For multi-level hierarchy traversal, see [Section 24](#24-hierarchical-data--recursive-cte) — no `CONNECT BY` in MySQL.

[⬆ Back to top](#-table-of-contents)

---

## 9. Operators

`+ - * /`, `= != <> < > <= >=`, `AND OR NOT`, `BETWEEN`, `IN`/`NOT IN`, `LIKE`, `IS NULL` — **(same as Oracle)**.

⚠️ `LIKE` is **case-insensitive by default** in MySQL (depends on collation); Oracle's is case-sensitive by default.

```sql
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;
SELECT * FROM emp WHERE ename LIKE 'S%';
```

### Set Operators
```sql
SELECT deptno FROM emp UNION SELECT deptno FROM dept;         -- same as Oracle
SELECT deptno FROM emp UNION ALL SELECT deptno FROM dept;     -- same as Oracle
SELECT empno FROM emp INTERSECT SELECT empno FROM emp_backup; -- 8.0.31+ only
SELECT empno FROM emp EXCEPT SELECT empno FROM emp_backup;    -- Oracle's MINUS = MySQL's EXCEPT (8.0.31+ only)
```

[⬆ Back to top](#-table-of-contents)

---

## 10. CASE & IF

`CASE WHEN ... THEN ... ELSE ... END` — **(same as Oracle)**, ANSI standard.

```sql
SELECT ename, sal,
  CASE WHEN sal >= 5000 THEN 'HIGH' WHEN sal >= 2000 THEN 'MEDIUM' ELSE 'LOW' END AS grade
FROM emp;
```

❌ No `DECODE` — always use `CASE`. ✅ MySQL has its own shorthand `IF()` (Oracle has no equivalent):
```sql
SELECT ename, IF(sal >= 3000, 'HIGH', 'LOW') AS grade FROM emp;
```

[⬆ Back to top](#-table-of-contents)

---

## 11. Subqueries, Correlated Subqueries & LIMIT

Single-row, multi-row (`IN`/`ANY`/`ALL`), inline views, correlated subqueries, `EXISTS` — **(same as Oracle)** in syntax and behavior.

```sql
SELECT ename, sal FROM emp WHERE sal > (SELECT AVG(sal) FROM emp);

SELECT e.ename, e.sal FROM emp e
WHERE e.sal > (SELECT AVG(sal) FROM emp WHERE deptno = e.deptno);
```

❌ **No `ROWNUM`** — MySQL uses `LIMIT` instead (much simpler, no subquery-wrap trap):
```sql
SELECT * FROM emp ORDER BY sal DESC LIMIT 3;
SELECT * FROM emp LIMIT 5 OFFSET 10;   -- skip 10, take next 5
```

[⬆ Back to top](#-table-of-contents)

---

## 12. WHERE, GROUP BY, HAVING, ORDER BY

Execution order `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY` — **(same as Oracle)**.

```sql
SELECT deptno, COUNT(*) AS emp_count
FROM emp WHERE sal > 2000
GROUP BY deptno HAVING COUNT(*) > 3
ORDER BY emp_count DESC;
```

⚠️ MySQL quirk: you **can** reference a `SELECT`-clause alias inside `GROUP BY`/`HAVING` (e.g. `HAVING emp_count > 3`), which Oracle generally does not allow.

[⬆ Back to top](#-table-of-contents)

---

## 13. TCL — Transactions

`COMMIT`, `ROLLBACK`, `SAVEPOINT`, `ROLLBACK TO` — **(same as Oracle)** syntax, but:

⚠️ Only works on **InnoDB** tables — MyISAM has no transaction support at all.
⚠️ `autocommit = 1` by default — every statement commits immediately unless you `START TRANSACTION` first.

```sql
START TRANSACTION;
UPDATE emp SET sal = sal + 500 WHERE deptno = 10;
SAVEPOINT sp1;
DELETE FROM emp WHERE deptno = 20;
ROLLBACK TO sp1;
COMMIT;
```

ACID properties — **(same as Oracle)**: Atomicity, Consistency, Isolation, Durability.

[⬆ Back to top](#-table-of-contents)

---

## 14. DCL

Purpose **(same as Oracle)** — `GRANT`/`REVOKE`. Syntax requires a **host**:

```sql
GRANT SELECT, INSERT ON db1.emp TO 'ramesh'@'localhost';
REVOKE INSERT ON db1.emp FROM 'ramesh'@'localhost';
FLUSH PRIVILEGES;   -- refresh cached grant tables (safety habit)
```

[⬆ Back to top](#-table-of-contents)

---

## 15. CTAS & Users

CTAS — **(same as Oracle)**, constraints not copied except NOT NULL:
```sql
CREATE TABLE emp_backup AS SELECT * FROM emp;
```

```sql
CREATE USER 'ramesh'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON db1.* TO 'ramesh'@'localhost';
FLUSH PRIVILEGES;
```

[⬆ Back to top](#-table-of-contents)

---

## 16. CLI Commands

| Purpose | MySQL |
|---|---|
| Describe table | `DESCRIBE tabname;` or `DESC tabname;` |
| List all columns | `SHOW COLUMNS FROM tabname;` |
| List all tables | `SHOW TABLES;` |
| Save output to file | `mysql -e "query" > file.txt` (shell) or `SELECT ... INTO OUTFILE 'path'` |
| Current user | `SELECT CURRENT_USER();` |

> No `SPOOL`, `SET LINESIZE`/`PAGESIZE` — those are Oracle SQL*Plus-only formatting commands.

[⬆ Back to top](#-table-of-contents)

---

## 17. Views

`CREATE VIEW`, `CREATE OR REPLACE VIEW`, `DROP VIEW` — **(same as Oracle)** in syntax and updatability limitations.

```sql
CREATE VIEW emp_view AS SELECT empno, ename, sal FROM emp WHERE deptno = 10;
```

❌ **No native Materialized View** — must simulate with a real table + a scheduled `EVENT` (see [Section 26](#26-events-scheduler)) that periodically refreshes it.

[⬆ Back to top](#-table-of-contents)

---

## 18. AUTO_INCREMENT

MySQL has **no `CREATE SEQUENCE` object** — it uses `AUTO_INCREMENT` directly on a column instead (tied to one table, not standalone/reusable).

```sql
CREATE TABLE emp (
    empno INT AUTO_INCREMENT PRIMARY KEY,
    ename VARCHAR(30)
);

INSERT INTO emp(ename) VALUES ('Ramesh');   -- empno generated automatically
SELECT LAST_INSERT_ID();                     -- like Oracle's .CURRVAL

ALTER TABLE emp AUTO_INCREMENT = 500;        -- sets the next value to be generated
```

[⬆ Back to top](#-table-of-contents)

---

## 19. 🪟 Window Functions — Complete

Window functions perform a calculation across a set of rows (the "window") related to the current row, **without collapsing rows** like `GROUP BY` does. Available from **MySQL 8.0 onward only** (5.7 and earlier have none).

### General syntax
```sql
function_name(...) OVER (
    [PARTITION BY col1, col2, ...]
    [ORDER BY col3, col4, ...]
    [frame_clause]
)
```

### A. Ranking functions

```sql
SELECT ename, deptno, sal,
  ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rn,
  RANK()       OVER (PARTITION BY deptno ORDER BY sal DESC) AS rnk,
  DENSE_RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS drnk
FROM emp;
```
- **`ROW_NUMBER()`** — unique sequential number, no ties.
- **`RANK()`** — same rank for ties, skips subsequent rank(s) (gap).
- **`DENSE_RANK()`** — same rank for ties, no gap.

```sql
-- Nth highest salary per department
SELECT * FROM (
    SELECT ename, deptno, sal, ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rn
    FROM emp
) t WHERE rn = 2;
```

### B. NTILE — divide rows into N buckets

```sql
SELECT ename, sal, NTILE(4) OVER (ORDER BY sal DESC) AS quartile
FROM emp;
```
Splits all rows as evenly as possible into 4 groups (quartiles) based on the ORDER BY — commonly used for percentile-style bucketing (e.g. "top 25% earners").

### C. Offset functions — LAG / LEAD

```sql
SELECT ename, hiredate, sal,
  LAG(sal, 1)  OVER (ORDER BY hiredate) AS prev_sal,
  LEAD(sal, 1) OVER (ORDER BY hiredate) AS next_sal
FROM emp;
```
- **`LAG(col, n, default)`** — value from `n` rows **before** the current row (default `n=1`).
- **`LEAD(col, n, default)`** — value from `n` rows **after** the current row.
- Optional third argument sets a default when there's no such row (e.g. `LAG(sal, 1, 0)`).

```sql
-- Month-over-month salary change
SELECT ename, hiredate, sal,
  sal - LAG(sal) OVER (ORDER BY hiredate) AS change_from_prev
FROM emp;
```

### D. First/Last/Nth value in the window

```sql
SELECT ename, deptno, sal,
  FIRST_VALUE(ename) OVER (PARTITION BY deptno ORDER BY sal DESC) AS top_earner,
  LAST_VALUE(ename) OVER (
      PARTITION BY deptno ORDER BY sal DESC
      ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS lowest_earner,
  NTH_VALUE(ename, 2) OVER (
      PARTITION BY deptno ORDER BY sal DESC
      ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS second_earner
FROM emp;
```
> ⚠️ `LAST_VALUE` needs an explicit frame (`ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`) — without it, the default frame only looks at rows up to the *current* row, so `LAST_VALUE` would just return the current row's own value, not the true last value in the partition. This is one of the most common window-function interview traps.

### E. Aggregate functions used as window functions

Any aggregate (`SUM`, `AVG`, `COUNT`, `MAX`, `MIN`) can be turned into a window function by adding `OVER(...)` — this is the key feature that avoids collapsing rows the way `GROUP BY` does.

```sql
SELECT ename, deptno, sal,
  SUM(sal) OVER (PARTITION BY deptno) AS dept_total,
  AVG(sal) OVER (PARTITION BY deptno) AS dept_avg,
  sal - AVG(sal) OVER (PARTITION BY deptno) AS diff_from_avg,
  COUNT(*) OVER (PARTITION BY deptno) AS dept_headcount
FROM emp;
```
Every row keeps its own identity (unlike `GROUP BY deptno`, which would collapse to one row per department) while still showing department-level aggregates alongside it — extremely common in reporting queries.

**Running total (cumulative sum):**
```sql
SELECT ename, hiredate, sal,
  SUM(sal) OVER (ORDER BY hiredate ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM emp;
```

**Moving average (last 3 rows including current):**
```sql
SELECT ename, hiredate, sal,
  AVG(sal) OVER (ORDER BY hiredate ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg_3
FROM emp;
```

### F. Distribution functions — CUME_DIST, PERCENT_RANK

```sql
SELECT ename, sal,
  CUME_DIST()    OVER (ORDER BY sal) AS cumulative_distribution,
  PERCENT_RANK() OVER (ORDER BY sal) AS percent_rank
FROM emp;
```
- **`CUME_DIST()`** — the proportion of rows with a value ≤ (or partition-relative rank ≤) the current row's value; range `(0, 1]`.
- **`PERCENT_RANK()`** — relative rank of the current row as a percentage, computed as `(rank - 1) / (total_rows - 1)`; range `[0, 1]`, always starts at 0 for the first row.

### G. Frame clauses — ROWS vs RANGE

The **frame** defines exactly which rows within the partition are included in the calculation for each row.

| Clause | Meaning |
|---|---|
| `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` | From the start of the partition to the current row (running total) |
| `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING` | Current row plus one before and one after |
| `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` | The entire partition (needed for correct `LAST_VALUE`) |
| `RANGE BETWEEN ...` | Same idea, but groups by **value** ties in ORDER BY instead of physical row position |

```sql
-- ROWS counts physical rows; RANGE groups logical peers with the same ORDER BY value
SELECT ename, sal,
  SUM(sal) OVER (ORDER BY sal ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS rows_total,
  SUM(sal) OVER (ORDER BY sal RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS range_total
FROM emp;
```
If two employees tie on `sal`, `ROWS` treats them as two separate steps in the running total, while `RANGE` includes **all** tied rows together at once — a subtle but real interview distinction.

### Window Functions Summary Table

| Function | Purpose |
|---|---|
| `ROW_NUMBER()` | Unique sequential number, no ties |
| `RANK()` | Ranking with gaps after ties |
| `DENSE_RANK()` | Ranking without gaps after ties |
| `NTILE(n)` | Splits rows into n roughly-equal buckets |
| `LAG(col, n, default)` | Value from n rows before |
| `LEAD(col, n, default)` | Value from n rows after |
| `FIRST_VALUE(col)` | First value in the window frame |
| `LAST_VALUE(col)` | Last value in the window frame (needs explicit frame!) |
| `NTH_VALUE(col, n)` | Nth value in the window frame |
| `SUM/AVG/COUNT/MAX/MIN(col) OVER(...)` | Aggregate without collapsing rows |
| `CUME_DIST()` | Cumulative distribution |
| `PERCENT_RANK()` | Relative percentile rank |

[⬆ Back to top](#-table-of-contents)

---

## 20. Triggers

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

```sql
-- Audit trigger
DELIMITER //
CREATE TRIGGER trg_audit_sal
AFTER UPDATE ON emp
FOR EACH ROW
BEGIN
    IF NEW.sal <> OLD.sal THEN
        INSERT INTO sal_audit(empno, old_sal, new_sal, changed_on)
        VALUES (OLD.empno, OLD.sal, NEW.sal, NOW());
    END IF;
END//
DELIMITER ;
```

Key points vs Oracle:
- `OLD.col` / `NEW.col` — **no colon prefix** (Oracle uses `:OLD`/`:NEW`).
- `DELIMITER //` switch needed before, `DELIMITER ;` after — since `;` is used inside the trigger body.
- **Every trigger is row-level** — `FOR EACH ROW` is mandatory, no statement-level trigger option at all.
- No column-specific trigger clause (`AFTER UPDATE OF sal`) — must check `IF NEW.sal <> OLD.sal` manually inside the body, as shown above.
- No `INSTEAD OF` triggers, no compound triggers, no DDL/database-event triggers — MySQL only supports DML row-level triggers (`BEFORE`/`AFTER` `INSERT`/`UPDATE`/`DELETE`).

```sql
DROP TRIGGER trg_before_insert;
SHOW TRIGGERS;
```

[⬆ Back to top](#-table-of-contents)

---

## 21. Indexing

`CREATE INDEX`, `CREATE UNIQUE INDEX` — **(same as Oracle)**. `DROP INDEX` needs the table name:
```sql
CREATE INDEX idx_ename ON emp(ename);
DROP INDEX idx_ename ON emp;   -- table name required (Oracle just needs the index name)
```

Default is **B-Tree**. MySQL also supports **FULLTEXT** indexes (natural-language text search) and **HASH** indexes (mainly Memory engine). ❌ No Bitmap indexes (Oracle-only, data-warehousing feature).

```sql
CREATE FULLTEXT INDEX idx_ft_desc ON products(description);
SELECT * FROM products WHERE MATCH(description) AGAINST('wireless headphones');
```

[⬆ Back to top](#-table-of-contents)

---

## 22. Normalization

1NF, 2NF, 3NF, BCNF — **(same as Oracle)**, pure relational theory, not vendor-specific.

[⬆ Back to top](#-table-of-contents)

---

## 23. LPAD/RPAD

**(same as Oracle)** — identical syntax and behavior, including the truncate-if-longer rule.

```sql
SELECT LPAD('45', 5, '0');    -- '00045'
SELECT LPAD(empno, 6, '0');   -- zero-padded employee code
```

[⬆ Back to top](#-table-of-contents)

---

## 24. Hierarchical Data — Recursive CTE

❌ **No `CONNECT BY`/`START WITH`/`LEVEL`/`PRIOR`** — those are 100% Oracle-only. MySQL (8.0+) uses a **recursive CTE** for all tree/hierarchy traversal.

```sql
CREATE TABLE emp2 (empno INT PRIMARY KEY, ename VARCHAR(20), mgr INT);
```

```sql
WITH RECURSIVE org_chart AS (
    -- anchor: root row(s)
    SELECT empno, ename, mgr, 1 AS lvl, CAST(ename AS CHAR(200)) AS path
    FROM emp2
    WHERE mgr IS NULL

    UNION ALL

    -- recursive step: join children to their parent's row already in the CTE
    SELECT e.empno, e.ename, e.mgr, o.lvl + 1, CONCAT(o.path, ' > ', e.ename)
    FROM emp2 e
    JOIN org_chart o ON e.mgr = o.empno
)
SELECT lvl, LPAD('', (lvl-1)*2, ' ') || ename AS indented_name, path
FROM org_chart
ORDER BY lvl;
```

There's no automatic `LEVEL` — you build the depth counter (`lvl + 1`) yourself in the recursive step, and no `SYS_CONNECT_BY_PATH` — build the path manually with `CONCAT()`.

**Cycle protection** — no `NOCYCLE` keyword; guard manually, e.g. by capping `lvl` or tracking visited IDs in the path:
```sql
WITH RECURSIVE org_chart AS (
    SELECT empno, mgr, 1 AS lvl FROM emp2 WHERE mgr IS NULL
    UNION ALL
    SELECT e.empno, e.mgr, o.lvl + 1
    FROM emp2 e JOIN org_chart o ON e.mgr = o.empno
    WHERE o.lvl < 50   -- safety cap to avoid infinite recursion on bad data
)
SELECT * FROM org_chart;
```

[⬆ Back to top](#-table-of-contents)

---

## 25. Stored Procedures & Functions

### Procedure
```sql
DELIMITER //
CREATE PROCEDURE give_raise (
    IN p_empno INT,
    IN p_pct   DECIMAL(5,2)
)
BEGIN
    UPDATE emp SET sal = sal + (sal * p_pct / 100) WHERE empno = p_empno;

    IF ROW_COUNT() = 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'No employee found with that empno';
    END IF;
END//
DELIMITER ;

CALL give_raise(101, 10);
```

### Procedure with OUT parameter
```sql
DELIMITER //
CREATE PROCEDURE get_emp_details (
    IN  p_empno INT,
    OUT p_ename VARCHAR(30),
    OUT p_sal   DECIMAL(8,2)
)
BEGIN
    SELECT ename, sal INTO p_ename, p_sal FROM emp WHERE empno = p_empno;
END//
DELIMITER ;

CALL get_emp_details(101, @name, @sal);
SELECT @name, @sal;
```

### Function
```sql
DELIMITER //
CREATE FUNCTION get_annual_salary(p_empno INT)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE v_sal DECIMAL(8,2);
    SELECT sal INTO v_sal FROM emp WHERE empno = p_empno;
    RETURN v_sal * 12;
END//
DELIMITER ;

SELECT ename, get_annual_salary(empno) AS annual_sal FROM emp;
```
> `DETERMINISTIC` tells MySQL the function always returns the same result for the same input — required by some settings (`binlog` safety) and good practice.

### Procedure vs Function

| Feature | Procedure | Function |
|---|---|---|
| Returns a value | Optional (via `OUT` params) | Mandatory (one, via `RETURNS`) |
| Callable directly in SQL | No — use `CALL` | Yes — usable in `SELECT` |
| Parameter modes | `IN`, `OUT`, `INOUT` | `IN` only |

### Cursor
```sql
DELIMITER //
CREATE PROCEDURE list_dept_emps(IN p_deptno INT)
BEGIN
    DECLARE v_ename VARCHAR(30);
    DECLARE v_sal DECIMAL(8,2);
    DECLARE done INT DEFAULT FALSE;
    DECLARE emp_cur CURSOR FOR SELECT ename, sal FROM emp WHERE deptno = p_deptno;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

    OPEN emp_cur;
    read_loop: LOOP
        FETCH emp_cur INTO v_ename, v_sal;
        IF done THEN
            LEAVE read_loop;
        END IF;
        SELECT CONCAT(v_ename, ' - ', v_sal);
    END LOOP;
    CLOSE emp_cur;
END//
DELIMITER ;
```
> No implicit cursor `FOR` loop like Oracle's `FOR rec IN (SELECT...) LOOP` — MySQL always requires explicit `DECLARE CURSOR`, `OPEN`, `FETCH`, and a `NOT FOUND` handler to detect the end.

[⬆ Back to top](#-table-of-contents)

---

## 26. Events (Scheduler)

MySQL's equivalent of Oracle's `DBMS_SCHEDULER` — runs SQL/procedures automatically on a schedule.

```sql
SET GLOBAL event_scheduler = ON;   -- must be enabled once (OFF by default)

CREATE EVENT refresh_sales_summary
ON SCHEDULE EVERY 1 DAY STARTS '2026-01-01 02:00:00'
DO
  CALL refresh_summary_table();

-- One-time event
CREATE EVENT cleanup_temp
ON SCHEDULE AT '2026-12-31 23:59:00'
DO
  DELETE FROM temp_logs WHERE created_at < NOW() - INTERVAL 30 DAY;

ALTER EVENT refresh_sales_summary DISABLE;
ALTER EVENT refresh_sales_summary ENABLE;
DROP EVENT refresh_sales_summary;

SHOW EVENTS;
```

[⬆ Back to top](#-table-of-contents)

---

## 27. Type Conversion

| Purpose | MySQL |
|---|---|
| Number/Date → String | `DATE_FORMAT(date, '%d-%b-%Y')` |
| String → Date | `STR_TO_DATE('22-08-2026', '%d-%m-%Y')` |
| String → Number | `CAST(str AS SIGNED)` / `CAST(str AS DECIMAL(p,s))`, or informally `str + 0` |
| General cast | `CAST(val AS type)` — target types: `CHAR`, `DATE`, `DATETIME`, `DECIMAL`, `SIGNED`, `UNSIGNED`, `BINARY`, `TIME`, `JSON` |

```sql
SELECT DATE_FORMAT(NOW(), '%d-%b-%Y');       -- '22-Aug-2026'
SELECT STR_TO_DATE('22-08-2026', '%d-%m-%Y');
SELECT CAST('123' AS SIGNED);
SELECT '123' + 0;                              -- quick informal cast trick
```

Format codes are **percent-prefixed** (`%Y %m %d %H %i %s`) — different letters from Oracle's `YYYY MM DD HH24 MI SS`.

⚠️ Implicit conversion (comparing a numeric column to a string literal, etc.) is just as risky here as in Oracle — it can silently disable index usage and depends on the current SQL mode. Prefer explicit conversion in production queries.

[⬆ Back to top](#-table-of-contents)

---

## 28. DBA-Level Concepts

| Concept | MySQL |
|---|---|
| Execution plan | `EXPLAIN SELECT ...;` or `EXPLAIN ANALYZE SELECT ...;` (8.0.18+) |
| Storage engine | **InnoDB** (default — transactions, FKs, row locks) vs **MyISAM** (no transactions, table locks) |
| Crash recovery log | InnoDB's own redo log |
| Rollback support | InnoDB's undo log |
| Replication / PITR | Binary Log (`binlog`) |
| Logical backup | `mysqldump`, `mysqlpump` |
| Physical/hot backup | `xtrabackup` (Percona), `mysqlbackup` |
| Roles (grouped privileges) | Supported from **8.0 only** — earlier versions grant individually per user |
| List all tables | `information_schema.tables` or `SHOW TABLES;` |
| List columns | `information_schema.columns` or `SHOW COLUMNS FROM tbl;` |
| Running queries/locks | `SHOW PROCESSLIST;`, `information_schema.innodb_trx`, `SHOW ENGINE INNODB STATUS;` |
| Partitioning | `PARTITION BY RANGE/LIST/HASH/KEY` — supported, but partition column generally must be part of every unique key including PK |

```sql
CREATE ROLE 'app_readonly';
GRANT SELECT ON db1.emp TO 'app_readonly';
GRANT 'app_readonly' TO 'ramesh'@'localhost';
SET DEFAULT ROLE 'app_readonly' TO 'ramesh'@'localhost';
```

[⬆ Back to top](#-table-of-contents)

---

## 29. 🔑 Interview Quick-Fire Points

- `autocommit = 1` by default in MySQL — Oracle requires explicit COMMIT.
- Only **InnoDB** supports transactions/FKs/row-locking — MyISAM supports neither.
- No `SEQUENCE` object — `AUTO_INCREMENT` is tied to a single column/table instead.
- No `ROWNUM` — use `LIMIT`/`LIMIT OFFSET`.
- No `FULL OUTER JOIN` — simulate with `UNION` of LEFT and RIGHT joins.
- No `DECODE` — always use `CASE` (or MySQL's own `IF()` for simple two-way conditions).
- No `CONNECT BY`/`LEVEL` — use recursive `WITH RECURSIVE` CTEs (8.0+ only).
- No native Materialized View — simulate with a table + scheduled `EVENT`.
- Window functions require **MySQL 8.0+** — nothing before that.
- Triggers are **always row-level** — no statement-level, no INSTEAD OF, no compound triggers.
- `Events` (`CREATE EVENT`) are MySQL's cron-like scheduler — must enable `event_scheduler` globally first.
- `LAST_VALUE()` needs an explicit `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` frame, or it silently returns the wrong (current-row) value.

[⬆ Back to top](#-table-of-contents)

---

## 30. 💼 Most Asked Interview Questions

**Q1: Why does autocommit matter in MySQL?**
Because it's ON by default, every individual statement commits immediately unless you explicitly run `START TRANSACTION` first — unlike Oracle, where nothing is saved until an explicit `COMMIT`, which changes how you must structure multi-statement operations that need to be atomic.

**Q2: InnoDB vs MyISAM — what's the real difference?**
InnoDB supports transactions (COMMIT/ROLLBACK), foreign keys, and row-level locking; MyISAM supports none of these — only table-level locking and no transaction safety — making InnoDB the standard choice for any application needing data integrity under concurrent writes.

**Q3: How do you get the "2nd highest salary" in MySQL without a subquery trick, using modern syntax?**
Use a window function: `SELECT sal FROM (SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS drnk FROM emp) t WHERE drnk = 2;` — this correctly handles duplicate top salaries, unlike a naive `LIMIT 1 OFFSET 1` which can return a tied value incorrectly.

**Q4: Why would LAST_VALUE() in a window function return an unexpected result?**
Because its default frame only extends up to the current row, so without explicitly specifying `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`, `LAST_VALUE()` just returns the current row's own value instead of the true last value across the whole partition.

**Q5: How do you replicate a materialized view in MySQL?**
Create a real table holding the pre-computed result, then populate/refresh it on a schedule using a `CREATE EVENT` job (MySQL's built-in scheduler) that periodically re-runs the summarizing query and replaces the table's contents.

[⬆ Back to top](#-table-of-contents)

---

## 31. 🧪 Practice Questions — Hidden Solutions

---

### Question 1 (Query)

**Write a query that shows each employee's salary alongside their department's total salary, without collapsing the individual employee rows.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** A plain `GROUP BY deptno` with `SUM(sal)` would collapse the result to one row per department, losing individual employee rows. A window function aggregate (`SUM(sal) OVER (PARTITION BY deptno)`) computes the same department total but keeps every employee's own row intact.

**Query:**
```sql
SELECT ename, deptno, sal,
       SUM(sal) OVER (PARTITION BY deptno) AS dept_total
FROM emp;
```
</details>

---

### Question 2 (Theory + Query)

**A trigger needs to log salary changes only when the salary actually changes, not on every UPDATE to the row. Write it, and explain why this check is necessary in MySQL specifically.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** Unlike Oracle, which supports a column-specific trigger (`AFTER UPDATE OF sal ON emp`) that only fires when that particular column is part of the UPDATE statement, MySQL triggers have no such column-targeting clause — an `AFTER UPDATE` trigger fires on **every** UPDATE to the row, regardless of which columns changed. So the check for "did this column actually change" must be done manually inside the trigger body by comparing `OLD.col` to `NEW.col`.

**Query:**
```sql
DELIMITER //
CREATE TRIGGER trg_audit_sal
AFTER UPDATE ON emp
FOR EACH ROW
BEGIN
    IF NEW.sal <> OLD.sal THEN
        INSERT INTO sal_audit(empno, old_sal, new_sal, changed_on)
        VALUES (OLD.empno, OLD.sal, NEW.sal, NOW());
    END IF;
END//
DELIMITER ;
```
</details>

---

### Question 3 (Query)

**Given `emp2(empno, ename, mgr)`, write a recursive CTE that prints an indented org chart from the top down, similar to what CONNECT BY does in Oracle.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** Since MySQL has no `CONNECT BY`/`LEVEL`, a `WITH RECURSIVE` CTE must build the depth counter manually in the recursive branch (`lvl + 1`), starting from an anchor query that selects the root row(s).

**Query:**
```sql
WITH RECURSIVE org_chart AS (
    SELECT empno, ename, mgr, 1 AS lvl
    FROM emp2
    WHERE mgr IS NULL

    UNION ALL

    SELECT e.empno, e.ename, e.mgr, o.lvl + 1
    FROM emp2 e
    JOIN org_chart o ON e.mgr = o.empno
)
SELECT CONCAT(LPAD('', (lvl-1)*2, ' '), ename) AS org_chart
FROM org_chart
ORDER BY lvl;
```
</details>

---

### Question 4 (Query)

**Write a query using a window function to find, for each employee, how their salary compares to the previous employee hired (by hiredate), including a default of 0 for the very first employee hired.**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** `LAG(column, offset, default)` looks back `offset` rows within the ordered window; the third argument supplies a fallback value when there is no such row (i.e. for the very first row in the ordering, which has nothing before it).

**Query:**
```sql
SELECT ename, hiredate, sal,
       LAG(sal, 1, 0) OVER (ORDER BY hiredate) AS prev_hire_sal,
       sal - LAG(sal, 1, 0) OVER (ORDER BY hiredate) AS diff
FROM emp;
```
</details>

---

### Question 5 (Theory)

**Why would a stored procedure using a cursor loop forever if you forget the `DECLARE CONTINUE HANDLER FOR NOT FOUND` line?**

<details>
<summary><b>Show Solution</b></summary>

**Theory:** MySQL's explicit cursor loop has no automatic "no more rows" exit like Oracle's cursor `FOR` loop does implicitly. Without a `CONTINUE HANDLER FOR NOT FOUND` to catch the point where `FETCH` runs out of rows and set a `done` flag, the `FETCH` statement inside the `LOOP` would keep executing against an exhausted cursor indefinitely (or error out unhandled), because nothing tells the loop to `LEAVE` once there's nothing left to fetch. The handler is what converts "no more rows" into a controlled exit condition.

```sql
DECLARE done INT DEFAULT FALSE;
DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
...
read_loop: LOOP
    FETCH emp_cur INTO v_ename, v_sal;
    IF done THEN
        LEAVE read_loop;
    END IF;
    ...
END LOOP;
```
</details>

---

[⬆ Back to top](#-table-of-contents)
