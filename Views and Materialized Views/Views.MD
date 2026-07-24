# 📘 Views & Materialized Views

## 📑 Table of Contents
1. [What is a View](#1-what-is-a-view)
2. [Types of Views](#2-types-of-views)
3. [View Syntax & Examples](#3-view-syntax--examples)
4. [Granting Permission to Create Views](#4-granting-permission-to-create-views)
5. [What is a Materialized View](#5-what-is-a-materialized-view)
6. [Materialized View Syntax & Example](#6-materialized-view-syntax--example)
7. [View vs Materialized View](#7-view-vs-materialized-view)
8. [Refreshing a Materialized View](#8-refreshing-a-materialized-view)
9. [📝 Practice Questions](#-practice-questions)
10. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
11. [💼 Tricky Interview Questions](#-tricky-interview-questions)

---

## 1. What is a View

A **View** is a **virtual table** built on top of a `SELECT` query. It has **no physical existence** of its own — it doesn't store any data.

- It is a **logical structure** used to represent the result of one or more base tables (often used to abstract away complex joins).
- Used for **abstraction**: hiding the internal complexity/implementation of a query, and exposing only the required functionality — the person querying the view doesn't need to know the internal joins/logic behind it.
- Used to achieve **reusability** — write the complex query once as a view, then simply `SELECT * FROM view_name` afterwards instead of rewriting the whole join/query every time.
- A view does **not require memory** to store data (it stores only the *query definition*, not the *result*).

[⬆ Back to top](#-table-of-contents)

---

## 2. Types of Views

### 1) Simple View
If the view is built on a **single query with no joins/functions/subqueries** (usually from a single table), it's called a **Simple View**.
```sql
CREATE VIEW empsel AS
SELECT ename, sal FROM emp;
```

### 2) Complex View
If the view is built using a **complex query** — involving **joins, subqueries, or functions** — it's called a **Complex View**.
```sql
CREATE VIEW emp_loc AS
SELECT e.ename, d.loc FROM emp e, dept d WHERE e.deptno = d.deptno;
```

| Simple View | Complex View |
|---|---|
| Single table | Multiple tables |
| No joins/functions/subqueries | Joins, subqueries, group functions allowed |
| Usually updatable (INSERT/UPDATE/DELETE work through it) | Often NOT directly updatable |

[⬆ Back to top](#-table-of-contents)

---

## 3. View Syntax & Examples

```sql
CREATE VIEW v_name AS
SELECT stmt;
```

**Simple view example:**
```sql
CREATE VIEW empsel AS
SELECT ename, sal FROM emp;

SELECT * FROM empsel;
```

**Complex view example (join):**
```sql
CREATE VIEW emp_loc AS
SELECT e.ename, d.loc FROM emp e, dept d WHERE e.deptno = d.deptno;

SELECT * FROM emp_loc;
```

[⬆ Back to top](#-table-of-contents)

---

## 4. Granting Permission to Create Views

Creating a view requires special system-level permission — a regular user can't create views out of the box.

```sql
-- Connect as system (admin of db, all permissions)
CONN system

-- Grant the permission
GRANT CREATE VIEW TO username;
```
```sql
GRANT CREATE VIEW TO scott;
```

[⬆ Back to top](#-table-of-contents)

---

## 5. What is a Materialized View

A **Materialized View** (often shortened as **mview**) **does have a physical existence**.

- It **is** a real database object.
- It **requires memory**, because it actually **stores data** on disk.
- It's a physical DB object that **precomputes and saves the result of a query into an actual table**.
- It stores **both** the query definition **and** the actual data (a real snapshot of the result), unlike a normal view which stores only the query.

[⬆ Back to top](#-table-of-contents)

---

## 6. Materialized View Syntax & Example

```sql
CREATE MATERIALIZED VIEW v_name AS
SELECT stmt;
```

**Granting permission (system-level, same idea as views):**
```sql
CONN system
GRANT CREATE MATERIALIZED VIEW TO username;
```
```sql
GRANT CREATE MATERIALIZED VIEW TO scott;
```

**Example:**
```sql
CREATE MATERIALIZED VIEW max_job AS
SELECT MAX(sal) AS msal, job FROM emp
GROUP BY job;

SELECT * FROM max_job;
```

[⬆ Back to top](#-table-of-contents)

---

## 7. View vs Materialized View

| | **View** | **Materialized View** |
|---|---|---|
| Physical existence | ❌ No — virtual only | ✅ Yes — a real, physical DB object |
| What it stores | Only the **query** | The query **AND** the actual **snapshot data** |
| Data source on each run | Always pulled **live** from the base table | Pulled from the **stored snapshot**, not the base table (except the very first time) |
| Speed | Slower — re-runs the underlying query every single time | Faster — data is already precomputed and sitting there |
| Reflects base table changes? | ✅ **Instantly** — always shows current base table data | ❌ **Not automatically** — needs a manual/scheduled refresh |
| Memory usage | Minimal (just stores the query text) | Higher (stores actual duplicate data) |

### Why This Trade-off Exists

**View:**
```text
exec view → goes to base table (1000 records) → filters down to 10 matched → returned
```
Every execution re-scans/re-joins the base table live. This means it's always accurate, but slower — especially for expensive joins/aggregations run repeatedly.

**Materialized View:**
```text
First time:  mat view → base table (1000 records) → stored as 10 matched records in the mview's own storage (db object)
Every time after: mat view → reads directly from its own stored 10 records (fast, but possibly stale)
```
Since it already has the filtered/aggregated result sitting in its own storage, it's much faster to read — but it won't automatically reflect new changes made to the base table.

[⬆ Back to top](#-table-of-contents)

---

## 8. Refreshing a Materialized View

Because a materialized view's stored data does **not** automatically update when the base table changes, you must manually (or via a schedule) **refresh** it:

```sql
EXEC DBMS_MVIEW.REFRESH('mview_name');
```

Until you run this, the materialized view keeps showing **old/stale data**, even though the base table has already changed.

[⬆ Back to top](#-table-of-contents)

---

## 📝 Practice Questions

**Q1. Create a view to display employee name and manager name.**
```sql
CREATE VIEW manager_view AS
(SELECT e.ename AS emp, m.ename AS manager
 FROM emp e, emp m
 WHERE e.mgr = m.empno);
```
*(This is a self-join wrapped inside a view — see the DQL notes file for more on self joins.)*

**Q2. Create a materialized view to display employee name, salary, salary grade, and location.**
```sql
CREATE MATERIALIZED VIEW empsalloc AS
(SELECT e.ename, e.sal, d.loc, s.grade
 FROM emp e, dept d, salgrade s
 WHERE e.deptno = d.deptno
 AND e.sal BETWEEN s.losal AND s.hisal);
```
*(This combines a non-equi join on `SALGRADE` with a regular equi join on `DEPT` — a good example of a complex, multi-table materialized view.)*

**Q3. Create a view to display employee details of those earning the Top 3 highest salaries.**
```sql
CREATE VIEW top3_earners AS
SELECT * FROM (
    SELECT emp.*, DENSE_RANK() OVER (ORDER BY sal DESC) AS dk FROM emp
) WHERE dk < 4;
```
*(A great example of wrapping a `DENSE_RANK()` ranking query — see the DENSE_RANK notes file — inside a reusable view.)*

**Q4. Create a view to display the total salary present in each location.**
```sql
CREATE VIEW loc_total_sal AS
SELECT d.loc, SUM(e.sal) AS total_sal
FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY d.loc;
```

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- A **View** stores **no data at all** — it's just a saved `SELECT` statement. Every time you query it, Oracle re-runs that underlying query against the live base table(s).
- A **Materialized View** stores **actual data** — it behaves like a real table with a snapshot of results, which is why it needs memory/storage and why it must be **refreshed** to stay current.
- Creating either type requires an explicit `GRANT CREATE VIEW` / `GRANT CREATE MATERIALIZED VIEW` from an admin (typically via `system`) — a plain user account can't create them by default.
- A **Simple View** built on a single table without joins/functions is generally **updatable** — `INSERT`/`UPDATE`/`DELETE` through the view can affect the base table.
- A **Complex View** (joins, `GROUP BY`, aggregate functions, `DISTINCT`) is often **NOT directly updatable** — Oracle can't always figure out which base table row an update should apply to.
- After creating a **materialized view**, changes to the base table are **silent** — no error, no warning — the mview just quietly keeps showing old data until you explicitly refresh it. This is the single most common gotcha.
- Materialized views are ideal for **expensive, repeated aggregate queries** (e.g., dashboards, reports) where slightly-stale data is an acceptable trade-off for speed.
- Regular views are ideal when you need **always up-to-date** data and the underlying query isn't too expensive to re-run each time.
- Dropping a view: `DROP VIEW view_name;` — Dropping a materialized view: `DROP MATERIALIZED VIEW mview_name;` (these are **different DROP statements** — you can't drop an mview with the plain `DROP VIEW` syntax).

[⬆ Back to top](#-table-of-contents)

---

## 💼 Tricky Interview Questions

**Q1. If I insert a new row into the base table `emp`, does a normal View on `emp` show that new row immediately?**
**Yes, instantly** — a view has no stored data of its own; every query against it re-reads the live base table, so any change to the base table is reflected the very next time the view is queried.

**Q2. If I insert a new row into the base table, does a Materialized View built on that table show the new row immediately?**
**No** — the materialized view keeps showing its **old snapshot** until someone explicitly runs `EXEC DBMS_MVIEW.REFRESH('mview_name');` (or an automatic refresh schedule kicks in, if one was configured). This is the single biggest source of "why is my report showing wrong numbers" bugs with materialized views.

**Q3. If both a View and a Materialized View are built on the exact same complex join query, which one is faster to query repeatedly, and why?**
The **Materialized View** is faster, because it already has the precomputed result physically stored — reading it is just a simple table scan of already-filtered data. The plain View has to **re-execute the entire join/aggregation** every single time it's queried, which gets expensive on large tables or complex joins.

**Q4. If I try to run `INSERT INTO` a Complex View built with a `JOIN`, what typically happens?**
It usually **fails** or is restricted — Oracle can't reliably determine which base table (and which specific row) the new data should go into when multiple tables are joined together. Simple views built on a single table are far more likely to support `INSERT`/`UPDATE`/`DELETE`.

**Q5. If I `DROP TABLE` a base table that a view depends on, what happens to the view?**
The view becomes **invalid** (it still exists as an object, but querying it throws an error) — since a view has no data of its own, it depends entirely on the base table existing. A materialized view, by contrast, still has its **last-refreshed data** physically stored, though it can no longer refresh further once its source table is gone.

**Q6. If I want a "Top N" or ranked report (like Top 3 highest earners) to always be instantly accurate, should I use a View or a Materialized View?**
A **View** — since ranking queries like `DENSE_RANK()` need to reflect the current state of the data (a new hire or a raise could change the ranking), a materialized view risks showing an outdated "Top 3" until refreshed. Use a materialized view only if slightly-stale rankings are acceptable in exchange for speed.

**Q7. If I create a Materialized View and never call `DBMS_MVIEW.REFRESH`, does it ever update on its own?**
Not unless it was created with an automatic refresh option (e.g., `ON COMMIT` or a refresh schedule/interval) — by default, without those options, it stays frozen at whatever data existed the moment it was created (or last manually refreshed).

**Q8. Why can't I just always use Materialized Views everywhere instead of regular Views, since they're faster?**
Because they trade **accuracy for speed** and cost **extra storage** — every materialized view duplicates real data on disk, and that data can silently go stale. For queries on small tables, or where up-to-the-second accuracy matters (e.g., checking current stock/inventory), a regular view (or the base table directly) is the safer choice.

**Q9. What's the difference between `DROP VIEW` and `DROP MATERIALIZED VIEW`, and what happens if you use the wrong one?**
They're **separate commands** for separate object types in Oracle's data dictionary — running `DROP VIEW` on a materialized view (or vice versa) fails with an error, because Oracle doesn't treat them as the same kind of object internally, even though they look similar conceptually.

[⬆ Back to top](#-table-of-contents)