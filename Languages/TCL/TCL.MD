# 📘 TCL – Transaction Control Language

## 📑 Table of Contents
1. [What is TCL](#1-what-is-tcl)
2. [COMMIT](#2-commit)
3. [ROLLBACK](#3-rollback)
4. [SAVEPOINT](#4-savepoint)
5. [Example 1 — Basic Commit & Rollback](#5-example-1--basic-commit--rollback)
6. [Example 2 — Multiple Savepoints](#6-example-2--multiple-savepoints)
7. [TCL Command Summary](#7-tcl-command-summary)
8. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
9. [💼 Tricky Interview Questions](#-tricky-interview-questions)

---

## 1. What is TCL

**TCL** is a type of SQL language used to **control the transactions** done on the database.

TCL consists of: **`COMMIT`**, **`ROLLBACK`**, **`SAVEPOINT`**

> 🔑 TCL commands only make sense in the context of **DML** (`INSERT`/`UPDATE`/`DELETE`), because DDL and DCL statements auto-commit and can't be rolled back anyway.

[⬆ Back to top](#-table-of-contents)

---

## 2. COMMIT
`COMMIT` is a TCL command used to **save data changes permanently**.
```sql
COMMIT;
```

[⬆ Back to top](#-table-of-contents)

---

## 3. ROLLBACK
`ROLLBACK` is a TCL command used to **undo transactions** back to the last `COMMIT` or a specific `SAVEPOINT` address.
```sql
ROLLBACK;                  -- undo everything since last COMMIT
ROLLBACK TO savepointname; -- undo everything since that SAVEPOINT
```

[⬆ Back to top](#-table-of-contents)

---

## 4. SAVEPOINT
`SAVEPOINT` is a **temporary memory address** used to mark a point in a transaction — you can `ROLLBACK` to that exact point later.
```sql
SAVEPOINT savepointname;
```

[⬆ Back to top](#-table-of-contents)

---

## 5. Example 1 — Basic Commit & Rollback

```sql
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(10));

INSERT INTO stud VALUES (101, 'ramesh');
INSERT INTO stud VALUES (102, 'ramya');
COMMIT;
-- Commit complete.

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |

```sql
INSERT INTO stud VALUES (103, 'mahesh');
INSERT INTO stud VALUES (104, 'manisha');

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |

```sql
ROLLBACK;
-- Rollback complete.

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |

> The two uncommitted inserts (103, 104) disappeared because `ROLLBACK` undid everything since the last `COMMIT`.

[⬆ Back to top](#-table-of-contents)

---

## 6. Example 2 — Multiple Savepoints

```sql
INSERT INTO stud VALUES (103, 'mahesh');
SAVEPOINT s1;
-- Savepoint created.

INSERT INTO stud VALUES (104, 'manisha');
SAVEPOINT s2;

INSERT INTO stud VALUES (105, 'suresh');
SAVEPOINT s3;

INSERT INTO stud VALUES (106, 'sunitha');
SAVEPOINT s4;

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |
| 105 | suresh |
| 106 | sunitha |

```sql
-- Roll back everything after savepoint s3 (undoes s4's insert only)
ROLLBACK TO s3;

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |
| 105 | suresh |

```sql
-- Roll back everything after savepoint s2 (undoes s3's insert too)
ROLLBACK TO s2;

SELECT * FROM stud;
```
| sid | name |
|---|---|
| 101 | ramesh |
| 102 | ramya |
| 103 | mahesh |
| 104 | manisha |

> 💡 `ROLLBACK TO <savepoint>` undoes everything done **after** that savepoint, but keeps everything done **up to and including** it.

[⬆ Back to top](#-table-of-contents)

---

## 7. TCL Command Summary

| Command | Purpose |
|---|---|
| `COMMIT` | Permanently save all changes |
| `ROLLBACK` | Undo changes since last COMMIT/SAVEPOINT |
| `SAVEPOINT` | Mark a point to roll back to later |

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- `COMMIT` makes changes **permanent and visible to all other sessions** — there's no going back after that.
- `SAVEPOINT`s only exist **within the current, uncommitted transaction** — running `COMMIT` (or any DDL/DCL) **erases all savepoints**.
- `ROLLBACK` (with no savepoint name) undoes the **entire** current transaction back to the last `COMMIT`.
- You can create **multiple savepoints** and roll back to any of them, but rolling back to an earlier savepoint automatically discards any savepoints created after it.
- Closing a session (or a crash) **without committing** results in an automatic rollback of all pending changes.
- TCL commands only affect **DML** — they have no effect on DDL/DCL, which are already auto-committed by the time you'd try to roll them back.

[⬆ Back to top](#-table-of-contents)

---

## 💼 Tricky Interview Questions

**Q1. If I insert 5 rows, create a savepoint, insert 3 more rows, and then run `ROLLBACK` (no savepoint name), what remains?**
**Nothing from this transaction** — a plain `ROLLBACK` ignores savepoints entirely and undoes everything back to the last `COMMIT`. Savepoints only matter when you explicitly say `ROLLBACK TO savepointname`.

**Q2. If I create `SAVEPOINT s1`, then insert more rows, then run `COMMIT`, can I still `ROLLBACK TO s1`?**
**No.** `COMMIT` wipes out all savepoints from that transaction. Once committed, that data is permanent, and there's nothing left to roll back to.

**Q3. If I run a `CREATE TABLE` (DDL) statement in the middle of my DML transaction, what happens to my uncommitted inserts?**
They get **auto-committed** immediately, because DDL statements implicitly commit the current transaction in Oracle. Your savepoints from before that DDL are also wiped out.

**Q4. If I close my SQL client (or my session disconnects) without running `COMMIT`, what happens to my changes?**
They are **automatically rolled back**. Nothing is saved unless `COMMIT` was explicitly executed (or your tool has auto-commit mode enabled, in which case each statement commits itself).

**Q5. If I run `ROLLBACK TO s2` and then later want to redo what was undone by that rollback (i.e., get back to s3's state), is that possible?**
**No** — once you roll back to `s2`, the changes after it are gone; there's no "redo." You'd have to re-run those INSERT/UPDATE statements again.

**Q6. If two different users are working on the same table simultaneously, and User A hasn't committed yet, can User B see User A's changes?**
**No.** Oracle's transaction isolation ensures User B only sees committed data (unless using special isolation levels). User B sees the old data until User A commits.

**Q7. If I run `SAVEPOINT s1;` twice with the same name, what happens?**
Oracle just **moves the savepoint marker** `s1` to the new, later position in the transaction — it doesn't throw an error, and rolling back to `s1` will now only undo statements after the *second* `SAVEPOINT s1;` call.

**Q8. If a statement inside my transaction fails (e.g., a constraint violation) partway through, does the whole transaction auto-rollback?**
No — Oracle **only rolls back that individual failed statement**; earlier successful statements in the same transaction remain pending (uncommitted) unless you explicitly `ROLLBACK` or `COMMIT` them.

[⬆ Back to top](#-table-of-contents)