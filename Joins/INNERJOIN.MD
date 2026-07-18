# SQL – Inner Join

## What is an Inner Join?
A type of SQL join used to retrieve the **matched records** from multiple tables simultaneously.

- To apply an Inner Join, there **must be a relationship** between the tables — the relationship can be **direct or indirect**.
- Inner Join retrieves **only matched records** (unmatched rows from either table are excluded).

## Syntax

**ANSI:**
```sql
SELECT * FROM TAB1 INNER JOIN TAB2 ON COND WHERE COND;
```

**Oracle (old style):**
```sql
SELECT * FROM TAB1, TAB2 WHERE COND;
```

---

## Direct vs Indirect Relationship

| Type | Meaning | Example |
|---|---|---|
| **Direct** | Two tables share a common key directly | `EMP.DEPTNO = DEPT.DEPTNO` |
| **Indirect** | Tables are related through one or more intermediate tables (chained joins) | `EMPLOYEES → DEPARTMENTS → LOCATIONS → COUNTRIES → REGIONS` |

The `HR` schema table chain used below is a classic example of an **indirect relationship** — `employees` isn't directly related to `countries` or `regions`, but is linked through `departments` and `locations`.

```
employees → departments → locations → countries → regions
   (department_id)  (location_id)  (country_id)  (region_id)
```

---

## Practice Queries

### 1. Employee name + department name
```sql
SELECT e.first_name, d.department_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id;

-- Oracle style
SELECT e.first_name, d.department_name 
FROM employees e, departments d 
WHERE e.department_id = d.department_id;
```

### 2. Employee name + department name + city
```sql
SELECT e.first_name, d.department_name, l.city 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
INNER JOIN locations l ON l.location_id = d.location_id;

-- Oracle style
SELECT e.first_name, d.department_name, l.city 
FROM employees e, departments d, locations l 
WHERE e.department_id = d.department_id 
  AND d.location_id = l.location_id;
```

### 3. Employee name + city + country name
```sql
SELECT e.first_name, l.city, c.country_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
INNER JOIN locations l ON l.location_id = d.location_id 
INNER JOIN countries c ON l.country_id = c.country_id;

-- Oracle style
SELECT e.first_name, l.city, c.country_name 
FROM employees e, departments d, locations l, countries c 
WHERE e.department_id = d.department_id 
  AND d.location_id = l.location_id 
  AND l.country_id = c.country_id;
```

### 4. Employee name + country name (skip displaying intermediate columns)
```sql
SELECT e.first_name, c.country_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
INNER JOIN locations l ON l.location_id = d.location_id 
INNER JOIN countries c ON l.country_id = c.country_id;
```
> Note: Even though `department_name` and `city` aren't selected, the joins through `departments` and `locations` are still required to reach `countries` — you must join through every table in the chain even if you don't display all their columns.

### 5. Employee name + region name (full 5-table chain)
```sql
SELECT e.first_name, r.region_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
INNER JOIN locations l ON l.location_id = d.location_id 
INNER JOIN countries c ON l.country_id = c.country_id 
INNER JOIN regions r ON r.region_id = c.region_id;
```

### 6. Employee name + job title
```sql
SELECT e.first_name, j.job_title 
FROM employees e 
INNER JOIN jobs j ON e.job_id = j.job_id;
```

### 7. Employee name + country name + region name
```sql
SELECT e.first_name, c.country_name, r.region_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
INNER JOIN locations l ON l.location_id = d.location_id 
INNER JOIN countries c ON l.country_id = c.country_id 
INNER JOIN regions r ON r.region_id = c.region_id;
```

### 8. Employee name + department + city + country + region (all levels)
```sql
SELECT e.first_name, d.department_name, l.city, c.country_name, r.region_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
INNER JOIN locations l ON l.location_id = d.location_id 
INNER JOIN countries c ON l.country_id = c.country_id 
INNER JOIN regions r ON r.region_id = c.region_id;
```

---

## Inner Join + Aggregate Functions (GROUP BY)

### 9. Count of employees per location
```sql
SELECT COUNT(*), d.location_id 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
GROUP BY d.location_id;

-- Same idea using EMP/DEPT sample schema
SELECT COUNT(*), d.loc 
FROM emp e 
INNER JOIN dept d ON e.deptno = d.deptno 
GROUP BY d.loc;
```

### 10. Total salary per location
```sql
SELECT SUM(e.salary), d.location_id 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id 
GROUP BY d.location_id;

-- EMP/DEPT version
SELECT SUM(e.sal), d.loc 
FROM emp e 
INNER JOIN dept d ON e.deptno = d.deptno 
GROUP BY d.loc;
```
> Any column selected alongside an aggregate function **must** be in the `GROUP BY` clause.

---

## Inner Join with a Non-Equi Condition (Range Join)

### 11. Employee name, salary, and salary grade
```sql
SELECT e.ename, e.sal, g.grade 
FROM emp e 
INNER JOIN salgrade g ON e.sal BETWEEN g.losal AND g.hisal;
```
This is a **non-equi join** — instead of `=`, it uses `BETWEEN`, since `salgrade` defines salary **ranges**, not exact matching values.

### 12. Employee, salary, grade, and department location
```sql
SELECT e.ename, e.sal, g.grade, d.loc 
FROM emp e 
INNER JOIN salgrade g ON e.sal BETWEEN g.losal AND g.hisal 
INNER JOIN dept d ON e.deptno = d.deptno;
```

---

## Bonus: Inner Join Concept Used Inside Subqueries

These aren't joins themselves, but often appear alongside join practice since they use similar filtering logic (`WHERE`, subqueries, `ROWNUM`).

### 13. Employee with the shortest name
```sql
SELECT ename, LENGTH(ename) 
FROM (
    SELECT ename, LENGTH(ename) 
    FROM emp 
    WHERE LENGTH(ename) = (SELECT MIN(LENGTH(ename)) FROM emp) 
    ORDER BY ename
) 
WHERE ROWNUM = 1;

-- Simpler alternative (works only if there's a single shortest name, alphabetically first)
SELECT MIN(ename) 
FROM emp 
WHERE LENGTH(ename) = (SELECT MIN(LENGTH(ename)) FROM emp);
```

### 14. Employee with the longest name
```sql
SELECT ename, LENGTH(ename) 
FROM (
    SELECT ename, LENGTH(ename) 
    FROM emp 
    WHERE LENGTH(ename) = (SELECT MAX(LENGTH(ename)) FROM emp) 
    ORDER BY ename
) 
WHERE ROWNUM = 1;

SELECT MAX(ename) 
FROM emp 
WHERE LENGTH(ename) = (SELECT MAX(LENGTH(ename)) FROM emp);
```

### 15. Most recently hired employee
```sql
SELECT * 
FROM (
    SELECT emp.* 
    FROM emp 
    WHERE hiredate = (SELECT MAX(hiredate) FROM emp)
) 
WHERE ROWNUM = 1;

-- Simpler version
SELECT * 
FROM emp 
WHERE hiredate = (SELECT MAX(hiredate) FROM emp);
```
> The `ROWNUM = 1` wrapper is only needed as a **tie-breaker** if more than one row could match (e.g., two employees hired on the exact same date, or two employees with names of equal shortest/longest length).

---

## Key Points to Remember

- Inner Join = only **matched rows** are returned; unmatched rows from either side are dropped.
- Works with both **equi joins** (`=`) and **non-equi joins** (`BETWEEN`, `<`, `>`, etc.).
- For **chained/indirect relationships**, you must join through *every* intermediate table, even if you don't display its columns in the final `SELECT`.
- When combining Inner Join with **aggregate functions**, all non-aggregated selected columns must appear in `GROUP BY`.
- ANSI syntax (`INNER JOIN ... ON`) is generally preferred over old Oracle comma-syntax (`FROM a, b WHERE ...`) because it clearly separates **join conditions** from **filter conditions**, and avoids accidental cross joins if a condition is missed.
- `ROWNUM` is commonly used with subqueries to pick a **single row** out of potential ties, after ordering.