# 📘 DQL – Data Query Language

## 📑 Table of Contents
1. [What is DQL](#1-what-is-dql)
2. [Selection & Projection](#2-selection--projection)
3. [SQL Functions](#3-sql-functions)
4. [SQL Joins](#4-sql-joins)
5. [Outer Joins – Practical Walkthrough](#5-outer-joins--practical-walkthrough)
6. [Self Join – Family Tree Example](#6-self-join--family-tree-example)
7. [SQL Operators](#7-sql-operators)
8. [Subqueries & ROWNUM](#8-subqueries--rownum)
9. [WHERE, GROUP BY, HAVING & ORDER BY](#9-where-group-by-having--order-by)
10. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
11. [💼 Tricky Interview Questions](#-tricky-interview-questions)

---

## 1. What is DQL

**DQL** (Data Query Language) is used to **retrieve data** from the database — its primary and only command is `SELECT`.

> 🔑 DQL is **read-only** — it never changes stored data. This is why `SELECT` has no `COMMIT`/`ROLLBACK` implications.

[⬆ Back to top](#-table-of-contents)

---

## 2. Selection & Projection

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

## 3. SQL Functions

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

## 4. SQL Joins

A **JOIN** combines data from two or more tables based on a common column/relationship.

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

### 2) Inner Join (Equi Join)
Returns only rows that satisfy the join condition — **matched records only**.
```sql
-- ANSI
SELECT e.ename, d.dname FROM emp e INNER JOIN dept d ON e.deptno=d.deptno;
-- Oracle old
SELECT e.ename, d.dname FROM emp e, dept d WHERE e.deptno=d.deptno;
```

### 3) Non-Equi Join
Uses operators **other than `=`**: `BETWEEN`, `>`, `<`, `>=`, `<=`.
```sql
-- ANSI
SELECT e.ename, e.sal, s.grade FROM emp e JOIN salgrade s ON e.sal BETWEEN s.losal AND s.hisal;
```

### 4) Natural Join
Automatically joins two tables based on columns having the **same name** — no join condition written.
```sql
SELECT ename, dname FROM emp NATURAL JOIN dept;
```
- Available **only in ANSI syntax** (no Oracle old-style equivalent).
- If no common column exists → behaves like **CROSS JOIN**.

### 5) Self Join
A table joined with **itself**, using aliases. See a full worked example in [Section 6](#6-self-join--family-tree-example).
```sql
-- ANSI
SELECT e.ename Employee, m.ename Manager FROM emp e JOIN emp m ON e.mgr=m.empno;
```

### 6), 7), 8) Outer Joins — Introduction

**Left Outer Join** → matched records from left + right, plus unmatched records from **left table** with NULL.
```sql
SELECT * FROM tab1 LEFT OUTER JOIN tab2 ON cond;              -- ANSI
SELECT * FROM tab1, tab2 WHERE tab1.col = tab2.col(+);        -- Oracle old
```

**Right Outer Join** → matched records from right + left, plus unmatched records from **right table** with NULL.
```sql
SELECT * FROM tab1 RIGHT OUTER JOIN tab2 ON cond;              -- ANSI
SELECT * FROM tab1, tab2 WHERE tab1.col(+) = tab2.col;         -- Oracle old
```

**Full Outer Join** → matched records + unmatched records from **both** sides with NULL.
```sql
SELECT * FROM tab1 FULL JOIN tab2 ON cond;   -- ANSI only, no Oracle old-style equivalent
```

> 🧠 **Memory trick:** The `(+)` symbol goes on the side that is *missing/deficient* — it marks the table that gets NULL-padded for unmatched rows.

Full detailed examples in [Section 5](#5-outer-joins--practical-walkthrough).

### ANSI vs Oracle Old Syntax

| ANSI SQL | Oracle Old Syntax |
|---|---|
| Uses `JOIN` keyword | Uses commas |
| Uses `ON` clause | Uses `WHERE` clause |
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

## 5. Outer Joins – Practical Walkthrough

Sample data:

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

### Left Outer Join — keeps every student
```sql
SELECT name, sname FROM student s LEFT OUTER JOIN subject sb ON s.subid = sb.subid;
```
| name | sname |
|---|---|
| indu | web |
| bindu | — |
| dindu | Java |
| chondu | web |
| nendu | — |

### Right Outer Join — keeps every subject
```sql
SELECT name, sname FROM student s RIGHT OUTER JOIN subject sb ON s.subid = sb.subid;
```
| name | sname |
|---|---|
| indu | web |
| dindu | Java |
| chondu | web |
| — | sql |
| — | Python |

### Full Outer Join — keeps everything
```sql
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

> 💡 **Pattern:** `LEFT/RIGHT JOIN + WHERE <joined-key> IS NULL` is the classic trick to find **unmatched / orphan records**.

[⬆ Back to top](#-table-of-contents)

---

## 6. Self Join – Family Tree Example

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

### Q2: Find the Great-Grandfather
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

> 💡 Each extra "generation" you need to trace back requires **one more self-join**.

[⬆ Back to top](#-table-of-contents)

---

## 7. SQL Operators

### Types of Operators
1. Arithmetic 2. Logical 3. Relational 4. Concatenation 5. Set 6. Special 7. Subquery

### 1) Arithmetic: `+ - * /`
```sql
SELECT ename, sal*12 AS annual_salary FROM emp;
```

### 2) Logical: `AND`, `OR`, `NOT`
```sql
SELECT * FROM emp WHERE deptno=30 AND sal>1500;
SELECT * FROM emp WHERE deptno=10 OR deptno=20;
```

### 3) Relational: `= > < >= <= <> !=`
```sql
SELECT * FROM emp WHERE sal>2000;
```

### 4) Concatenation: `||`
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

[⬆ Back to top](#-table-of-contents)

---

## 8. Subqueries & ROWNUM

### Subquery
A query written **inside another SQL query**. The inner query executes first; its result feeds the outer query.
```sql
SELECT * FROM emp WHERE sal = (SELECT MAX(sal) FROM emp);
```

### Types of Subqueries

**1) Single Row Subquery** — returns only **one value**. Uses `= > < >= <= <>`.
```sql
SELECT * FROM emp WHERE sal = (SELECT MAX(sal) FROM emp);
```

**2) Multi Row Subquery** — returns **more than one row**. Uses `IN ANY ALL EXISTS`.
```sql
SELECT * FROM emp WHERE deptno IN (SELECT deptno FROM dept);
SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=30);
SELECT * FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno=30);
```

**3) Nested Subquery** — a subquery **inside another subquery** (2+ levels).
```sql
SELECT * FROM emp WHERE deptno = (
    SELECT deptno FROM dept WHERE loc = (
        SELECT loc FROM dept WHERE dname='ACCOUNTING'
    )
);
```

**4) Inline Subquery (Inline View)** — a subquery in the **FROM** clause; behaves like a temporary table.
```sql
-- Top 5 highest salaries
SELECT * FROM (SELECT * FROM emp ORDER BY sal DESC) WHERE ROWNUM<=5;

-- Second highest salary
SELECT * FROM (
    SELECT e.*, ROWNUM r FROM (SELECT * FROM emp ORDER BY sal DESC) e
) WHERE r=2;
```

**5) Correlated Subquery** — depends on the outer query; executed **once per row**.
```sql
SELECT * FROM emp e WHERE sal > (SELECT AVG(sal) FROM emp WHERE deptno=e.deptno);
```

### ROWNUM
A **Pseudo Column** in Oracle — auto-assigns row numbers starting from **1**.

**Execution order:** `FROM → WHERE → ROWNUM assigned → SELECT → ORDER BY`
> ⚠️ `ROWNUM` is assigned **before** `ORDER BY` — this is why Inline Views are needed to sort first, then apply `ROWNUM`.

| Works ✅ | Does NOT work ❌ |
|---|---|
| `WHERE rownum=1` | `WHERE rownum=2` |
| `WHERE rownum<=5` | `WHERE rownum>5` |

[⬆ Back to top](#-table-of-contents)

---

## 9. WHERE, GROUP BY, HAVING & ORDER BY

### SQL Query Execution Order
```text
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
```

### 1) WHERE Clause
Filters rows **before grouping**.
```sql
SELECT * FROM emp WHERE deptno=10;
SELECT * FROM emp WHERE comm IS NULL;
```

### 2) GROUP BY Clause
Groups rows sharing the same value — used with aggregate functions.
```sql
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
```

### 3) HAVING Clause
Filters **groups** created by `GROUP BY`.

| WHERE | HAVING |
|---|---|
| Filters rows | Filters groups |
| Executed before GROUP BY | Executed after GROUP BY |
| Cannot use aggregate functions | Aggregate functions commonly used |

```sql
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*)>3;

SELECT deptno, COUNT(*) FROM emp
WHERE job='MANAGER'
GROUP BY deptno
HAVING COUNT(*)>1;
```

### 4) ORDER BY Clause
Sorts the result set — **Ascending (`ASC`, default)** or **Descending (`DESC`)**.
```sql
SELECT * FROM emp ORDER BY sal DESC;
SELECT * FROM emp ORDER BY deptno ASC, sal DESC;
```

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- `SELECT` execution order is `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY` — this explains why you **can't** use a column alias in `WHERE` (alias isn't computed yet) but **can** use it in `ORDER BY` (runs last).
- Every non-aggregated column in `SELECT` **must** appear in `GROUP BY`, or Oracle throws an error.
- `WHERE` filters rows **before** grouping and can't reference aggregate functions; `HAVING` filters **after** grouping and is where aggregate conditions belong.
- `NULL` is never equal to anything — not even another `NULL` (`NULL = NULL` is `UNKNOWN`, not `TRUE`). Always use `IS NULL` / `IS NOT NULL`.
- Aggregate functions (`SUM`, `AVG`, `COUNT(col)`, `MAX`, `MIN`) **ignore NULLs** automatically — except `COUNT(*)` which counts all rows regardless of NULLs.
- Forgetting a join condition (`ON`/`WHERE`) turns your query into an accidental **Cartesian product**.
- `ROWNUM` is generated **before** `ORDER BY`, so `WHERE ROWNUM > 1` or `= 2` never works directly — always sort first inside an inline view.
- `DISTINCT` applies to the **entire selected row**, not to a single column, when multiple columns are selected.
- A **Single Row subquery operator** (`=`, `>`, `<`) used with a subquery that returns multiple rows will error out — use `IN`/`ANY`/`ALL` instead.

[⬆ Back to top](#-table-of-contents)

---

## 💼 Tricky Interview Questions

**Q1. If I use `GROUP BY` but include a non-aggregated column in `SELECT` that isn't in the `GROUP BY` list, what happens?**
Oracle throws `ORA-00979: not a GROUP BY expression`. **Every** non-aggregate column in `SELECT` must appear in `GROUP BY`.

**Q2. If I use `HAVING` without a `GROUP BY` clause at all, does it work?**
Yes — Oracle treats the entire result set as **one single group** in that case, so `HAVING` just filters based on an aggregate over the whole table (e.g. `SELECT COUNT(*) FROM emp HAVING COUNT(*) > 10;`).

**Q3. If I put an aggregate function like `SUM(sal) > 5000` inside `WHERE` instead of `HAVING`, what happens?**
It **fails** — `WHERE` executes before grouping/aggregation happens, so aggregate functions aren't available there yet. You must use `HAVING` for aggregate-based filters.

**Q4. If I compare a column to `NULL` using `WHERE col = NULL`, what rows come back?**
**None** — `= NULL` never evaluates to true in SQL, because `NULL` represents "unknown," and unknown compared to anything (even itself) is unknown, not true. You must use `WHERE col IS NULL`.

**Q5. If I do `SELECT COUNT(comm) FROM emp;` where several employees have `comm` as NULL, what does it count?**
Only the rows where `comm` **is NOT NULL**. `COUNT(column)` skips NULLs; only `COUNT(*)` counts every row regardless of NULLs.

**Q6. If I forget the `ON`/join condition between two tables, what happens to my result?**
You get a **Cartesian product** — every row of table A combined with every row of table B (same result as `CROSS JOIN`), which is rarely what you actually want and can silently return a huge, meaningless result set.

**Q7. If I run `WHERE ROWNUM = 5` to try to get the 5th row, what do I get?**
**Zero rows.** Oracle assigns `ROWNUM` sequentially as rows are found and only increments it once row 1 matches the condition; since `ROWNUM = 5` can never be true for the first evaluated row, and once a row fails, later rows never get re-numbered to fill the gap — nothing is ever returned. You must use `<=` or wrap it in an inline view with a sorted subquery and alias the rownum in the outer query.

**Q8. If I use a Single Row subquery operator (`=`) with a subquery that unexpectedly returns multiple rows, what happens?**
Oracle throws `ORA-01427: single-row subquery returns more than one row`. You'd need to switch to `IN`, `ANY`, or `ALL` to safely handle multiple returned values.

**Q9. If two tables both have a column with the exact same name (say, `ID`) but they represent unrelated concepts, and I use `NATURAL JOIN`, what happens?**
Oracle **silently joins on that column** since it matches by name — even though the values are logically unrelated. This can produce **wrong, misleading results** without throwing any error, which is exactly why `NATURAL JOIN` is considered risky in production code.

**Q10. If I use `UNION` instead of `UNION ALL` on two large result sets with no actual duplicates, is there a performance cost?**
Yes — `UNION` **always performs an implicit sort/duplicate-check** across the combined result, even if there turn out to be no duplicates at all, making it slower than `UNION ALL` in that scenario.

**Q11. If I write `SELECT DISTINCT ename, deptno FROM emp;`, does DISTINCT apply to `ename` alone or to the combination?**
It applies to the **combination of both columns together** — a row is only removed if *both* `ename` and `deptno` match another row exactly.

**Q12. If my subquery in the `FROM` clause (inline view) has an `ORDER BY`, and the outer query also has its own `ORDER BY`, which one wins?**
The **outer query's `ORDER BY`** determines the final display order. However, the inner `ORDER BY` still matters when it's used together with `ROWNUM` inside that inline view — because `ROWNUM` needs the inner query's ordering to number rows in the desired sequence *before* the outer query re-sorts them.

[⬆ Back to top](#-table-of-contents)