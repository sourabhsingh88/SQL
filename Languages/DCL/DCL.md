# 📘 DCL – Data Control Language

## 📑 Table of Contents
1. [What is DCL](#1-what-is-dcl)
2. [GRANT](#2-grant)
3. [REVOKE](#3-revoke)
4. [Practical Walkthrough](#4-practical-walkthrough)
5. [DCL Command Summary](#5-dcl-command-summary)
6. [⚠️ Points to Keep in Mind](#️-points-to-keep-in-mind)
7. [💼 Tricky Interview Questions](#-tricky-interview-questions)

---

## 1. What is DCL

**DCL** is a type of SQL language used to **control the flow of data** or **grant/manage permissions** in the database.

DCL has two commands: **`GRANT`** and **`REVOKE`**

> 🔑 DCL statements, like DDL, **auto-commit** the current transaction in Oracle.

[⬆ Back to top](#-table-of-contents)

---

## 2. GRANT
Used to **give permission** to a user.
```sql
GRANT sql-statement ON tabname TO username;
```
```sql
GRANT SELECT ON emp TO hr;         -- read-only permission
GRANT UPDATE ON emp TO hr;         -- update permission
GRANT ALL ON emp TO hr;            -- all permissions
```

[⬆ Back to top](#-table-of-contents)

---

## 3. REVOKE
Used to **take back** a permission that was granted.
```sql
REVOKE sql-statement ON tabname FROM username;
```
```sql
REVOKE UPDATE ON emp FROM hr;
```

[⬆ Back to top](#-table-of-contents)

---

## 4. Practical Walkthrough

```sql
-- Logged in as SCOTT
SHOW USER;
-- USER is "SCOTT"

-- Grant SELECT permission on emp to hr
GRANT SELECT ON emp TO hr;
-- Grant succeeded.

-- Connect as hr
CONN hr/tiger
-- Connected.

-- hr can't query "emp" directly — must qualify with owner
SELECT * FROM emp;
```
```text
ORA-00942: table or view does not exist
```
```sql
-- Must reference the owning schema
SELECT * FROM scott.emp;
-- 14 rows selected
```
```sql
-- hr only has SELECT, not UPDATE — this fails
UPDATE scott.emp SET ename = 'KING' WHERE empno = 7839;
```
```text
-- No UPDATE permission granted yet
```
```sql
-- Back on SCOTT's session, grant UPDATE too
CONN scott/tiger
GRANT ALL ON emp TO hr;
-- Grant succeeded.

-- Back on hr's session
CONN hr/tiger
UPDATE scott.emp SET ename = 'KING' WHERE empno = 7839;
-- 1 row updated.

SELECT * FROM scott.emp;
-- 14 rows selected (with the update reflected)
```

[⬆ Back to top](#-table-of-contents)

---

## 5. DCL Command Summary

| Command | Purpose |
|---|---|
| `GRANT` | Give a permission to a user |
| `REVOKE` | Take back a previously granted permission |

> 💡 A granted user must reference another user's table using `schema.tablename` syntax (e.g. `scott.emp`) unless a synonym is created.

[⬆ Back to top](#-table-of-contents)

---

## ⚠️ Points to Keep in Mind

- Only the **owner** of an object (or a user with admin/`GRANT` privileges) can grant/revoke access to it.
- A grantee must reference the object using `schema.objectname` (e.g. `scott.emp`) unless a **synonym** is created for convenience.
- `GRANT ALL` gives every privilege (SELECT, INSERT, UPDATE, DELETE, etc.) — use it carefully, prefer granting only what's actually needed (principle of least privilege).
- `REVOKE` immediately removes access — any query the revoked user runs afterward on that object will fail.
- Object-level privileges (on a specific table) are different from **system-level privileges** (like `CREATE SESSION`, `CREATE TABLE`) — the new-user walkthrough in the DDL notes uses `CONNECT` and `RESOURCE` **roles**, which bundle several system privileges together.
- Permissions apply **per object**, not per column, unless you use column-level GRANT syntax (e.g. `GRANT UPDATE(sal) ON emp TO hr;`).

[⬆ Back to top](#-table-of-contents)

---

## 💼 Tricky Interview Questions

**Q1. If I query another user's table without being granted access, what error do I get?**
`ORA-00942: table or view does not exist` — Oracle deliberately gives this generic error (rather than "permission denied") so that unauthorized users can't even confirm whether the table exists.

**Q2. If I've been granted `SELECT` on a table but not `UPDATE`, and I try to update it, what happens?**
The statement fails with an insufficient privileges error (`ORA-01031: insufficient privileges`). You can only perform the operations you were explicitly granted.

**Q3. If the owner (grantor) of a table is dropped or their account is deleted, what happens to the permissions they granted to others?**
The grants tied to that owner's objects typically become invalid/are cascaded away since the underlying object no longer exists — access is lost. (In real DBA scenarios, dropping a schema also drops its objects, which removes any grants pointing to them.)

**Q4. If I `REVOKE` a permission from a user while they're in the middle of using it (mid-session), does it apply instantly?**
Yes, DCL commands like `REVOKE` take effect **immediately** for any *new* statement — though a currently *in-flight* long-running statement usually still completes; the next new query, however, is blocked.

**Q5. If I forget to prefix a granted table with the owner's schema name, what happens even though I *do* have access?**
It **still fails** with `ORA-00942`, because Oracle resolves unqualified table names against your **own schema first** — not the grantor's. You must always use `schema.tablename` (or set up a synonym) when accessing another user's object.

**Q6. If I grant `CONNECT` and `RESOURCE` roles to a new user but don't grant any object privileges, can they query `emp` in another schema?**
No. `CONNECT`/`RESOURCE` are **system-level roles** that let the user log in and create their own objects (tables, sequences, etc.) — they do **not** give access to another user's specific tables. You'd still need an explicit `GRANT SELECT ON emp TO newuser;` from the table's owner.

**Q7. If I grant a privilege using `GRANT ... WITH GRANT OPTION`, what changes?**
The grantee can then **re-grant** that same privilege to other users themselves — without this option, only the original owner (or a DBA) can grant it further.

[⬆ Back to top](#-table-of-contents)