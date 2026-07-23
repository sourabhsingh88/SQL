# 📘 DDL – Data Definition Language

## 📑 Table of Contents
1. [What is DDL](#1-what-is-ddl)
2. [CREATE](#2-create)
3. [RENAME](#3-rename)
4. [ALTER](#4-alter)
5. [TRUNCATE](#5-truncate)
6. [DROP](#6-drop)
7. [FLASHBACK](#7-flashback)
8. [PURGE](#8-purge)
9. [CTAS – Copying Tables](#9-ctas--copying-tables)
10. [Creating a New User](#10-creating-a-new-user)
11. [DDL Command Summary](#11-ddl-command-summary)
12. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
13. [💼 Tricky Interview Questions](#-tricky-interview-questions)

---

## 1. What is DDL

**DDL** is a type of SQL language used to **define objects** in the database (tables, views, sequences, procedures, etc).

DDL Commands: `CREATE`, `RENAME`, `ALTER`, `TRUNCATE`, `DROP`, `FLASHBACK`, `PURGE`

> 🔑 DDL statements are **auto-committed** in Oracle — you cannot roll them back with `ROLLBACK`.

[⬆ Back to top](#-table-of-contents)

---

## 2. CREATE
`CREATE` is a DDL command used to create a table, view, sequence, procedure, etc.

**Table Creation Syntax:**
```sql
CREATE TABLE tabname (
    coln1 datatype constraint,
    coln2 datatype(size) constraint,
    ...
);
```
```sql
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(20));
-- Table created.
```

[⬆ Back to top](#-table-of-contents)

---

## 3. RENAME
`RENAME` is a DDL command used to rename a table.
```sql
RENAME oldtabname TO newtabname;
```
```sql
RENAME emp TO student;
```

[⬆ Back to top](#-table-of-contents)

---

## 4. ALTER
`ALTER` is a DDL command used to alter (modify) the structure of an existing table.

| Operation | Syntax |
|---|---|
| Add column | `ALTER TABLE tabn ADD coln datatype constraint;` |
| Rename column | `ALTER TABLE tabn RENAME COLUMN oldcoln TO newcoln;` |
| Drop column | `ALTER TABLE tabn DROP COLUMN columnname;` |
| Modify column | `ALTER TABLE tabname MODIFY colname datatype constraint;` |

```sql
-- Add column
ALTER TABLE stud ADD phno NUMBER(10);
-- Table altered

-- Modify column
ALTER TABLE stud MODIFY email VARCHAR2(25);
-- Table altered

-- Drop column
ALTER TABLE stud DROP COLUMN email;
-- Table altered
```

**Describe (check structure after ALTER):**
```sql
DESC stud;
```
| Name | Null? | Type |
|---|---|---|
| SID | NOT NULL | NUMBER |
| NAME | | VARCHAR2(20) |
| PHNO | | NUMBER(10) |

[⬆ Back to top](#-table-of-contents)

---

## 5. TRUNCATE
`TRUNCATE` deletes **all records** from a table **permanently**. It does **not** affect the structure of the table, and records **cannot be recovered**.
```sql
TRUNCATE TABLE tabname;
```
```sql
TRUNCATE TABLE stud;
-- Table truncated.

SELECT * FROM stud;
-- no rows selected
```

[⬆ Back to top](#-table-of-contents)

---

## 6. DROP
`DROP` is a DDL command used to **delete a table**. The table gets moved to the **recycle bin** (it isn't gone forever — yet).
```sql
DROP TABLE tabname;
```
```sql
DROP TABLE student;
-- Table dropped.
```

[⬆ Back to top](#-table-of-contents)

---

## 7. FLASHBACK
`FLASHBACK` is a DDL command used to **retrieve a table back from the recycle bin**.
```sql
FLASHBACK TABLE tabn TO BEFORE DROP;
```
```sql
FLASHBACK TABLE stud TO BEFORE DROP;
-- Flashback complete.
```

**Check what's in the recycle bin:**
```sql
SELECT original_name FROM recyclebin;
```
| original_name |
|---|
| stud |
| movie |
| Director |

[⬆ Back to top](#-table-of-contents)

---

## 8. PURGE
`PURGE` is a DDL command used to **permanently delete a table from the recycle bin**. The table **must be present** in the recycle bin (i.e. must have been dropped first).
```sql
PURGE TABLE tabname;
```
```sql
DROP TABLE stud;
-- Table dropped.

PURGE TABLE stud;
-- Table purged.
```

⚠️ If you try to `PURGE` a table that was **never dropped**, Oracle throws:
```text
ORA-38307: object not in RECYCLEBIN
```
👉 You must `DROP` the table first (which sends it to the recycle bin), and *then* `PURGE` it.

### Full DDL Walkthrough Example
```sql
CREATE TABLE stud (sid NUMBER PRIMARY KEY, name VARCHAR2(20));
INSERT INTO stud VALUES (101, 'raju');
INSERT INTO stud VALUES (102, 'rani');

TRUNCATE TABLE stud;          -- all rows gone, structure remains
INSERT INTO stud VALUES (101, 'raju');
INSERT INTO stud VALUES (102, 'rani');

DROP TABLE stud;              -- moved to recycle bin
SELECT original_name FROM recyclebin;

FLASHBACK TABLE stud TO BEFORE DROP;   -- restored

DROP TABLE stud;
PURGE TABLE stud;             -- gone forever now
```

[⬆ Back to top](#-table-of-contents)

---

## 9. CTAS – Copying Tables

**Copy of a table WITH records:**
```sql
CREATE TABLE tabn AS (SELECT stmt);
```
```sql
CREATE TABLE empc AS (SELECT * FROM emp);
```

**Copy of a table WITHOUT records (structure only):**
```sql
CREATE TABLE tabn AS (SELECT * FROM tabn WHERE false-condition);
```
```sql
CREATE TABLE empnorec AS (SELECT * FROM emp WHERE 1=2);
```
> The condition `1=2` is always **FALSE**, so **no records** are copied — only the table **structure** is created.

[⬆ Back to top](#-table-of-contents)

---

## 10. Creating a New User

**Step 1 — Connect to the SYSTEM account**
```sql
CONN system
-- password: tiger
```

**Step 2 — Create the user**
```sql
CREATE USER username IDENTIFIED BY pwd;
```
```sql
CREATE USER sourabh IDENTIFIED BY soura;
```

**Step 3 — Grant permissions**
```sql
GRANT CONNECT, RESOURCE TO username;
```
```sql
GRANT CONNECT, RESOURCE TO sourabh;
```

[⬆ Back to top](#-table-of-contents)

---

## 11. DDL Command Summary

| Command | Purpose | Structure affected? | Recoverable? | Auto-commits? |
|---|---|---|---|---|
| `CREATE` | Create new object | — | — | Yes |
| `RENAME` | Rename table | No | — | Yes |
| `ALTER` | Add/Modify/Drop/Rename column | Yes | — | Yes |
| `TRUNCATE` | Delete all rows, keep structure | No | ❌ Not recoverable | Yes |
| `DROP` | Delete table (→ recycle bin) | Yes | ✅ via FLASHBACK | Yes |
| `FLASHBACK` | Restore dropped table | — | — | Yes |
| `PURGE` | Permanently remove from recycle bin | Yes | ❌ Not recoverable | Yes |

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- Every DDL command **auto-commits** the current transaction — including any pending DML you hadn't committed yet!
- `TRUNCATE` resets the high-water mark and **cannot be rolled back**; `DELETE` (DML) can be rolled back.
- `DROP` doesn't erase data immediately — it goes to the **recycle bin** until purged (or purged automatically by storage policy).
- Adding a `NOT NULL` column to a table that already has rows will **fail** unless you provide a `DEFAULT` value.
- `ALTER ... MODIFY` to a smaller size will fail if existing data doesn't fit.
- Renaming a column that's referenced by a constraint, index, or view may cause errors elsewhere.
- Always double-check the table name before running `DROP` or `TRUNCATE` — there's no confirmation prompt.
- Use `PURGE` only when you are **absolutely sure** you'll never need to flashback that table.

[⬆ Back to top](#-table-of-contents)

---

## 💼 Tricky Interview Questions

**Q1. If I run `TRUNCATE` and then try `ROLLBACK`, what happens?**
Nothing changes back — `TRUNCATE` is DDL and **auto-commits immediately**. `ROLLBACK` has no effect on it. This is the #1 gotcha vs `DELETE`, which *can* be rolled back.

**Q2. If I `DELETE` all rows from a huge table vs `TRUNCATE` it, what's the practical difference?**
`DELETE` logs each row deletion (so it's slower, and rollback-able); `TRUNCATE` deallocates space directly and resets the high-water mark (faster, no rollback). If you need the ability to undo, use `DELETE`; if you need speed and don't care about undo, use `TRUNCATE`.

**Q3. If I `DROP` a table and *don't* purge it, can I still get it back a week later?**
Yes, as long as it's still sitting in the recycle bin (hasn't been auto-purged by space pressure or manually purged), `FLASHBACK TABLE ... TO BEFORE DROP;` will restore it.

**Q4. If I `PURGE` a table that was never dropped, what happens?**
Oracle throws `ORA-38307: object not in RECYCLEBIN` — because `PURGE` only works on objects that are already sitting in the recycle bin.

**Q5. If two different tables named `STUD` get dropped one after another, and I flashback, which one comes back?**
Oracle keeps **multiple versions** in the recycle bin with system-generated names. A plain `FLASHBACK TABLE stud TO BEFORE DROP;` restores the **most recently dropped** version by default; older ones remain in the recycle bin under their generated names unless explicitly flashed back too.

**Q6. If I add a column with `NOT NULL` to a table that already has 10 rows, and I don't provide a `DEFAULT`, what happens?**
The `ALTER TABLE ... ADD` statement **fails** — Oracle can't satisfy `NOT NULL` for the existing 10 rows since it doesn't know what value to put there. You must either supply a `DEFAULT` value or add the column as nullable first, populate it, then add the `NOT NULL` constraint.

**Q7. If I don't specify a `WHERE` clause equivalent limitation on `TRUNCATE` — is there a way to truncate only some rows?**
No — `TRUNCATE` **always removes all rows**, there's no `WHERE` clause option. If you need to delete only some rows, you must use `DELETE` (DML) instead.

**Q8. If I run several `INSERT`/`UPDATE` statements and then run a `CREATE TABLE` statement, what happens to my uncommitted DML?**
It gets **auto-committed** the moment the DDL statement executes — you can no longer roll it back. This surprises a lot of people who assume only explicit `COMMIT` saves data.

[⬆ Back to top](#-table-of-contents)