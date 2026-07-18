# Oracle SQL - Operators

# What is an Operator?

## Definition

An **Operator** is a symbol or keyword used to perform a specific operation on one or more operands (values or columns).

Operators are used to:

- Perform mathematical calculations
- Compare values
- Combine multiple conditions
- Join strings
- Combine results of multiple queries
- Filter records
- Work with subqueries

---

# Types of Operators

1. Arithmetic Operator
2. Logical Operator
3. Relational Operator
4. Concatenation Operator
5. Set Operator
6. Special Operator
7. Subquery Operator

---

# 1. Arithmetic Operator

## Definition

Arithmetic operators are used to perform mathematical calculations.

---

## Operators

```
+
-
*
/
```

---

## Syntax

```sql
column_name operator value
```

---

## Examples

### Addition

```sql
SELECT ename,
       sal+1000 AS new_salary
FROM emp;
```

---

### Subtraction

```sql
SELECT ename,
       sal-500
FROM emp;
```

---

### Multiplication

```sql
SELECT ename,
       sal*12 AS annual_salary
FROM emp;
```

---

### Division

```sql
SELECT ename,
       sal/2
FROM emp;
```

---

## Practice Questions

### Q1 Display employee name and annual salary.

```sql
SELECT ename,
       sal*12 AS annual_salary
FROM emp;
```

---

### Q2 Display employee name and salary after increasing it by 500.

```sql
SELECT ename,
       sal+500
FROM emp;
```

---

# 2. Logical Operator

## Definition

Logical operators are used to combine two or more conditions.

---

## Operators

```
AND
OR
NOT
```

---

## Syntax

```sql
condition1 AND condition2
condition1 OR condition2
NOT condition
```

---

## Examples

### AND

```sql
SELECT *
FROM emp
WHERE deptno=30
AND sal>1500;
```

---

### OR

```sql
SELECT *
FROM emp
WHERE deptno=10
OR deptno=20;
```

---

### NOT

```sql
SELECT *
FROM emp
WHERE NOT deptno=10;
```

---

## Practice Questions

### Q1 Display employees working in department 30 having salary greater than 1500.

```sql
SELECT *
FROM emp
WHERE deptno=30
AND sal>1500;
```

---

### Q2 Display employees working in department 10 or 20.

```sql
SELECT *
FROM emp
WHERE deptno=10
OR deptno=20;
```

---

# 3. Relational Operator

## Definition

Relational operators are used to compare two values.

They always return TRUE or FALSE.

---

## Operators

```
=
>
<
>=
<=
<>
!=
```

---

## Syntax

```sql
column operator value
```

---

## Examples

```sql
SELECT *
FROM emp
WHERE sal>2000;
```

---

```sql
SELECT *
FROM emp
WHERE sal<1500;
```

---

```sql
SELECT *
FROM emp
WHERE sal>=3000;
```

---

```sql
SELECT *
FROM emp
WHERE deptno<>20;
```

---

## Practice Questions

### Display employees whose salary is greater than 2500.

```sql
SELECT *
FROM emp
WHERE sal>2500;
```

---

### Display employees not working in department 30.

```sql
SELECT *
FROM emp
WHERE deptno<>30;
```

---

# 4. Concatenation Operator

## Definition

Concatenation operator is used to join two or more strings.

Oracle uses the **Pipe Operator (||)**.

---

## Syntax

```sql
column1 || column2
```

---

## Examples

```sql
SELECT ename||job
FROM emp;
```

---

```sql
SELECT ename||' works as '||job
FROM emp;
```

---

```sql
SELECT empno||'-'||ename
FROM emp;
```

---

## Practice Question

Display employee number and employee name together.

```sql
SELECT empno||' - '||ename
FROM emp;
```

---

# 5. Set Operators

## Definition

Set operators are used to combine the results of two or more SELECT statements.

Both SELECT statements must have:

- Same number of columns
- Same data types
- Same order of columns

---

## Types

- UNION
- UNION ALL
- INTERSECT
- MINUS

---

## UNION

Returns only unique rows.

```sql
SELECT deptno
FROM emp
UNION
SELECT deptno
FROM dept;
```

---

## UNION ALL

Returns all rows including duplicates.

```sql
SELECT deptno
FROM emp
UNION ALL
SELECT deptno
FROM dept;
```

---

## INTERSECT

Returns only common rows.

```sql
SELECT deptno
FROM emp
INTERSECT
SELECT deptno
FROM dept;
```

---

## MINUS

Returns rows from first query that are not present in second query.

```sql
SELECT deptno
FROM dept
MINUS
SELECT deptno
FROM emp;
```

---

# 6. Special Operators

Special operators are used to filter records.

They make searching easier.

---

# IN Operator

## Definition

**IN** is a multi-valued special operator.

It accepts multiple values on the **RHS (Right Hand Side)** and only one value on the **LHS (Left Hand Side)**.

---

## Syntax

```sql
column IN(value1,value2,value3,...)
```

---

## Example

```sql
SELECT *
FROM emp
WHERE deptno IN(10,20,30);
```

---

### Practice Question

Display employees working in department 10,20 and 30.

```sql
SELECT *
FROM emp
WHERE deptno IN(10,20,30);
```

---

# NOT IN Operator

## Definition

NOT IN is used to exclude the values specified on the RHS.

---

## Syntax

```sql
column NOT IN(value1,value2,...)
```

---

## Example

```sql
SELECT *
FROM emp
WHERE deptno NOT IN(10,20);
```

---

### Practice Question

Display all employees except departments 10 and 20.

```sql
SELECT *
FROM emp
WHERE deptno NOT IN(10,20);
```

---

# LIKE Operator

## Definition

LIKE is a special operator used for **Pattern Matching**.

---

## Wildcards

```
%  → Any number of characters

_  → Exactly one character
```

---

## Syntax

```sql
column LIKE 'pattern'
```

---

## Examples

### Starts with S

```sql
SELECT ename
FROM emp
WHERE ename LIKE 'S%';
```

---

### Ends with H

```sql
SELECT ename
FROM emp
WHERE ename LIKE '%H';
```

---

### Contains A

```sql
SELECT ename
FROM emp
WHERE ename LIKE '%A%';
```

---

### Exactly Five Characters

```sql
SELECT ename
FROM emp
WHERE ename LIKE '_____';
```

---

### Starts with M and Ends with R

```sql
SELECT ename
FROM emp
WHERE ename LIKE 'M%R';
```

---

### Second Character is A

```sql
SELECT ename
FROM emp
WHERE ename LIKE '_A%';
```

---

# NOT LIKE

## Definition

NOT LIKE excludes records matching the given pattern.

---

## Syntax

```sql
column NOT LIKE 'pattern'
```

---

## Example

```sql
SELECT *
FROM emp
WHERE ename NOT LIKE 'S%';
```

---

# BETWEEN

## Definition

BETWEEN is used to retrieve data within a range.

It includes both lower limit and upper limit.

---

## Syntax

```sql
column BETWEEN lower_value AND upper_value
```

---

## Example

```sql
SELECT *
FROM emp
WHERE sal BETWEEN 1000 AND 3000;
```

---

### Practice Question

Display employees whose salary is between 1500 and 2500.

```sql
SELECT *
FROM emp
WHERE sal BETWEEN 1500 AND 2500;
```

---

# NOT BETWEEN

## Definition

NOT BETWEEN returns rows outside the specified range.

---

## Syntax

```sql
column NOT BETWEEN lower_value AND upper_value
```

---

## Example

```sql
SELECT *
FROM emp
WHERE sal NOT BETWEEN 1000 AND 3000;
```

---

# IS NULL

## Definition

IS NULL checks whether a column contains NULL values.

---

## Syntax

```sql
column IS NULL
```

---

## Example

```sql
SELECT *
FROM emp
WHERE comm IS NULL;
```

---

# IS NOT NULL

## Definition

IS NOT NULL checks whether a column contains a value.

---

## Syntax

```sql
column IS NOT NULL
```

---

## Example

```sql
SELECT *
FROM emp
WHERE comm IS NOT NULL;
```

---

# 7. Subquery Operators

## Definition

Subquery operators are used with subqueries that return one or more values.

---

## Types

- IN
- ANY
- ALL
- EXISTS
- NOT EXISTS

---

# ANY

## Definition

Returns TRUE if the condition is satisfied by at least one value.

---

## Example

```sql
SELECT *
FROM emp
WHERE sal>ANY
(
SELECT sal
FROM emp
WHERE deptno=30
);
```

---

# ALL

## Definition

Returns TRUE only if the condition is satisfied by every value returned.

---

## Example

```sql
SELECT *
FROM emp
WHERE sal>ALL
(
SELECT sal
FROM emp
WHERE deptno=30
);
```

---

# EXISTS

## Definition

Returns TRUE if the subquery returns at least one row.

---

## Example

```sql
SELECT *
FROM dept d
WHERE EXISTS
(
SELECT *
FROM emp e
WHERE e.deptno=d.deptno
);
```

---

# NOT EXISTS

## Definition

Returns TRUE if the subquery returns no rows.

---

## Example

```sql
SELECT *
FROM dept d
WHERE NOT EXISTS
(
SELECT *
FROM emp e
WHERE e.deptno=d.deptno
);
```

---

# Summary

| Operator Type | Operators |
|--------------|-----------|
| Arithmetic | +, -, *, / |
| Logical | AND, OR, NOT |
| Relational | =, >, <, >=, <=, <>, != |
| Concatenation | \|\| |
| Set | UNION, UNION ALL, INTERSECT, MINUS |
| Special | IN, NOT IN, LIKE, NOT LIKE, BETWEEN, NOT BETWEEN, IS NULL, IS NOT NULL |
| Subquery | IN, ANY, ALL, EXISTS, NOT EXISTS |