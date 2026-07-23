# 📘 DENSE_RANK() – Window / Ranking Function

## 📑 Table of Contents
1. [What is DENSE_RANK](#1-what-is-dense_rank)
2. [Syntax](#2-syntax)
3. [Worked Example](#3-worked-example)
4. [DENSE_RANK vs RANK vs ROW_NUMBER](#4-dense_rank-vs-rank-vs-row_number)
5. [🧠 How to Build These Queries Without Guessing](#-how-to-build-these-queries-without-guessing)
6. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
7. [📝 Practice Questions](#-practice-questions)
8. [💼 Interview Questions](#-interview-questions)

---

## 1. What is DENSE_RANK

`DENSE_RANK()` is a **window function** (also called a **ranking function**) used to **assign a rank** to each row based on **ascending or descending order** of a given column.

- It's called a "window" function because it operates on a **window/slice** of rows (defined by `PARTITION BY`) without collapsing them into a single row — unlike `GROUP BY`, every original row still comes back, just with an extra rank column attached.
- Unlike normal aggregate functions (`SUM`, `AVG`, `COUNT`), **rows are not merged/grouped** — each row keeps its own identity, but gets a rank number.
- **"Dense"** means there are **no gaps** in the ranking sequence — if two rows tie for rank 3, the next distinct value gets rank 4 (not rank 5).

[⬆ Back to top](#-table-of-contents)

---

## 2. Syntax

```sql
DENSE_RANK() OVER (
    PARTITION BY col      -- optional: restart ranking within each group
    ORDER BY col ASC/DESC -- required: defines what "rank 1" means
)
```

| Part | Meaning |
|---|---|
| `PARTITION BY col` | **Optional.** Works like `GROUP BY`, but doesn't collapse rows — it restarts the ranking counter for each group/partition. |
| `ORDER BY col ASC/DESC` | **Required.** Defines the sort order used to assign ranks. `DESC` → highest value gets rank 1. `ASC` → lowest value gets rank 1. |

> Without `PARTITION BY`, ranking runs across the **entire result set** as one single group.

[⬆ Back to top](#-table-of-contents)

---

## 3. Worked Example

Given a table of salaries (with a duplicate value, to show how ties are handled):

| sal |
|---|
| 700 |
| 500 |
| 400 |
| 500 |
| 800 |

Query:
```sql
SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dr
FROM emp;
```

Result (sorted by salary descending):

| sal | dense_rank |
|---|---|
| 800 | 1 |
| 700 | 2 |
| 500 | 3 |
| 500 | 3 |
| 400 | 4 |

👉 Both rows with `sal = 500` get rank **3** (a tie), and the very next distinct value (`400`) still gets rank **4**, not 5. That's the "dense" (no-gap) behaviour.

**Using it to filter — find the row(s) with the 3rd highest salary:**
```sql
SELECT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk
    FROM emp
)
WHERE dk = 3;
```

> ⚠️ You **cannot** filter directly with `WHERE dk = 3` in the same query where you compute `DENSE_RANK()` — window functions are evaluated **after** `WHERE`, so you must wrap it in an **inline view (subquery)** and filter in the outer query.

[⬆ Back to top](#-table-of-contents)

---

## 4. DENSE_RANK vs RANK vs ROW_NUMBER

| Function | Behaviour on ties | Gaps after a tie? |
|---|---|---|
| `ROW_NUMBER()` | Gives every row a **unique** number, even ties get different numbers | — |
| `RANK()` | Ties get the **same** rank | ✅ Yes — skips the next rank(s) (e.g. 1,2,2,4) |
| `DENSE_RANK()` | Ties get the **same** rank | ❌ No gap — next value continues immediately (e.g. 1,2,2,3) |

**Same data, side by side:**

| sal | ROW_NUMBER | RANK | DENSE_RANK |
|---|---|---|---|
| 800 | 1 | 1 | 1 |
| 700 | 2 | 2 | 2 |
| 500 | 3 | 3 | 3 |
| 500 | 4 | 3 | 3 |
| 400 | 5 | 5 | 4 |

> 💡 **Interview favorite:** "If I want the true Nth *distinct* value (e.g. 3rd highest **unique** salary, counting duplicates as one), use `DENSE_RANK`. If I want the Nth *row* regardless of duplicates, use `ROW_NUMBER`. If I want competition-style ranking (like Olympic medals, where two golds mean no silver), use `RANK`."

[⬆ Back to top](#-table-of-contents)

---

## 5. 🧠 How to Build These Queries Without Guessing

This is the exact skill that fixes "I can't write it directly, I keep needing to test again and again." Don't try to write the whole nested query in one shot — build it in **layers**, testing each layer before adding the next.

### Step 1 — Write and run the PLAIN query first
Forget ranking for a second. Just get the raw data right.
```sql
SELECT sal FROM emp;
```
Run it. Does the data look right? Good, move on.

### Step 2 — Add ORDER BY and eyeball the order
```sql
SELECT sal FROM emp ORDER BY sal DESC;
```
Run it. Confirm the sort direction is what you actually want (`DESC` for highest-first, `ASC` for lowest-first). This is the #1 source of "wrong Nth value" bugs.

### Step 3 — Add DENSE_RANK() as an extra column, don't filter yet
```sql
SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk
FROM emp;
```
Run it and **look at the `dk` column with your own eyes**. Confirm rank 1 is where you expect, confirm ties share a rank, confirm no gaps. Don't skip this step — this is where 90% of bugs get caught early.

### Step 4 — ONLY NOW wrap it as a subquery and filter
```sql
SELECT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk
    FROM emp
) WHERE dk = 3;
```

### Step 5 — If you need it PER GROUP, add PARTITION BY and re-test from Step 3
```sql
SELECT sal, deptno, DENSE_RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS dk
FROM emp;
```
Look at the output again — does the rank column **reset back to 1** every time `deptno` changes? If yes, you're ready to filter with `WHERE dk = 2` in an outer query.

### Golden Rule
> **Never write the WHERE-filtered outer query and the DENSE_RANK() inner query at the same time.** Always get the inner `SELECT ... , DENSE_RANK() OVER (...)` running and *visually verified* on its own first. Only wrap it once you trust it.

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- `DENSE_RANK()` (like all window functions) **cannot be used directly in a `WHERE` clause** — `WHERE` runs before window functions are calculated. You must compute it in a subquery/inline view and filter in the **outer** query.
- `DENSE_RANK()` **cannot be used in `GROUP BY` or `HAVING`** either, for the same reason — it's evaluated after grouping/aggregation.
- Always double check `ASC` vs `DESC` in `ORDER BY` — this single word decides whether rank 1 means "highest" or "lowest."
- `PARTITION BY` **resets** the ranking counter for each group; without it, ranking runs across the whole table as one group.
- If you want the **Nth highest/lowest**, use `DENSE_RANK` (duplicates count once). If you want the **Nth row**, use `ROW_NUMBER` (duplicates count separately).
- `DENSE_RANK() = N` can return **more than one row** if there's a tie at that rank — don't assume it always returns exactly one row.
- Combine with `DISTINCT` in the outer query if you only care about the ranked value itself and not duplicate rows tied at the same rank.
- Window functions can be combined with aggregates (e.g. `COUNT(*)` + `DENSE_RANK()`) to rank **groups** — very common for "top N department by employee count" style questions.
- Test the inner query **standalone** before nesting it — see the [step-by-step method above](#-how-to-build-these-queries-without-guessing).

[⬆ Back to top](#-table-of-contents)

---

## 📝 Practice Questions

> All questions use the classic `emp`/`dept` schema. Try writing the inner ranking query yourself first, then check.

**Q1. Find the 4th lowest salary.**
```sql
SELECT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal ASC) AS dk FROM emp
) WHERE dk = 4;
```

**Q2. Find the 2nd, 4th, and 5th highest salary.**
```sql
SELECT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk FROM emp
) WHERE dk IN (2,4,5);
```

**Q3. Find the 1st, 3rd, and 4th lowest salary.**
```sql
SELECT DISTINCT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal ASC) AS dk FROM emp
) WHERE dk IN (1,3,4);
```

**Q4. Find employee details of those earning the 3rd or 5th highest salary.**
```sql
SELECT * FROM emp WHERE sal IN (
    SELECT sal FROM (
        SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk FROM emp
    ) WHERE dk IN (3,5)
);
```

**Q5. Find employee details of those earning the 2nd or 5th lowest salary.**
```sql
SELECT * FROM (
    SELECT emp.*, DENSE_RANK() OVER (ORDER BY sal ASC) AS dk FROM emp
) WHERE dk IN (2,5);
```

**Q6. Find the Top 3 highest salaries.**
```sql
SELECT DISTINCT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk FROM emp
) WHERE dk < 4;
```

**Q7. Find the Top 3 lowest salaries.**
```sql
SELECT DISTINCT * FROM (
    SELECT sal, DENSE_RANK() OVER (ORDER BY sal ASC) AS dk FROM emp
) WHERE dk < 4;
```

**Q8. Find employee details of those earning the Top 3 highest salaries.**
```sql
SELECT * FROM (
    SELECT emp.*, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk FROM emp
) WHERE dk < 4;
```

**Q9. Find the max salary in each job.**
```sql
SELECT job, MAX(sal) FROM emp GROUP BY job;
```
*(No ranking needed — plain `GROUP BY` + `MAX` is enough here.)*

**Q10. Find the max salary in each department.**
```sql
SELECT deptno, MAX(sal) FROM emp GROUP BY deptno;
```

**Q11. Find the 2nd max salary in each department.**
```sql
SELECT * FROM (
    SELECT emp.*, DENSE_RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS dk FROM emp
) WHERE dk = 2;
```

**Q12. Find the 2nd max salary in each job.**
```sql
SELECT * FROM (
    SELECT emp.*, DENSE_RANK() OVER (PARTITION BY job ORDER BY sal DESC) AS dk FROM emp
) WHERE dk = 2;
```

**Q13. Find the number of employees working in each location.**
```sql
SELECT d.loc, COUNT(*) FROM emp e, dept d WHERE e.deptno = d.deptno GROUP BY d.loc;
```
*(No ranking needed.)*

**Q14. Find the department/location where the highest number of employees are working.**
```sql
SELECT * FROM (
    SELECT d.loc, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) DESC) AS dk
    FROM emp e, dept d WHERE e.deptno = d.deptno
    GROUP BY d.loc
) WHERE dk = 1;
```

**Q15. Find the location where the highest number of employees are working (with department breakdown).**
```sql
SELECT * FROM (
    SELECT d.deptno, d.loc, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) DESC) AS dk
    FROM emp e, dept d WHERE e.deptno = d.deptno
    GROUP BY d.deptno, d.loc
) WHERE dk = 1;
```

**Q16. Find the location where the least number of employees are working.**
```sql
SELECT * FROM (
    SELECT d.deptno, d.loc, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) ASC) AS dk
    FROM emp e, dept d WHERE e.deptno = d.deptno
    GROUP BY d.deptno, d.loc
) WHERE dk = 1;
```

**Q17. Find the manager who has the highest number of reporting employees.**
```sql
SELECT * FROM (
    SELECT m.empno, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) DESC) AS dk
    FROM emp e, emp m WHERE e.mgr = m.empno
    GROUP BY m.empno
) WHERE dk = 1;
```

**Q18. Find the managers who have at least 3 employees reporting to them.**
```sql
SELECT e.mgr, COUNT(*) FROM emp e, emp m WHERE e.mgr = m.empno
GROUP BY e.mgr HAVING COUNT(*) >= 3;
```
*(Just `GROUP BY` + `HAVING` — DENSE_RANK isn't needed for an "at least N" style question, only for "top N" or "Nth" questions.)*

**Q19. Find the Top 2 locations with the highest number of employees working.**
```sql
SELECT * FROM (
    SELECT d.loc, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) DESC) AS dk
    FROM emp e, dept d WHERE e.deptno = d.deptno
    GROUP BY d.loc
) WHERE dk <= 2;
```

**Q20. Find the Top 2 designations (jobs) with the highest number of employees.**
```sql
SELECT * FROM (
    SELECT job, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) DESC) AS dk
    FROM emp GROUP BY job
) WHERE dk <= 2;
```

**Q21. Find the job with the least number of employees working.**
```sql
SELECT * FROM (
    SELECT job, COUNT(*), DENSE_RANK() OVER (ORDER BY COUNT(*) ASC) AS dk
    FROM emp GROUP BY job
) WHERE dk = 1;
```

[⬆ Back to top](#-table-of-contents)

---

## 💼 Interview Questions

**Q1. If I write `SELECT sal, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk FROM emp WHERE dk = 1;` directly, what happens?**
It **fails** with an error, because `dk` doesn't exist yet at the point `WHERE` is evaluated — window functions execute **after** `WHERE`/`GROUP BY`/`HAVING`, so `dk` is only available at the `SELECT`/`ORDER BY` stage. You must wrap the ranking query in a subquery and filter in the **outer** `SELECT`.

**Q2. If two employees are tied for the 2nd highest salary, and I filter `WHERE dk = 2`, how many rows come back?**
**Both** of them — `DENSE_RANK` gives ties the same rank number, so any query filtering on that rank returns every row sharing it, not just one.

**Q3. If I use `RANK()` instead of `DENSE_RANK()` for the same "3rd highest salary" question, could I get a different (wrong) answer?**
Yes — if there's a tie for 2nd place, `RANK()` will **skip** rank 3 entirely (e.g., 1,2,2,4), so filtering `WHERE rnk = 3` returns **nothing**, even though there clearly is a "3rd highest distinct value" conceptually. `DENSE_RANK` avoids this by not leaving gaps.

**Q4. If I forget `PARTITION BY` when I actually wanted "2nd highest salary per department," what happens?**
The ranking runs across the **entire table** as a single group instead of restarting per department — you'll get the overall 2nd highest salary company-wide, and most departments will show **no rows at all** for `dk = 2`, since only one or two departments actually contain the company's 2nd-highest earner.

**Q5. If I use `ORDER BY sal ASC` when I meant to find the highest salary, what's rank 1?**
Rank 1 becomes the **lowest** salary, not the highest — a very common accidental-bug source. Always double-check `ASC`/`DESC` matches the actual question ("highest" → `DESC`, "lowest" → `ASC`).

**Q6. Can I use `DENSE_RANK()` inside a `HAVING` clause directly?**
No — same reason as `WHERE`: window functions are computed **after** grouping/aggregation/HAVING, so you cannot reference the rank column inside `HAVING` in the same query level. Wrap it in a subquery first.

**Q7. If my table has only 2 distinct salary values but I ask for the "3rd highest salary," what does the query return?**
**No rows** — there simply is no rank 3 to match, so the outer `WHERE dk = 3` filters everything out. This is expected behavior, not a bug.

**Q8. What's the difference between using `DENSE_RANK` vs simply using `LIMIT`/`ROWNUM` with `ORDER BY` to get the "Nth highest" value?**
`ROWNUM`/`LIMIT`-based approaches give you the Nth **row**, which breaks silently if there are duplicate values (you might get a duplicate value's row instead of the true Nth *distinct* value). `DENSE_RANK` correctly treats duplicate values as a single rank, giving you the true Nth-distinct answer, and can also *correctly* return multiple tied rows when appropriate.

**Q9. If I combine `DENSE_RANK()` with `COUNT(*)` and `GROUP BY` in the same subquery, does it rank the raw rows or the grouped result?**
It ranks the **grouped/aggregated result** — since `GROUP BY` runs before window functions are applied, `DENSE_RANK()` sees one row per group (e.g., one row per `deptno` with its `COUNT(*)`), and ranks those group-level rows.

**Q10. Why must you test the inner `SELECT ..., DENSE_RANK() OVER (...)` query on its own before wrapping it in an outer filter?**
Because if the ranking logic itself is wrong (wrong `ORDER BY` direction, missing `PARTITION BY`, wrong column), the outer query will still run **without any error** and just silently return the wrong rows — there's no error message to catch these logic mistakes, so you have to visually verify the rank column yourself before trusting it.

[⬆ Back to top](#-table-of-contents)