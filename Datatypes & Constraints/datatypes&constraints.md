# Oracle SQL - Data Types and Constraints
> **Date:** 29-06-2026  
> **Database:** Oracle SQL

---

# Data Types

## Definition

A **Data Type** specifies the type of data that can be stored in a table column.

It determines:
- What kind of data can be stored.
- How much memory is allocated.
- The operations that can be performed on the data.

---

# 1. CHAR

## Definition

`CHAR` is a character data type used to store:

- Alphabets
- Numbers
- Special characters

Example:
- PAN Number
- IFSC Code
- Country Code
- Gender (M/F)

---

## Features

- Fixed-length data type.
- Maximum size: **2000 bytes** (Oracle).
- Extra space is automatically filled with blanks.

---

## Syntax

```sql
CHAR(size)
```

Example

```sql
CHAR(3)
```

Stored Value

```
ABC
```

Memory

```
|A|B|C|
```

---

### Example

```sql
CREATE TABLE student
(
    gender CHAR(1)
);
```

---

## Advantages

- Faster comparison.
- Suitable when the data length is always fixed.

Examples

- Gender
- State Code
- Country Code
- IFSC Prefix

---

## Disadvantages

- Wastes memory if stored value is smaller than allocated size.

Example

```text
CHAR(5)

Store:
A

Memory

|A| | | | |
```

Unused spaces are still reserved.

---

# 2. VARCHAR

## Definition

`VARCHAR` stores variable-length character data.

It can store:

- Alphabets
- Numbers
- Special Characters

---

## Features

- Variable-length memory allocation.
- Maximum size: **2000 bytes**.
- Uses only the required memory.
- No memory wastage.

---

## Syntax

```sql
VARCHAR(size)
```

Example

```sql
VARCHAR(3)
```

Store

```
A
```

Memory

```
|A|
```

Store

```
AB
```

Memory

```
|A|B|
```

---

### Example

```sql
CREATE TABLE student
(
    name VARCHAR(30)
);
```

---

## Advantages

- Saves memory.
- Best when exact length of data is unknown.

Examples

- Employee Name
- City
- Address
- Email

---

# CHAR vs VARCHAR

| CHAR | VARCHAR |
|-------|----------|
| Fixed length | Variable length |
| Memory wastage possible | No memory wastage |
| Faster comparisons | Slightly slower |
| Use when length is fixed | Use when length is unknown |

---

# 3. NUMBER

## Definition

`NUMBER` is used to store numeric values.

---

## Syntax

```sql
NUMBER(precision, scale)
```

- **Precision** → Total number of digits.
- **Scale** → Digits after decimal point.

> Scale is optional.

---

## Examples

```sql
NUMBER(3)
```

Valid

```
374
```

---

```sql
NUMBER(4)
```

Valid

```
5761
```

---

```sql
NUMBER(5,2)
```

Valid

```
548.17
```

---

```sql
NUMBER(4,4)
```

Valid

```
0.7654
```

---

### Example

```sql
salary NUMBER(8,2)
```

---

# 4. DATE

## Definition

Stores date values.

Oracle default format

```
DD-MON-YY
```

Example

```
29-JUN-26
```

Another format

```
DD-MON-YYYY
```

Example

```
29-JUN-2026
```

---

### Example

```sql
hiredate DATE
```

---

# 5. VARCHAR2

## Definition

`VARCHAR2` is Oracle's recommended variable-length character data type.

---

## Features

- Stores up to **4000 bytes**.
- Same concept as VARCHAR.
- Oracle internally converts VARCHAR to VARCHAR2.

---

### Syntax

```sql
VARCHAR2(size)
```

Example

```sql
ename VARCHAR2(50)
```

---

# VARCHAR vs VARCHAR2

| VARCHAR | VARCHAR2 |
|----------|-----------|
| Old SQL standard | Oracle recommended |
| Up to 2000 bytes | Up to 4000 bytes |
| Rarely used | Mostly used in Oracle |

> **Interview Note:** Always use **VARCHAR2** in Oracle instead of VARCHAR.

---

# 6. LOB (Large Object)

LOB is used to store very large data.

---

## CLOB

Character Large Object

Stores

- Large Text
- Articles
- Documents
- XML

---

## BLOB

Binary Large Object

Stores

- Images
- Videos
- Audio
- PDF Files

---

### Example

```sql
resume CLOB

photo BLOB
```

---

# Summary of Data Types

| Data Type | Used For |
|------------|-----------|
| CHAR | Fixed-length characters |
| VARCHAR | Variable-length characters |
| VARCHAR2 | Oracle variable-length characters |
| NUMBER | Numbers |
| DATE | Date values |
| CLOB | Large text |
| BLOB | Images, videos, files |

---

# Constraints

## Definition

**Constraints** are rules applied to table columns to restrict invalid data.

They improve:

- Data Integrity
- Accuracy
- Consistency

---

# Types of Constraints

1. UNIQUE
2. NOT NULL
3. PRIMARY KEY
4. CHECK
5. DEFAULT
6. FOREIGN KEY

---

# 1. UNIQUE

## Definition

Prevents duplicate values.

Duplicate values are **not allowed**.

NULL values are allowed.

---

### Syntax

```sql
column_name datatype UNIQUE
```

---

### Example

```sql
email VARCHAR2(50) UNIQUE
```

Allowed

```
abc@gmail.com
xyz@gmail.com
NULL
```

Not Allowed

```
abc@gmail.com
abc@gmail.com
```

---

# 2. NOT NULL

## Definition

Does not allow NULL values.

Every row must contain a value.

---

### Syntax

```sql
column_name datatype NOT NULL
```

---

### Example

```sql
name VARCHAR2(30) NOT NULL
```

Allowed

```
Rahul
```

Not Allowed

```
NULL
```

---

# 3. PRIMARY KEY

## Definition

Primary Key uniquely identifies every record.

It is a combination of:

- UNIQUE
- NOT NULL

---

## Properties

- Duplicate values are not allowed.
- NULL values are not allowed.
- Every table should have one Primary Key.
- Not mandatory, but highly recommended.

---

### Syntax

```sql
PRIMARY KEY(column_name)
```

or

```sql
id NUMBER PRIMARY KEY
```

---

### Example

```sql
CREATE TABLE student
(
    sid NUMBER PRIMARY KEY,
    name VARCHAR2(30)
);
```

---

# 4. CHECK

## Definition

Provides additional validation.

Allows only values satisfying the given condition.

---

### Syntax

```sql
CHECK(condition)
```

---

### Example

```sql
age NUMBER CHECK(age>=18)
```

Allowed

```
18
25
40
```

Not Allowed

```
15
12
```

---

# 5. DEFAULT

## Definition

Assigns a default value when the user does not provide one.

---

### Syntax

```sql
DEFAULT value
```

---

### Example

```sql
status VARCHAR2(20) DEFAULT 'ACTIVE'
```

Insert

```sql
INSERT INTO student(name)
VALUES('Rahul');
```

Stored

```
Rahul
ACTIVE
```

---

# 6. FOREIGN KEY

## Definition

A Foreign Key establishes a relationship between two tables.

It references the Primary Key of another table.

---

### Syntax

```sql
FOREIGN KEY(column_name)
REFERENCES parent_table(parent_column)
```

---

### Example

```sql
CREATE TABLE department
(
    deptno NUMBER PRIMARY KEY,
    dname VARCHAR2(30)
);

CREATE TABLE employee
(
    empno NUMBER PRIMARY KEY,
    ename VARCHAR2(30),
    deptno NUMBER,
    FOREIGN KEY(deptno)
    REFERENCES department(deptno)
);
```

---

# Example Table

| Column | Data Type | Constraint |
|---------|-----------|------------|
| sid | NUMBER | PRIMARY KEY |
| name | VARCHAR2(15) | NOT NULL |
| dob | DATE | NOT NULL |
| gender | CHAR(1) | NOT NULL |
| phone | NUMBER(10) | UNIQUE |

---

# Constraint Summary

| Constraint | Purpose |
|------------|---------|
| UNIQUE | Prevent duplicate values |
| NOT NULL | Prevent NULL values |
| PRIMARY KEY | Unique + Not Null |
| CHECK | Validate values |
| DEFAULT | Assign default value |
| FOREIGN KEY | Establish relationship between tables |

---

# Interview Questions

### Q1. Difference between CHAR and VARCHAR2?

| CHAR | VARCHAR2 |
|------|-----------|
| Fixed length | Variable length |
| Memory wastage | No wastage |
| Faster comparison | Flexible |
| Max 2000 bytes | Max 4000 bytes |

---

### Q2. Why use VARCHAR2 instead of VARCHAR?

Oracle recommends `VARCHAR2` because it supports larger storage (up to 4000 bytes) and is the standard character type in Oracle.

---

### Q3. Difference between UNIQUE and PRIMARY KEY?

| UNIQUE | PRIMARY KEY |
|---------|-------------|
| Duplicate not allowed | Duplicate not allowed |
| NULL allowed | NULL not allowed |
| Multiple UNIQUE constraints allowed | Only one PRIMARY KEY per table |

---

### Q4. Can a table exist without a Primary Key?

Yes. A Primary Key is **not mandatory**, but it is strongly recommended for uniquely identifying records.

---

### Q5. What is a Foreign Key?

A Foreign Key creates a relationship between a child table and a parent table by referencing the parent's Primary Key.

---

# Key Points to Remember

- `CHAR` → Fixed length.
- `VARCHAR2` → Variable length (recommended in Oracle).
- `NUMBER(p,s)` → Precision and Scale.
- `DATE` stores date values.
- `CLOB` stores text.
- `BLOB` stores binary data.
- `PRIMARY KEY = UNIQUE + NOT NULL`.
- `FOREIGN KEY` creates relationships.
- `CHECK` validates data.
- `DEFAULT` provides automatic values.
- Constraints help maintain **data integrity**.