# DBMS & SQL – Basic Notes

## Data
Raw facts which describe attributes of an entity/object. Unprocessed / unorganised / unfiltered.
- Data is used to describe the attributes/properties of an object.
- Ex: Bus → seat, ticket, AC/NAC (object → attributes)
- Attributes depend on the entity & situation.
- Data can be valid or invalid based on the situation.

## Information
Organised or processed or filtered data.

## Database
A container to store interrelated data.

## SQL
SQL is a query language used to interact with a database.
- First name of SQL was **SEQUEL** (Structured English Query Language)
- SQL introduced by **Raymond Boyce** and **Donald Chamberlin**

## DBMS
DBMS is a software used to manage & maintain a database using a language (SQL) by performing CRUD operations.

---

## Data Models

### 1) Relational Model
A type of data model used to store data in the form of a **table**.
- Table = combination of rows and columns
- **Row** → representation of all the attributes of an entity/table; also called a **tuple**
- **Column** → representation of a single attribute of an entity/table; also called an **attribute**
- **Cell** → intersection space between a row and a column
- Relational model was introduced by **E.F. Codd**

Example table:
| id | name | age |
|----|------|-----|
| 101 | dingu | 21 |
| 102 | dingi | 16 |
| 103 | raju | 25 |
| 104 | chutki | 18 |

### 2) Document Model
A type of data model used to store data in the form of **JSON** and **XML**.

**JSON** – JavaScript Object Notation
```json
{
  "id": 101,
  "name": "Raju",
  "age": 21
}
```
(key → value pairs)

**XML** – eXtensible Markup Language
```xml
<student>
  <id>101</id>
  <name>Raju</name>
</student>
```

Examples of Document DBs: **MongoDB, Redis, Cassandra**

---

## SQL Command Categories

SQL commands are grouped into 5 languages:

| Language | Full Form | Purpose | Common Commands |
|---|---|---|---|
| **DDL** | Data Definition Language | Defines/modifies structure of DB objects (tables, schema) | CREATE, ALTER, DROP, TRUNCATE, RENAME |
| **DML** | Data Manipulation Language | Manipulates the data inside tables | INSERT, UPDATE, DELETE |
| **DQL** | Data Query Language | Retrieves/queries data | SELECT |
| **DCL** | Data Control Language | Controls access/permissions on the DB | GRANT, REVOKE |
| **TCL** | Transaction Control Language | Manages transactions | COMMIT, ROLLBACK, SAVEPOINT |

**Basic building blocks:**
- Datatype / Operators
- DDL, DML, DCL, DQL, TCL
- Functions, GROUP BY, WHERE, ORDER BY, HAVING, JOIN, Subquery
- Normalization

---

## Login / Connection Info (example used in class)
- username: `hr`
- password: `tiger`

---

## Some Important SQL*Plus Commands

| # | Command | Purpose |
|---|---|---|
| 1 | `show user` | Display current username |
| 2 | `show pagesize` | Display page size |
| 3 | `show linesize` | Display line size |
| 4 | `select * from tab` | Display all table names |
| 5 | `select * from dept` | Display all details/data of table `dept` |
| 6 | `desc dept` | Describe the table `dept` (datatype, column name) |
| 7 | `set pagesize 100 linesize 100` | Set line size & page size |
| 8 | `cl scr` / clear screen | Clear the screen |
| 9 | `exit` | Exit SQL*Plus (shortcut: `Ctrl+Z`) |
| 10 | `conn` | Connect as another user (enter username or username/password) |