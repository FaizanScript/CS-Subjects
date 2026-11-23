

## **1. What is NULL?**

In a relational database, **NULL represents the absence of a known or applicable value for an attribute.**

It can represent different situations:

1. **Unknown value:** The student's phone number is not known.
    
2. **Missing value:** The student has not provided their address yet.
    
3. **Not applicable:** A student does not have a spouse's name because they are unmarried.

Example:

|student_id|name|phone_number|spouse_name|
|---|---|---|---|
|101|Rahul|9876543210|NULL|
|102|Aman|NULL|NULL|
|103|Priya|9123456780|NULL|

Here:

- Rahul's phone number is available.
    
- Aman's phone number is unknown or missing.
    
- `spouse_name` may be NULL because it is not applicable.

**Important:** NULL does not tell us _why_ a value is absent. The database simply stores the NULL marker.

### **NULL is different from other values**

|Value|Meaning|
|---|---|
|`NULL`|Missing, unknown or not applicable|
|`0`|Numeric zero|
|`''`|Empty string|
|`' '`|String containing a space|
|`FALSE`|Boolean false|

These values are not interchangeable.

For example, if a student's marks are `0`, that means the student scored zero marks. If marks are `NULL`, the marks may not have been entered or may be unknown.


## **2. NULL is not an ordinary value**

This is one of the most important concepts in SQL.

Suppose we have:

```
SELECT *
FROM Student
WHERE phone_number = NULL;
```

You might expect this query to return students whose phone numbers are NULL.

But it does not work that way.

Why?

Because **NULL is not an ordinary value that can be compared using normal equality operators.**

Consider:

```
NULL = NULL
```

The result is not TRUE.

It is **UNKNOWN**.

Similarly:

```
NULL = 10       -- UNKNOWN
NULL <> 10      -- UNKNOWN
NULL > 10       -- UNKNOWN
NULL = 'Hello'  -- UNKNOWN
```

The database cannot determine the result of these comparisons because NULL represents an absent or unknown value.

To check for NULL, SQL provides special operators:

```
IS NULL
IS NOT NULL
```

Correct examples:

```
-- Find students whose phone number is missing
SELECT *
FROM Student
WHERE phone_number IS NULL;

-- Find students whose phone number is available
SELECT *
FROM Student
WHERE phone_number IS NOT NULL;
```

Remember

Never use `= NULL` or `<> NULL` to check for NULL. Use `IS NULL` and `IS NOT NULL`.


## **3. Three-Valued Logic in SQL**

In ordinary Boolean logic, we generally work with two possible results:

- TRUE
    
- FALSE

However, SQL uses **Three-Valued Logic (3VL)** because NULL introduces a third possible result:

1. TRUE
    
2. FALSE
    
3. UNKNOWN

Let's understand each.

|Result|Meaning|
|---|---|
|TRUE|The condition is satisfied|
|FALSE|The condition is not satisfied|
|UNKNOWN|The result cannot be determined|

Example:

Suppose a table contains:

|student_id|marks|
|---|---|
|101|90|
|102|40|
|103|NULL|

Now execute:

```
SELECT *
FROM Student
WHERE marks > 50;
```

Evaluation:

|student_id|Evaluation|Result|
|---|---|---|
|101|90 > 50|TRUE|
|102|40 > 50|FALSE|
|103|NULL > 50|UNKNOWN|

**Which rows will SQL return?**

Only rows where the WHERE condition evaluates to TRUE.

Therefore:

- Student 101 is returned.
    
- Student 102 is excluded.
    
- Student 103 is also excluded.

This is important: SQL's WHERE clause filters out both FALSE and UNKNOWN.


## **4. Truth Tables for NULL**

To understand SQL's three-valued logic properly, we need to understand how `AND`, `OR`, and `NOT` behave.

### **A. AND operator**

Both conditions must be TRUE for the result to be TRUE.

|A|B|A AND B|
|---|---|---|
|TRUE|TRUE|TRUE|
|TRUE|FALSE|FALSE|
|TRUE|UNKNOWN|UNKNOWN|
|FALSE|TRUE|FALSE|
|FALSE|FALSE|FALSE|
|FALSE|UNKNOWN|FALSE|
|UNKNOWN|TRUE|UNKNOWN|
|UNKNOWN|FALSE|FALSE|
|UNKNOWN|UNKNOWN|UNKNOWN|

Notice something interesting:

`FALSE AND UNKNOWN` gives FALSE.

Why? Because even if the unknown condition were TRUE, the overall result would still be FALSE.

### **B. OR operator**

At least one condition must be TRUE for the result to be TRUE.

|A|B|A OR B|
|---|---|---|
|TRUE|TRUE|TRUE|
|TRUE|FALSE|TRUE|
|TRUE|UNKNOWN|TRUE|
|FALSE|TRUE|TRUE|
|FALSE|FALSE|FALSE|
|FALSE|UNKNOWN|UNKNOWN|
|UNKNOWN|TRUE|TRUE|
|UNKNOWN|FALSE|UNKNOWN|
|UNKNOWN|UNKNOWN|UNKNOWN|

Notice:

`TRUE OR UNKNOWN` gives TRUE.

Why? Because one condition is already TRUE, so the overall result is definitely TRUE.

### **C. NOT operator**

NOT reverses TRUE and FALSE, but UNKNOWN remains UNKNOWN.

|A|NOT A|
|---|---|
|TRUE|FALSE|
|FALSE|TRUE|
|UNKNOWN|UNKNOWN|

For example:

```
NOT (NULL = 10)
```

Result: UNKNOWN.

It does not become TRUE.


## **5. NULL Behaviour in WHERE Clause**

Let's understand how this affects real SQL queries.

Consider this table:

**Student**

|student_id|name|marks|
|---|---|---|
|101|Rahul|90|
|102|Aman|40|
|103|Priya|NULL|
|104|Rohan|70|

### **Query 1: Students scoring more than 50**

```
SELECT *
FROM Student
WHERE marks > 50;
```

Result: Rahul and Rohan.

Priya is excluded because `NULL > 50` evaluates to UNKNOWN.

### **Query 2: Students not scoring more than 50**

```
SELECT *
FROM Student
WHERE marks <= 50;
```

Result: Aman.

Priya is still excluded.

You might think that students who did not score more than 50 should include Priya. But SQL cannot determine whether Priya's marks are above or below 50.

### **Query 3: Students whose marks are either above 50 or NULL**

```
SELECT *
FROM Student
WHERE marks > 50 OR marks IS NULL;
```

Result: Rahul, Priya and Rohan.

This is how you explicitly include NULL values in a condition.


## **6. NULL Behaviour in Aggregate Functions**

Aggregate functions perform calculations on multiple rows.

Common aggregate functions include:

- `COUNT()`
    
- `SUM()`
    
- `AVG()`
    
- `MIN()`
    
- `MAX()`

**Most SQL aggregate functions ignore NULL values.**

Let's understand this with an example.

|student_id|marks|
|---|---|
|101|80|
|102|90|
|103|NULL|
|104|70|
|105|NULL|

### **A. COUNT(*)**

```
SELECT COUNT(*)
FROM Student;
```

Result: `5`

`COUNT(*)` counts all rows, including rows containing NULL values.

### **B. COUNT(column)**

```
SELECT COUNT(marks)
FROM Student;
```

Result: `3`

`COUNT(marks)` counts only non-NULL values in the specified column.

### **C. SUM()**

```
SELECT SUM(marks)
FROM Student;
```

Result: `240`

Calculation:

\[ 80 + 90 + 70 = 240 \]

NULL values are ignored.

### **D. AVG()**

```
SELECT AVG(marks)
FROM Student;
```

Result:

	[ 80+90+70 / 3=80 ]

NULL values are excluded from both the sum and the count used for the average.

### **E. MIN() and MAX()**

```
SELECT MIN(marks), MAX(marks)
FROM Student;
```

Result:

|MIN|MAX|
|---|---|
|70|90|

NULL values are ignored.

### **Quick comparison**

|Function|NULL behaviour|
|---|---|
|`COUNT(*)`|Counts all rows|
|`COUNT(column)`|Ignores NULL|
|`SUM(column)`|Ignores NULL|
|`AVG(column)`|Ignores NULL|
|`MIN(column)`|Ignores NULL|
|`MAX(column)`|Ignores NULL|

**Important exception:** If an aggregate receives no non-NULL input values, `COUNT(column)` returns 0, while `SUM`, `AVG`, `MIN` and `MAX` generally return NULL.


## **7. Handling NULL using COALESCE()**

Sometimes we want to replace NULL with a meaningful alternative while retrieving data.

SQL provides the `COALESCE()` function.

**Definition:** `COALESCE()` returns the first non-NULL value from the supplied list of expressions.

Syntax:

```
COALESCE(value1, value2, value3, ...)
```

Example:

|student_id|name|marks|
|---|---|---|
|101|Rahul|80|
|102|Aman|NULL|
|103|Priya|90|

Query:

```
SELECT
    name,
    COALESCE(marks, 0) AS marks
FROM Student;
```

Result:

|name|marks|
|---|---|
|Rahul|80|
|Aman|0|
|Priya|90|

Here, `COALESCE(marks, 0)` means:

- If marks is not NULL, return marks.
    
- If marks is NULL, return 0.

**Important:** This query does not update the original database value. It only changes what is displayed in the query result.

To update actual records, you would need an `UPDATE` statement.


## **8. NULLIF() and IS DISTINCT FROM**

Two additional SQL features are useful to know.

### **A. NULLIF()**

`NULLIF(value1, value2)` returns NULL if both values are equal; otherwise, it returns the first value.

Example:

```
SELECT NULLIF(10, 10);
```

Result: NULL.

```
SELECT NULLIF(10, 20);
```

Result: 10.

A practical use is avoiding division by zero:

```
SELECT 100 / NULLIF(total_students, 0);
```

If `total_students` is zero, `NULLIF()` returns NULL instead of zero, avoiding a division-by-zero error. The division result becomes NULL.

### **B. IS DISTINCT FROM**

In PostgreSQL, `IS DISTINCT FROM` compares two values while treating NULL as a comparable condition.

```
SELECT NULL IS DISTINCT FROM NULL;
```

Result: FALSE.

```
SELECT NULL IS DISTINCT FROM 10;
```

Result: TRUE.

Unlike `=`, this operator always produces TRUE or FALSE, not UNKNOWN.

Its opposite is:

```
IS NOT DISTINCT FROM
```

This is useful when you need NULL-safe comparisons.


## **9. Important Points to Remember**

|Concept|Key takeaway|
|---|---|
|NULL|Represents absent, unknown or inapplicable information|
|NULL vs 0|NULL is not zero|
|NULL vs empty string|NULL is not an empty string|
|`NULL = NULL`|UNKNOWN|
|`IS NULL`|Checks whether a value is NULL|
|`IS NOT NULL`|Checks whether a value is not NULL|
|Three-valued logic|TRUE, FALSE, UNKNOWN|
|WHERE clause|Returns only rows where condition is TRUE|
|`COUNT(*)`|Counts all rows|
|`COUNT(column)`|Counts non-NULL values|
|`COALESCE()`|Returns the first non-NULL expression|
|`NULLIF()`|Returns NULL when two expressions are equal|
|`IS DISTINCT FROM`|NULL-safe comparison in PostgreSQL|
