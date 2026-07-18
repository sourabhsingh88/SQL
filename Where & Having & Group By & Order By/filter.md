# Oracle SQL - WHERE, GROUP BY, HAVING & ORDER BY

# SQL Query Execution Order

Oracle executes a SELECT query in the following order:

```text
FROM
   ↓
WHERE
   ↓
GROUP BY    
   ↓
HAVING
   ↓
SELECT
   ↓
ORDER BY
```

---

# 1. WHERE Clause

## Definition

The **WHERE** clause is used to filter records from a table based on a specified condition.

Only the rows satisfying the condition are returned.

---

## When to Use

- Filter rows before grouping.
- Retrieve only required records.
- Use comparison, logical and special operators.

---

## Syntax

```sql
SELECT column_name
FROM table_name
WHERE condition;
```

---

## Examples

### Display employees working in department 10.

```sql
SELECT *
FROM emp
WHERE deptno=10;
```

---

### Display employees having salary greater than 3000.

```sql
SELECT *
FROM emp
WHERE sal>3000;
```

---

### Display employees working as MANAGER.

```sql
SELECT *
FROM emp
WHERE job='MANAGER';
```

---

### Display employees hired after 01-JAN-1982.

```sql
SELECT *
FROM emp
WHERE hiredate>'01-JAN-82';
```

---

### Display employees whose commission is NULL.

```sql
SELECT *
FROM emp
WHERE comm IS NULL;
```

---

## Practice Questions

### Q1 Display employees working in department 20.

```sql
SELECT *
FROM emp
WHERE deptno=20;
```

---

### Q2 Display employees having salary between 1500 and 3000.

```sql
SELECT *
FROM emp
WHERE sal BETWEEN 1500 AND 3000;
```

---

# 2. GROUP BY Clause

## Definition

The **GROUP BY** clause is used to group rows having the same value in one or more columns.

It is generally used with Aggregate Functions.

---

## Why Do We Use GROUP BY?

Without GROUP BY, aggregate functions calculate the result for the entire table.

GROUP BY divides the rows into groups and calculates aggregate values for each group.

---

## Aggregate Functions Used with GROUP BY

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()

---

## Syntax

```sql
SELECT column_name,
       aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

---

## Examples

### Count employees in each department.

```sql
SELECT deptno,
       COUNT(*)
FROM emp
GROUP BY deptno;
```

---

### Find average salary of each department.

```sql
SELECT deptno,
       AVG(sal)
FROM emp
GROUP BY deptno;
```

---

### Find maximum salary in each department.

```sql
SELECT deptno,
       MAX(sal)
FROM emp
GROUP BY deptno;
```

---

### Find minimum salary in each department.

```sql
SELECT deptno,
       MIN(sal)
FROM emp
GROUP BY deptno;
```

---

### Count employees working in each job.

```sql
SELECT job,
       COUNT(*)
FROM emp
GROUP BY job;
```

---

## GROUP BY Rules

- GROUP BY is executed after WHERE.
- GROUP BY is executed before HAVING.
- Every non-aggregate column in SELECT must be present in GROUP BY.
- Aggregate functions cannot be used directly in GROUP BY.

---

# 3. HAVING Clause

## Definition

The **HAVING** clause is used to filter groups created by the GROUP BY clause.

It is similar to WHERE, but WHERE filters rows whereas HAVING filters groups.

---

## Difference Between WHERE and HAVING

| WHERE | HAVING |
|--------|---------|
| Filters rows | Filters groups |
| Executed before GROUP BY | Executed after GROUP BY |
| Cannot use aggregate functions | Aggregate functions are commonly used |

---

## Syntax

```sql
SELECT column_name,
       aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

---

## Examples

### Display departments having more than 3 employees.

```sql
SELECT deptno,
       COUNT(*)
FROM emp
GROUP BY deptno
HAVING COUNT(*)>3;
```

---

### Display departments whose average salary is greater than 2000.

```sql
SELECT deptno,
       AVG(sal)
FROM emp
GROUP BY deptno
HAVING AVG(sal)>2000;
```

---

### Display jobs having more than 2 employees.

```sql
SELECT job,
       COUNT(*)
FROM emp
GROUP BY job
HAVING COUNT(*)>2;
```

---

### Display departments whose maximum salary is greater than 4000.

```sql
SELECT deptno,
       MAX(sal)
FROM emp
GROUP BY deptno
HAVING MAX(sal)>4000;
```

---

## WHERE + GROUP BY + HAVING

### Display departments having more than one MANAGER.

```sql
SELECT deptno,
       COUNT(*)
FROM emp
WHERE job='MANAGER'
GROUP BY deptno
HAVING COUNT(*)>1;
```

---

# 4. ORDER BY Clause

## Definition

The **ORDER BY** clause is used to sort the result set.

Sorting can be done in:

- Ascending Order
- Descending Order

---

## Types

### ASC (Default)

Ascending Order

```text
A → Z
0 → 9
Oldest → Newest
```

---

### DESC

Descending Order

```text
Z → A
9 → 0
Newest → Oldest
```

---

## Syntax

```sql
SELECT column_name
FROM table_name
ORDER BY column_name;
```

---

## Syntax (Descending)

```sql
SELECT column_name
FROM table_name
ORDER BY column_name DESC;
```

---

## Examples

### Display employees ordered by employee name.

```sql
SELECT *
FROM emp
ORDER BY ename;
```

---

### Display employees ordered by salary in descending order.

```sql
SELECT *
FROM emp
ORDER BY sal DESC;
```

---

### Display employees ordered by hire date.

```sql
SELECT *
FROM emp
ORDER BY hiredate;
```

---

### Display employees ordered by department and salary.

```sql
SELECT *
FROM emp
ORDER BY deptno,sal;
```

---

### Display employees ordered by job and employee name.

```sql
SELECT *
FROM emp
ORDER BY job,ename;
```

---

## ORDER BY Rules

- ORDER BY is the last clause in a SELECT statement.
- ASC is the default sorting order.
- Multiple columns can be used.
- Each column can have its own sorting order.

Example

```sql
SELECT *
FROM emp
ORDER BY deptno ASC,
         sal DESC;
```

---

# Difference Between WHERE, GROUP BY, HAVING and ORDER BY

| Clause | Purpose |
|----------|---------|
| WHERE | Filters rows |
| GROUP BY | Groups similar rows |
| HAVING | Filters groups |
| ORDER BY | Sorts the final result |

---

# Query Execution Order

```text
FROM
   ↓
WHERE
   ↓
GROUP BY
   ↓
HAVING
   ↓
SELECT
   ↓
ORDER BY
```

---

# Summary

| Clause | Used For |
|---------|----------|
| WHERE | Filter rows |
| GROUP BY | Group similar rows |
| HAVING | Filter grouped rows |
| ORDER BY | Sort final output |