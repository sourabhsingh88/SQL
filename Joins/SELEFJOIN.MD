# SQL – Self Join

## What is a Self Join?
A type of SQL join used to join the **same table to itself** — treating it as if it were two separate tables (using aliases), usually to compare rows within the same table (e.g., employee & manager, both stored in the `EMP` table).

## Syntax

**ANSI:**
```sql
SELECT * 
FROM TAB1 T1 
JOIN TAB1 T2 ON COND 
WHERE COND;
```

**Oracle (old style):**
```sql
SELECT * 
FROM TAB1 T1, TAB1 T2 
WHERE COND;
```

> Since it's the same table used twice, **aliases are mandatory** to differentiate the two references (e.g., `E` for employee, `M` for manager).

---

## Practice Queries

### 1. Employee name, salary, manager name, manager's salary
```sql
SELECT E.ENAME, E.SAL, M.ENAME AS MANAGER, M.SAL AS MAN_SAL 
FROM EMP E 
JOIN EMP M ON E.MGR = M.EMPNO;
```

### 2. Employee name, job, manager name, manager's job
```sql
SELECT E.ENAME, E.JOB, M.ENAME AS MANAGER, M.JOB AS MAN_JOB 
FROM EMP E 
JOIN EMP M ON E.MGR = M.EMPNO;
```

### 3. Employee name, deptno, manager name, manager's deptno
```sql
SELECT E.ENAME, E.DEPTNO, M.ENAME AS MANAGER, M.DEPTNO 
FROM EMP E 
JOIN EMP M ON E.MGR = M.EMPNO;
```

### 4. Employee name, salary, job + manager name, salary, job
```sql
SELECT E.ENAME, E.SAL, E.JOB, M.ENAME AS MANAGER, M.SAL AS MAN_SAL, M.JOB AS MAN_JOB 
FROM EMP E 
INNER JOIN EMP M ON E.MGR = M.EMPNO;
```

### 5. Employees who earn more than their manager
```sql
SELECT E.ENAME, E.SAL, M.ENAME AS MANAGER, M.SAL AS MAN_SAL 
FROM EMP E 
JOIN EMP M ON E.MGR = M.EMPNO 
WHERE E.SAL > M.SAL;
```

### 6. Employees whose manager's job is 'MANAGER'
```sql
SELECT E.ENAME, E.JOB, M.ENAME AS MANAGER, M.JOB AS MAN_JOB 
FROM EMP E 
JOIN EMP M ON E.MGR = M.EMPNO 
WHERE M.JOB = 'MANAGER';
```

### 7. Count of employees reporting to each manager
```sql
SELECT COUNT(*), M.ENAME AS MANAGER 
FROM EMP E 
INNER JOIN EMP M ON E.MGR = M.EMPNO 
GROUP BY M.ENAME;
```

### 8. Employee + location, manager + manager's location (self join + join with DEPT)
```sql
SELECT E.ENAME, ED.LOC, M.ENAME, MD.LOC 
FROM EMP E, EMP M, DEPT ED, DEPT MD 
WHERE E.MGR = M.EMPNO 
  AND E.DEPTNO = ED.DEPTNO 
  AND M.DEPTNO = MD.DEPTNO;
```

### 9. Employee + salary grade, manager + manager's salary grade
```sql
SELECT E.ENAME, E.SAL, EG.GRADE AS EMP_GRADE, 
       M.ENAME, M.SAL, MG.GRADE AS MGR_GRADE 
FROM EMP E, EMP M, SALGRADE EG, SALGRADE MG 
WHERE E.MGR = M.EMPNO 
  AND (E.SAL BETWEEN EG.LOSAL AND EG.HISAL) 
  AND M.SAL BETWEEN MG.LOSAL AND MG.HISAL;
```
> Note: `SALGRADE` has no direct relationship column, so it's joined using the `BETWEEN` range condition (non-equi join) rather than an `=` condition.

### 10. Employee, grade, location + manager, grade, location (self join + 2 dept joins + 2 salgrade joins)
```sql
SELECT E.ENAME, E.SAL, EG.GRADE AS EMP_GRADE, ED.LOC, 
       M.ENAME, M.SAL, MG.GRADE AS MGR_GRADE, MD.LOC 
FROM EMP E, EMP M, SALGRADE EG, SALGRADE MG, DEPT ED, DEPT MD 
WHERE E.MGR = M.EMPNO 
  AND (E.SAL BETWEEN EG.LOSAL AND EG.HISAL) 
  AND M.SAL BETWEEN MG.LOSAL AND MG.HISAL 
  AND E.DEPTNO = ED.DEPTNO 
  AND M.DEPTNO = MD.DEPTNO;
```

### 11. Employee, manager, manager's manager (self join of same table 3 times)
```sql
SELECT E.ENAME, M.ENAME, MM.ENAME 
FROM EMP E, EMP M, EMP MM 
WHERE E.MGR = M.EMPNO 
  AND M.MGR = MM.EMPNO;
```

### 12. Employee, manager, manager's manager — with salary & grade for all three
```sql
SELECT E.ENAME, E.SAL, EG.GRADE AS EMP_GRADE, 
       M.ENAME, M.SAL, MG.GRADE AS MGR_GRADE, 
       MM.ENAME AS MANAGERS_MANAGER, MM.SAL, MMG.GRADE 
FROM EMP E, EMP M, EMP MM, SALGRADE EG, SALGRADE MG, SALGRADE MMG 
WHERE E.MGR = M.EMPNO 
  AND M.MGR = MM.EMPNO 
  AND (E.SAL BETWEEN EG.LOSAL AND EG.HISAL) 
  AND M.SAL BETWEEN MG.LOSAL AND MG.HISAL 
  AND MM.SAL BETWEEN MMG.LOSAL AND MMG.HISAL;
```

### 13. Employees having the same salary as another employee (duplicate salaries)
```sql
SELECT DISTINCT E.SAL 
FROM EMP E, EMP E1 
WHERE E.SAL = E1.SAL 
  AND E.EMPNO != E1.EMPNO;
```

---

## Key Points to Remember

- Self Join is not a special SQL keyword/type of join at the syntax level — it's just a **regular JOIN (or comma-style join) applied on the same table**, differentiated using **aliases**.
- Very commonly used for **hierarchical data** — e.g., employee → manager → manager's manager.
- Can be extended to **multiple levels** by joining the same table 3, 4, or more times with different aliases (see Query 11 & 12).
- When combining a self join with a **range-based table** like `SALGRADE`, use `BETWEEN` instead of `=` since there's no direct matching column — this is technically a **non-equi self join combination**.
- Query 13 is a classic self-join use case to find **duplicate values** (same salary, different employees) — `E.EMPNO != E1.EMPNO` prevents an employee from matching with themselves.
- Always use table aliases in a self join — without them, SQL cannot distinguish between the two references to the same table.