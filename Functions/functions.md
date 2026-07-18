# SQL Functions – Notes

## What is a Function?
A **function** is a block of code used to perform a specific task.

**Types:**
1. User Defined Function
2. Pre-defined (Built-in) Function

**Pre-defined functions are of 2 types:**
1. Single Row Function
2. Multi Row Function (Group / Aggregate Function)

---

## 1. Single Row Function
Takes **n input** and gives **n output** (one output per row).

**Types of Single Row Functions:**
- Character Single Row Function
- Number Single Row Function
- Date Single Row Function
- General Single Row Function

---

### A. Character Single Row Function
Used to perform operations on characters/strings.

**Sub-types:**
- Case Manipulation Function
- Character Manipulation Function

#### i) Case Manipulation
| Function | Purpose | Example | Result |
|---|---|---|---|
| `UPPER(arg)` | Converts to uppercase | `SELECT UPPER('raju') FROM dual;` | RAJU |
| `LOWER(arg)` | Converts to lowercase | `SELECT LOWER('RAJU') FROM dual;` | raju |
| `INITCAP(arg)` | Capitalizes first char of each word | `SELECT INITCAP('manoj is kind') FROM dual;` | Manoj Is Kind |

#### ii) Character Manipulation

**1. SUBSTR** – extracts characters from a string.
Accepts 3 args: original string, position, length
```sql
SUBSTR('os', pos, len)
```
- `SELECT SUBSTR('Manu',1,3) FROM dual;` → `Man`
- First char: `SUBSTR(ename,1,1)`
- First N letters: `SUBSTR(ename,1,N)`
- 2nd letter only: `SUBSTR(ename,2,1)`
- Last letter: `SUBSTR(ename,LENGTH(ename),1)`
- Last 2 letters: `SUBSTR(ename,LENGTH(ename)-1,2)`
- 2nd last letter: `SUBSTR(ename,LENGTH(ename)-1,1)`
- First half of name: `SUBSTR(ename,1,FLOOR(LENGTH(ename)/2))`
  - `FLOOR` = largest integer less than or equal to given number
- 2nd half of name: `SUBSTR(ename,CEIL(LENGTH(ename)/2))`
  - `CEIL` = smallest integer greater than or equal to given number

**2. CONCAT** – joins two arguments (character single row function).
```sql
CONCAT('arg1','arg2')
```
- `SELECT CONCAT('Hi:',empname) FROM emp;` → `Hi: <empname>` for all rows

**3. REVERSE** – reverses a given string.
```sql
REVERSE('arg')
```
- `SELECT REVERSE('FORD') FROM emp;` → `DORF`

**4. INSTR** – finds the index/position value of a character.
Accepts 4 args: string, character, start position, nth occurrence
```sql
INSTR('os','char', pos, nth_occ)
```
- `SELECT INSTR('NAYAN','A',1,1) FROM dual;` → 2 (first occurrence)
- `SELECT INSTR('NAYAN','A',1,2) FROM dual;` → 4 (second occurrence)
- To find first occurrence of a space: `INSTR(name,' ',1,1)`
- To find second occurrence of a space: `INSTR(name,' ',1,2)`
- Find number of a particular character (e.g. count of 'P'):
  `SELECT LENGTH('Pushpa') - LENGTH(REPLACE('Pushpa','P','')) FROM dual;`
- Find index of `@` in email: `INSTR(email,'@',1,1)`
- Find domain name from email: `SUBSTR(email, INSTR(email,'@')+1)`

**5. TRIM** – deletes first letter, last letter, or both (same char).
```sql
TRIM(LEADING/TRAILING/BOTH 'char' FROM 'os')
```
- `SELECT TRIM(LEADING 'P' FROM 'PUSHPA') FROM dual;` → `USHPA`
- `SELECT TRIM(TRAILING 'A' FROM 'PUSHPA') FROM dual;` → `PUSHP`
- `SELECT TRIM(BOTH 'P' FROM 'PUSHP') FROM dual;` → `USH`

**6. REPLACE** – replaces a character/substring with a new string. Replaces **all occurrences**.
```sql
REPLACE('os','substr','newstr')
```
- `SELECT REPLACE('BANGLORE','B','M') FROM dual;` → `MANGLORE`
- `SELECT REPLACE('PUSHPA','P','K') FROM dual;` → `KUSHKA`
- To display "USHA" from "PUSHPA": `REPLACE('PUSHPA','P','')`

---

### B. Number Single Row Function
Used to perform operations on numbers.

| Function | Purpose | Example | Result |
|---|---|---|---|
| `MOD(m,n)` | Get remainder | `SELECT MOD(11,2) FROM dual;` | 2 |
| `SQRT(arg)` | Get square root | `SELECT SQRT(100) FROM dual;` | 10 |
| `POWER(m,n)` | Get exponential value | `SELECT POWER(2,5) FROM dual;` | 32 |
| `ABS(arg)` | Returns positive value | `SELECT ABS(-10) FROM dual;` | 10 |
| `ROUND(number,scale)` | Rounds off number to nearest value | see below | — |

**ROUND rules:** decimal 0–4 → rounds down (0), decimal 5–9 → rounds up (1)

ROUND examples:
- `SELECT ROUND(37.7494,1) FROM dual;` → 37.7
- `SELECT ROUND(37.7634,2) FROM dual;` → 37.8
- `SELECT ROUND(37.889,1) FROM dual;` → 38
- `SELECT ROUND(789.12,-2) FROM dual;` → 800
- `SELECT ROUND(739.12,-2) FROM dual;` → 700
- `SELECT ROUND(349.12,-3) FROM dual;` → 0

(negative scale rounds to the left of decimal point — tens/hundreds/thousands)

---

### C. Date Single Row Function
Used to perform operations on dates.

| Function | Purpose | Example | Result |
|---|---|---|---|
| `ADD_MONTHS('date',n)` | Adds n months to a date | `SELECT ADD_MONTHS('07-JUL-2026',12) FROM dual;` | 07-JUL-2027 |
| `MONTHS_BETWEEN('date1','date2')` | Difference between 2 dates in months | `SELECT MONTHS_BETWEEN('07-JUL-2027','07-JUL-2026') FROM dual;` | 12 |
| `EXTRACT(YEAR/MONTH/DAY FROM col_date)` | Extracts year/month/day as a number | `SELECT EXTRACT(MONTH FROM hiredate) FROM emp;` | month number |
| `LAST_DAY('date')` | Returns last day of the month | `SELECT LAST_DAY('12-FEB-2020') FROM dual;` | 29-FEB-2020 |
| `SYSDATE` | Returns current system date | `SELECT SYSDATE FROM dual;` | — |

Other uses:
- Display ename + experience in years:
  `SELECT ename, TRUNC(MONTHS_BETWEEN('07-JUL-2026',hiredate)/12) AS experience FROM emp;`
- Display ename + hired year: `SELECT ename, EXTRACT(YEAR FROM hiredate) FROM emp;`
- Display ename + last day of hire month: `SELECT ename, LAST_DAY(hiredate) AS lastday FROM emp;`

---

### D. General Single Row Function
Used to overcome the problem that occurs while performing operations with `NULL`.

**1. NVL** – Null Value Logic
```sql
NVL(arg1, arg2)
```
Accepts 2 arguments. If arg1 is null → substitutes with arg2. If arg1 is not null → returns arg1.

**2. NVL2**
```sql
NVL2(arg1, arg2, arg3)
```
Accepts 3 arguments. If arg1 is null → substitutes with arg3. If arg1 is not null → substitutes with arg2.

---

## 2. Multi Row Function / Group Function / Aggregate Function
Takes **n input** and gives **one output**.

| Function | Purpose |
|---|---|
| `MAX(arg)` | Returns max value |
| `MIN(arg)` | Returns min value |
| `AVG(arg)` | Returns average value |
| `SUM(arg)` | Returns sum of values |
| `COUNT(*/colName)` | Returns count of rows |

Examples:
- `SELECT MAX(sal) FROM emp;`
- `SELECT COUNT(*) FROM emp;`
- `SELECT MAX(sal) FROM emp WHERE deptno=20;`
- `SELECT MIN(sal) FROM emp WHERE deptno=30;`
- `SELECT SUM(sal) FROM emp WHERE job='SALESMAN';`
- `SELECT AVG(sal) FROM emp WHERE job IN ('SALESMAN','ANALYST','CLERK');`
- `SELECT COUNT(*) FROM emp WHERE job='MANAGER';`

⚠️ We **cannot use** a normal column with an Aggregate/Multi Row Function directly (without GROUP BY).
- Wrong-ish without grouping: `SELECT ename, MAX(sal) ...`
- Needs: `GROUP BY` → `SELECT ename, SUM(sal) FROM emp GROUP BY ename;`

---

## Quick Reference: Practice Query Patterns
- Even salary: `SELECT ename,sal FROM emp WHERE MOD(sal,2)!=1;`
- Odd salary: `SELECT ename,sal FROM emp WHERE MOD(sal,2)=1;`