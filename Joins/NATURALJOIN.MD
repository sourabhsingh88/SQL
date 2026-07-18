# SQL – Natural Join

## What is a Join?
A **Join** is used to retrieve data from two or more tables based on a related column between them.

## What is Natural Join?
A type of join used to retrieve data from two or more tables **without needing an explicit joining condition**.

- No joining condition (`ON` clause) is required.
- SQL automatically joins the tables based on **columns with the same name and same datatype** present in both tables.

---

## How Natural Join Behaves

| Condition | Behaviour |
|---|---|
| If there **is** a relationship between the tables (common column) | Returns data in the form of an **INNER JOIN** |
| If there is **no direct relationship** between the tables (no common column) | Returns data in the form of a **CROSS JOIN** (n × m — every row of table1 combined with every row of table2) |

---

## Syntax

⚠️ Natural Join does **not have Oracle-specific syntax** — it is only available in **ANSI SQL** syntax.

```sql
SELECT * FROM TAB1 NATURAL JOIN TAB2 WHERE COND;
```

---

## Examples of Behaviour

**Case 1 – Relationship exists (acts as INNER JOIN):**
```sql
SELECT ENAME, LOC FROM EMP NATURAL JOIN DEPT;
```
`EMP` and `DEPT` both have a common column (`DEPTNO`), so this works like an INNER JOIN.

**Case 2 – No direct relationship (acts as CROSS JOIN):**
```sql
SELECT * FROM EMP NATURAL JOIN SALGRADE;
```
`EMP` and `SALGRADE` have no common column name, so this returns a **CROSS JOIN** result (every row of EMP combined with every row of SALGRADE).

---

## Practice Queries

**1. Display ENAME, SAL, LOC where SAL > 2000 using Natural Join**
```sql
SELECT E.ENAME, E.SAL, D.LOC 
FROM EMP E NATURAL JOIN DEPT D 
WHERE SAL > 2000;
```

**2. Display ENAME, JOB, DEPTNAME where JOB is SALESMAN or MANAGER**
```sql
SELECT E.ENAME, E.JOB, D.DNAME 
FROM EMP E NATURAL JOIN DEPT D 
WHERE JOB IN ('SALESMAN', 'MANAGER');
```

**3. Display ENAME, SAL, GRADE using Natural Join**
```sql
SELECT E.ENAME, E.SAL, G.GRADE 
FROM EMP E NATURAL JOIN SALGRADE G 
WHERE E.SAL BETWEEN G.LOSAL AND G.HISAL;
```
> Note: Since `EMP` and `SALGRADE` don't share a common column, a plain `NATURAL JOIN` between them behaves as a cross join. Here the `WHERE` condition (`SAL BETWEEN LOSAL AND HISAL`) is what actually filters it down to the correct grade — this is effectively acting like a non-equi join, not a true natural join match.

---

## Key Points to Remember

- Natural Join uses **all columns with matching names and datatypes** across both tables to join automatically.
- We do **not** write an `ON` or explicit join condition.
- If tables have **multiple** matching columns, all of them are used for the join — this can sometimes cause **unexpected/incorrect results**, especially if a common column name is not actually meant to be a relationship (e.g., both tables have a column named `ID` for different purposes).
- Because of this unpredictability, Natural Join is **used less frequently** in real-world queries compared to `INNER JOIN ... ON` or `JOIN ... USING`.
- Available only in **ANSI syntax**, not in Oracle's proprietary join syntax (`WHERE table1.col = table2.col`).

---

## Natural Join vs Other Joins (Quick Comparison)

| Join Type | Condition Needed? | Matches On |
|---|---|---|
| **NATURAL JOIN** | No | All columns with same name & datatype (automatic) |
| **INNER JOIN ... ON** | Yes | Explicit condition specified by programmer |
| **JOIN ... USING(col)** | Yes (column name only) | Specified common column name |
| **CROSS JOIN** | No | No matching — returns Cartesian product (n × m) |