# 📘 DML – Data Manipulation Language

## 📑 Table of Contents
1. [What is DML](#1-what-is-dml)
2. [INSERT](#2-insert)
3. [UPDATE](#3-update)
4. [DELETE](#4-delete)
5. [DML Command Summary](#5-dml-command-summary)
6. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
7. [💼 Tricky Interview Questions](#-tricky-interview-questions)

---

## 1. What is DML

**DML** is a type of SQL language used to **manipulate data** stored in tables.

DML Commands: `INSERT`, `UPDATE`, `DELETE`

> 🔑 Unlike DDL, DML statements are **NOT auto-committed** — changes stay temporary until you run `COMMIT` (see the TCL notes file), and can be undone with `ROLLBACK`.

[⬆ Back to top](#-table-of-contents)

---

## 2. INSERT
`INSERT` is a DML command used to insert records into a table.
```sql
INSERT INTO tabn VALUES (v1, v2, ...);
INSERT INTO tabn (col1, col2) VALUES (v1, v2, ...);
```
```sql
INSERT INTO stud VALUES (101, 'ramesh');
INSERT INTO stud (sid, name) VALUES (102, 'ramya');
```

[⬆ Back to top](#-table-of-contents)

---

## 3. UPDATE
`UPDATE` is a DML command used to update existing records.
⚠️ **Always use a `WHERE` condition** — otherwise every row in the table gets updated.
```sql
UPDATE tabn SET col=val, col=val, ...
WHERE cond;
```
```sql
-- Increase salary by 10% for clerks
UPDATE emp SET sal = sal + sal * 0.10 WHERE job = 'CLERK';
```

**More examples:**
```sql
-- Give a flat bonus of 300
UPDATE emp SET sal = sal + 300 WHERE deptno = 10;

-- Decrease salary by 30% for a department
UPDATE emp SET sal = sal - (sal * 0.30) WHERE deptno = 40;
```

[⬆ Back to top](#-table-of-contents)

---

## 4. DELETE
`DELETE` is a DML command used to delete records.
⚠️ **Always use a `WHERE` condition** — otherwise every row is deleted.
```sql
DELETE FROM tabn WHERE cond;
```
```sql
DELETE FROM emp WHERE job = 'CLERK';
```

[⬆ Back to top](#-table-of-contents)

---

## 5. DML Command Summary

| Command | Purpose | WHERE mandatory? | Rollback possible? | Auto-commits? |
|---|---|---|---|---|
| `INSERT` | Add new records | No | ✅ Yes | No |
| `UPDATE` | Modify existing records | ⚠️ Strongly recommended | ✅ Yes | No |
| `DELETE` | Remove records | ⚠️ Strongly recommended | ✅ Yes | No |

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- **Always** write a `WHERE` clause with `UPDATE`/`DELETE` unless you genuinely intend to affect every row.
- DML changes are only **temporary** until `COMMIT` is run — until then, they're only visible in your own session (others see the old data).
- Inserting into a `NOT NULL` column without a value, or violating a `UNIQUE`/`PRIMARY KEY`/`FOREIGN KEY` constraint, causes the statement to fail — the row is simply **not inserted**.
- Column order in `INSERT INTO tabn VALUES(...)` (without naming columns) **must exactly match** the table's column order.
- `UPDATE` can reference the **current value of a column** in its own `SET` clause (e.g. `sal = sal + 100`), which is very common for raises/bonuses.
- A `DELETE` without a `WHERE` clause removes all rows one by one (slow, but rollback-able) — different from `TRUNCATE` (fast, not rollback-able).
- Run a `SELECT` with the same `WHERE` condition first to preview which rows will be affected, **before** running `UPDATE`/`DELETE`.

[⬆ Back to top](#-table-of-contents)

---

## 💼 Tricky Interview Questions

**Q1. If I run `UPDATE emp SET sal = 5000;` without a `WHERE` clause, what happens?**
**Every single row** in the `emp` table gets its salary set to 5000. This is one of the most common (and dangerous) real-world mistakes — always double check for a `WHERE` clause before running `UPDATE`.

**Q2. If I run `DELETE FROM emp;` without a `WHERE` clause, is it the same as `TRUNCATE TABLE emp;`?**
The **end result looks the same** (empty table), but they behave very differently: `DELETE` is DML (row-by-row, logged, **rollback-able**, doesn't reset storage/high-water mark, fires triggers), while `TRUNCATE` is DDL (**auto-committed**, cannot be rolled back, resets storage, doesn't fire row-level triggers).

**Q3. If I insert a row and then close my session **without** running `COMMIT`, what happens to that row?**
It gets **automatically rolled back** by Oracle when the session ends (unless auto-commit is enabled in your client tool). The insert never becomes permanent.

**Q4. If I try to insert a duplicate value into a `PRIMARY KEY` or `UNIQUE` column, what happens?**
The `INSERT` statement **fails** with a constraint violation error (e.g. `ORA-00001: unique constraint violated`) and the row is not added. Any *other* rows already inserted earlier in the same transaction remain (uncommitted) unless you also roll those back.

**Q5. If I try to insert a child row referencing a parent key that doesn't exist (with a `FOREIGN KEY` constraint), what happens?**
Oracle **rejects the insert** with a foreign key constraint violation (`ORA-02291: integrity constraint violated - parent key not found`).

**Q6. If I `UPDATE` a row and then run another `SELECT` in a *different* session before committing, will the second session see the change?**
**No.** Uncommitted changes are only visible in the session that made them. Other sessions continue to see the old (pre-update) data until you `COMMIT`.

**Q7. If I provide fewer values than columns in an `INSERT INTO tabn VALUES(...)` statement, what happens?**
It **fails** with `ORA-00947: not enough values` — unless you explicitly name the columns you're providing values for (`INSERT INTO tabn(col1,col2) VALUES(v1,v2)`), in which case the unlisted columns get `NULL` (or their `DEFAULT`) automatically.

**Q8. If a table has a `DEFAULT` value defined on a column, and I don't mention that column in my `INSERT`, what value gets stored?**
The column's **default value** is used automatically, rather than `NULL`.

[⬆ Back to top](#-table-of-contents)