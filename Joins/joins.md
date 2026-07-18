# Oracle SQL - JOINS

# What is a JOIN?

## Definition

A **JOIN** is used to combine data from **two or more tables** based on a common column or relationship.

Most databases follow the concept of **Normalization**, where related data is stored in different tables to reduce redundancy. To retrieve meaningful information from multiple tables, we use **JOINS**.

---

# Why Do We Need JOINS?

Suppose we have two tables.

## EMP Table

| EMPNO | ENAME |  SAL | DEPTNO |
| ----: | ----- | ---: | -----: |
|  7369 | SMITH |  800 |     20 |
|  7499 | ALLEN | 1600 |     30 |
|  7782 | CLARK | 2450 |     10 |

---

## DEPT Table

| DEPTNO | DNAME      | LOC      |
| -----: | ---------- | -------- |
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |

The **EMP** table stores employee information.

The **DEPT** table stores department information.

If we want:

```text
ENAME     DNAME        LOCATION

SMITH     RESEARCH     DALLAS
ALLEN     SALES        CHICAGO
CLARK     ACCOUNTING   NEW YORK
```

The data comes from two different tables.

Therefore, we use **JOINS**.

---

# Types of JOINS

Oracle supports the following joins:

1. Cross Join (Cartesian Join)
2. Inner Join (Equi Join)
3. Non-Equi Join
4. Natural Join
5. Self Join
6. Left Outer Join
7. Right Outer Join
8. Full Outer Join

---

# Tables Used

## EMP

| EMPNO | ENAME | JOB | MGR | HIREDATE | SAL | COMM | DEPTNO |
| ----: | ----- | --- | --: | -------- | --: | ---: | -----: |

---

## DEPT

| DEPTNO | DNAME | LOC |
| -----: | ----- | --- |

---

## SALGRADE

| GRADE | LOSAL | HISAL |
| ----: | ----: | ----: |

---

# 1. Cross Join (Cartesian Join)

## Definition

A **Cross Join** returns every row of the first table combined with every row of the second table.

There is **no join condition**.

Formula

```text
Rows in Table A × Rows in Table B
```

Example

EMP = 14 rows

DEPT = 4 rows

Result

```text
14 × 4 = 56 Rows
```

---

## ANSI Syntax

```sql
SELECT *
FROM emp
CROSS JOIN dept;
```

---

## Oracle Old Syntax

```sql
SELECT *
FROM emp, dept;
```

(No WHERE condition)

---

## Example

```sql
SELECT e.ename,
       d.dname
FROM emp e
CROSS JOIN dept d;
```

---

## When to Use

Very rarely.

Usually used only for generating combinations.

---

# 2. Inner Join (Equi Join)

## Definition

An **Inner Join** returns only the rows that satisfy the join condition.

Only matching records are displayed.

---

## Common Column

```text
EMP.DEPTNO = DEPT.DEPTNO
```

---

## ANSI Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e
INNER JOIN dept d
ON e.deptno=d.deptno;
```

---

## Oracle Old Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e,
     dept d
WHERE e.deptno=d.deptno;
```

---

## Example 1

Display Employee Name and Department Name

```sql
SELECT e.ename,
       d.dname
FROM emp e
INNER JOIN dept d
ON e.deptno=d.deptno;
```

---

## Example 2

Display Employee Name, Department Name and Location

```sql
SELECT e.ename,
       d.dname,
       d.loc
FROM emp e
INNER JOIN dept d
ON e.deptno=d.deptno;
```

---

## Example 3

Display Employees Working in SALES Department

```sql
SELECT e.ename,
       d.dname
FROM emp e
INNER JOIN dept d
ON e.deptno=d.deptno
WHERE d.dname='SALES';
```

---

## When to Use

When only matching records are required.

---

# 3. Non-Equi Join

## Definition

A **Non-Equi Join** uses operators other than `=`.

Common operators

* BETWEEN
* >
* <
* > =
* <=

---

## Example Table

SALGRADE

| GRADE | LOSAL | HISAL |
| ----: | ----: | ----: |
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |

---

## ANSI Syntax

```sql
SELECT e.ename,
       e.sal,
       s.grade
FROM emp e
JOIN salgrade s
ON e.sal BETWEEN s.losal AND s.hisal;
```

---

## Oracle Old Syntax

```sql
SELECT e.ename,
       e.sal,
       s.grade
FROM emp e,
     salgrade s
WHERE e.sal BETWEEN s.losal AND s.hisal;
```

---

## Example

Display Employee Name, Salary and Salary Grade

```sql
SELECT e.ename,
       e.sal,
       s.grade
FROM emp e
JOIN salgrade s
ON e.sal BETWEEN s.losal AND s.hisal;
```

---

## When to Use

Whenever the join condition is not equality.

---

# 4. Natural Join

## Definition

A **Natural Join** automatically joins two tables based on columns having the **same name**.

You do not write the join condition.

Oracle automatically identifies the common column.

---

## ANSI Syntax

```sql
SELECT ename,
       dname
FROM emp
NATURAL JOIN dept;
```

---

## Oracle Old Syntax

Not Available.

Natural Join exists only in ANSI SQL.

---

## Example

```sql
SELECT ename,
       dname,
       loc
FROM emp
NATURAL JOIN dept;
```

---

## When to Use

Only when both tables have identical column names representing the same data.

---

# 5. Self Join

## Definition

A **Self Join** is a join where a table is joined with itself.

It is commonly used to display employee-manager relationships.

---

## ANSI Syntax

```sql
SELECT e.ename Employee,
       m.ename Manager
FROM emp e
JOIN emp m
ON e.mgr=m.empno;
```

---

## Oracle Old Syntax

```sql
SELECT e.ename Employee,
       m.ename Manager
FROM emp e,
     emp m
WHERE e.mgr=m.empno;
```

---

## Example 1

Display Employee and Manager Name

```sql
SELECT e.ename,
       m.ename
FROM emp e
JOIN emp m
ON e.mgr=m.empno;
```

---

## Example 2

Display Employee, Manager and Employee Salary

```sql
SELECT e.ename,
       m.ename,
       e.sal
FROM emp e
JOIN emp m
ON e.mgr=m.empno;
```

---

## When to Use

Whenever rows from the same table are related to one another.

---

# 6. Left Outer Join

## Definition

Returns:

* All rows from the Left Table.
* Matching rows from the Right Table.
* NULL for non-matching rows.

---

## ANSI Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e
LEFT OUTER JOIN dept d
ON e.deptno=d.deptno;
```

---

## Oracle Old Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e,
     dept d
WHERE e.deptno=d.deptno(+);
```

---

# 7. Right Outer Join

## Definition

Returns:

* All rows from the Right Table.
* Matching rows from the Left Table.
* NULL for non-matching rows.

---

## ANSI Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e
RIGHT OUTER JOIN dept d
ON e.deptno=d.deptno;
```

---

## Oracle Old Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e,
     dept d
WHERE e.deptno(+)=d.deptno;
```

---

# 8. Full Outer Join

## Definition

Returns:

* All rows from both tables.
* Matching rows are merged.
* Non-matching rows display NULL.

---

## ANSI Syntax

```sql
SELECT e.ename,
       d.dname
FROM emp e
FULL OUTER JOIN dept d
ON e.deptno=d.deptno;
```

---

## Oracle Old Syntax

Not Supported using the `(+)` operator.

Traditionally achieved using:

```sql
LEFT OUTER JOIN
UNION
RIGHT OUTER JOIN
```

---

# Difference Between ANSI and Oracle Join Syntax

| ANSI SQL          | Oracle Old Syntax  |
| ----------------- | ------------------ |
| Uses JOIN keyword | Uses commas        |
| Uses ON clause    | Uses WHERE clause  |
| Easier to read    | Older style        |
| Recommended       | Legacy Oracle code |

---

# Summary Table

| Join Type        | Matching Records   | Non-Matching Records      |
| ---------------- | ------------------ | ------------------------- |
| Cross Join       | No Condition       | Every Combination         |
| Inner Join       | Yes                | No                        |
| Non-Equi Join    | Range/Comparison   | Depends on Condition      |
| Natural Join     | Automatic Matching | No                        |
| Self Join        | Same Table         | Depends on Condition      |
| Left Outer Join  | Yes                | All Left Rows             |
| Right Outer Join | Yes                | All Right Rows            |
| Full Outer Join  | Yes                | All Rows from Both Tables |

---

# Interview Points

* Cross Join produces a Cartesian Product.
* Inner Join returns only matching rows.
* Natural Join automatically detects common column names.
* Self Join uses the same table with different aliases.
* Non-Equi Join uses conditions like `BETWEEN`, `<`, or `>`.
* Left Outer Join returns all rows from the left table.
* Right Outer Join returns all rows from the right table.
* Full Outer Join returns all rows from both tables.
* ANSI Join syntax is preferred in modern SQL development.
* Oracle's `(+)` operator is legacy syntax and may still appear in older Oracle applications.
