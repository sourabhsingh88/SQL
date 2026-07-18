# Oracle SQL - Subqueries and ROWNUM

# Subquery

## Definition

A **Subquery** is a query written inside another SQL query.

It is also known as an **Inner Query** or **Nested Query**.

The inner query executes first, and its result is passed to the outer query.

---

## Why do we use Subqueries?

We use subqueries when:

* The required value is not known in advance.
* The required data depends on another query.
* We need to compare data with values returned by another query.
* We need to solve complex SQL problems in multiple steps.

Example:

Find the employee having the highest salary.

Instead of manually checking the highest salary, Oracle first finds it and then returns the employee.

```sql
SELECT *
FROM emp
WHERE sal =
(
    SELECT MAX(sal)
    FROM emp
);
```

---

# General Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name operator
(
    SELECT column_name
    FROM table_name
);
```

---

# Types of Subqueries

Oracle mainly has **five types of subqueries**:

1. Single Row Subquery
2. Multi Row Subquery
3. Nested Subquery
4. Inline Subquery (Inline View)
5. Correlated Subquery

---

# 1. Single Row Subquery

## Definition

A **Single Row Subquery** returns **only one row (one value)**.

Since only one value is returned, it is compared using single-value comparison operators.

---

## Operators Used

* =
* >
* <
* > =
* <=
* <>

---

## Syntax

```sql
SELECT *
FROM table_name
WHERE column_name operator
(
    SELECT ...
);
```

---

## Examples

### Example 1 - Highest Salary

```sql
SELECT *
FROM emp
WHERE sal =
(
    SELECT MAX(sal)
    FROM emp
);
```

---

### Example 2 - Lowest Salary

```sql
SELECT *
FROM emp
WHERE sal =
(
    SELECT MIN(sal)
    FROM emp
);
```

---

### Example 3 - Employees Working in SALES Department

```sql
SELECT *
FROM emp
WHERE deptno =
(
    SELECT deptno
    FROM dept
    WHERE dname='SALES'
);
```

---

# 2. Multi Row Subquery

## Definition

A **Multi Row Subquery** returns **more than one row**.

Since multiple values are returned, operators like `IN`, `ANY`, and `ALL` are used.

---

## Operators Used

* IN
* ANY
* ALL
* EXISTS

---

## Syntax

```sql
SELECT *
FROM table_name
WHERE column_name operator
(
    SELECT ...
);
```

---

## Examples

### Example 1

```sql
SELECT *
FROM emp
WHERE deptno IN
(
    SELECT deptno
    FROM dept
);
```

---

### Example 2

```sql
SELECT *
FROM emp
WHERE sal > ANY
(
    SELECT sal
    FROM emp
    WHERE deptno=30
);
```

---

### Example 3

```sql
SELECT *
FROM emp
WHERE sal > ALL
(
    SELECT sal
    FROM emp
    WHERE deptno=30
);
```

---

# 3. Nested Subquery

## Definition

A **Nested Subquery** is a subquery inside another subquery.

There are two or more levels of subqueries.

Oracle executes the innermost query first, then the next query, and finally the outer query.

---

## Syntax

```sql
SELECT ...
FROM table_name
WHERE column=
(
    SELECT ...
    FROM table_name
    WHERE column=
    (
        SELECT ...
    )
);
```

---

## Examples

### Example 1

```sql
SELECT *
FROM emp
WHERE deptno=
(
    SELECT deptno
    FROM dept
    WHERE loc=
    (
        SELECT loc
        FROM dept
        WHERE dname='ACCOUNTING'
    )
);
```

---

### Example 2

```sql
SELECT *
FROM emp
WHERE deptno=
(
    SELECT MAX(deptno)
    FROM
    (
        SELECT deptno
        FROM dept
    )
);
```

---

### Example 3

```sql
SELECT *
FROM emp
WHERE sal=
(
    SELECT MAX(sal)
    FROM
    (
        SELECT sal
        FROM emp
    )
);
```

---

# Before Learning Inline Subquery

To understand an **Inline Subquery**, we must first understand **ROWNUM**, because inline views are commonly used with `ROWNUM` for Top-N queries, ranking, pagination, and nth highest salary problems.

---

# ROWNUM

## Definition

`ROWNUM` is a **Pseudo Column** provided by Oracle.

It is not stored inside the table.

Oracle automatically assigns a row number to every row returned by a query.

Numbering always starts from **1**.

---

## Syntax

```sql
SELECT rownum, column_name
FROM table_name;
```

---

## Rules of ROWNUM

### Rule 1

`ROWNUM` starts from **1**.

---

### Rule 2

`ROWNUM` is assigned **before ORDER BY**.

Execution Order

```text
FROM
↓

WHERE
↓

ROWNUM Assigned
↓

SELECT
↓

ORDER BY
```

---

### Rule 3

These work correctly.

```sql
WHERE rownum=1
```

```sql
WHERE rownum<=5
```

```sql
WHERE rownum<10
```

---

### Rule 4

These do **NOT** work.

```sql
WHERE rownum=2
```

```sql
WHERE rownum>5
```

Output

```text
No rows selected
```

---

## Examples

### Display Employee Name with ROWNUM

```sql
SELECT rownum,ename
FROM emp;
```

---

### Display First Employee

```sql
SELECT *
FROM emp
WHERE rownum=1;
```

---

### Display First Five Employees

```sql
SELECT *
FROM emp
WHERE rownum<=5;
```

---

### Wrong Example

```sql
SELECT *
FROM emp
WHERE rownum=2;
```

Output

```text
No rows selected
```

---

### Wrong Example

```sql
SELECT *
FROM emp
WHERE rownum>5;
```

Output

```text
No rows selected
```

---

# 4. Inline Subquery (Inline View)

## Definition

An **Inline Subquery** is a subquery written inside the **FROM** clause.

It behaves like a temporary table.

Inline subqueries are **commonly used with ROWNUM** because `ROWNUM` is assigned before `ORDER BY`.

To sort data first and then apply `ROWNUM`, we create an Inline View.

---

## When to Use

* Top N Records
* Bottom N Records
* Nth Highest Salary
* Nth Lowest Salary
* Pagination
* Ranking using ROWNUM

---

## Syntax

```sql
SELECT *
FROM
(
    SELECT ...
    FROM table_name
)
WHERE condition;
```

---

## Examples

### Third Record

```sql
SELECT *
FROM
(
    SELECT emp.*,rownum r
    FROM emp
)
WHERE r=3;
```

---

### 2nd, 4th and 5th Records

```sql
SELECT *
FROM
(
    SELECT emp.*,rownum r
    FROM emp
)
WHERE r IN(2,4,5);
```

---

### Last Record

```sql
SELECT *
FROM
(
    SELECT emp.*,rownum r
    FROM emp
)
WHERE r=
(
    SELECT COUNT(*)
    FROM emp
);
```

---

### Top Five Highest Salaries

```sql
SELECT *
FROM
(
    SELECT *
    FROM emp
    ORDER BY sal DESC
)
WHERE rownum<=5;
```

---

### Second Highest Salary

```sql
SELECT *
FROM
(
    SELECT e.*,rownum r
    FROM
    (
        SELECT *
        FROM emp
        ORDER BY sal DESC
    ) e
)
WHERE r=2;
```

---

### Employees Ranked Between 4 and 7

```sql
SELECT *
FROM
(
    SELECT e.*,rownum r
    FROM
    (
        SELECT *
        FROM emp
        ORDER BY sal DESC
    ) e
)
WHERE r BETWEEN 4 AND 7;
```

---

# 5. Correlated Subquery

## Definition

A **Correlated Subquery** is a subquery that depends on the outer query.

Unlike other subqueries, it is **executed once for every row** processed by the outer query.

---

## When to Use

* Compare each row with its group.
* Find employees above department average.
* Find highest-paid employee in each department.
* Row-by-row comparisons.

---

## Syntax

```sql
SELECT ...
FROM table1 t1
WHERE ...
(
    SELECT ...
    FROM table2 t2
    WHERE t1.column=t2.column
);
```

---

## Examples

### Employees Earning More Than Their Department Average

```sql
SELECT *
FROM emp e
WHERE sal>
(
    SELECT AVG(sal)
    FROM emp
    WHERE deptno=e.deptno
);
```

---

### Highest Paid Employee in Each Department

```sql
SELECT *
FROM emp e
WHERE sal=
(
    SELECT MAX(sal)
    FROM emp
    WHERE deptno=e.deptno
);
```

---

### Employees Having at Least One Colleague with the Same Job

```sql
SELECT *
FROM emp e
WHERE EXISTS
(
    SELECT 1
    FROM emp x
    WHERE x.job=e.job
    AND x.empno<>e.empno
);
```
