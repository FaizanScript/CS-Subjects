
In Topic 5.4, we learned how to retrieve data using `SELECT`.

But real-world applications rarely need every record from a database. They need specific records based on conditions.

For example:

- Find students older than 20.
    
- Find students from Ranchi.
    
- Find students whose names start with A.
    
- Display students from youngest to oldest.
    
- Retrieve only the first 10 records.

We'll learn how to do all of this.

Our examples will continue using the following `Student` table:

| student_id | name   | age | city       | department |
| ---------- | ------ | --- | ---------- | ---------- |
| 101        | Faizan | 20  | Hazaribagh | CSE        |
| 102        | Aman   | 21  | Ranchi     | IT         |
| 103        | Rahul  | 19  | Patna      | CSE        |
| 104        | Priya  | 22  | Delhi      | ECE        |
| 105        | Rohit  | 20  | Mumbai     | IT         |
| 106        | Neha   | 19  | Ranchi     | CSE        |
| 107        | Arjun  | 23  | Delhi      | ECE        |
| 108        | Meera  | 18  | Patna      | CSE        |



### **1. WHERE Clause**

The `WHERE` clause filters records based on a specified condition.

**Syntax:**

```
SELECT column1, column2
FROM table_name
WHERE condition;
```

Example:

```
SELECT *
FROM Student
WHERE age > 20;
```

Result:

|student_id|name|age|city|department|
|---|---|---|---|---|
|102|Aman|21|Ranchi|IT|
|104|Priya|22|Delhi|ECE|
|107|Arjun|23|Delhi|ECE|

Only students older than 20 are returned.

The original table remains unchanged.


### **2. Comparison Operators**

Comparison operators compare two values and produce a Boolean result: TRUE or FALSE (or UNKNOWN when NULL is involved).

|Operator|Meaning|Example|
|---|---|---|
|`=`|Equal to|`age = 20`|
|`<>`|Not equal to|`age <> 20`|
|`!=`|Not equal to|`age != 20`|
|`>`|Greater than|`age > 20`|
|`<`|Less than|`age < 20`|
|`>=`|Greater than or equal to|`age >= 20`|
|`<=`|Less than or equal to|`age <= 20`|

`<>` is the standard SQL not-equal operator. PostgreSQL also supports `!=`.

**Examples**

Find students exactly 20 years old:

```
SELECT *
FROM Student
WHERE age = 20;
```

Find students who are not 20:

```
SELECT *
FROM Student
WHERE age <> 20;
```

Find students younger than 20:

```
SELECT *
FROM Student
WHERE age < 20;
```

Find students aged 20 or older:

```
SELECT *
FROM Student
WHERE age >= 20;
```


### **3. Logical Operators — AND, OR, NOT**

Logical operators combine or reverse conditions.

**A. AND**

`AND` returns TRUE only when both conditions are TRUE.

Example: Find students older than 19 who belong to CSE.

```
SELECT *
FROM Student
WHERE age > 19 AND department = 'CSE';
```

Both conditions must be satisfied.

**B. OR**

`OR` returns TRUE when at least one condition is TRUE.

Example: Find students from Ranchi or Delhi.

```
SELECT *
FROM Student
WHERE city = 'Ranchi' OR city = 'Delhi';
```

A student qualifies if either condition is true.

**C. NOT**

`NOT` reverses a Boolean condition.

Example: Find students who are not from Delhi.

```
SELECT *
FROM Student
WHERE NOT city = 'Delhi';
```

Equivalent:

```
SELECT *
FROM Student
WHERE city <> 'Delhi';
```

**D. Combining multiple logical operators**

```
SELECT *
FROM Student
WHERE age >= 20
AND (city = 'Ranchi' OR city = 'Delhi');
```

The parentheses matter.

They group the OR conditions together, meaning:

- Age must be at least 20.
    
- City must be either Ranchi or Delhi.

Without parentheses, SQL operator precedence can change the result. In general, `NOT` is evaluated before `AND`, and `AND` before `OR`.


### **4. BETWEEN Operator**

`BETWEEN` checks whether a value falls within a specified range.

**Syntax:**

```
SELECT *
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

Example:

```
SELECT *
FROM Student
WHERE age BETWEEN 19 AND 21;
```

This retrieves students aged 19, 20 or 21.

Important

`BETWEEN` is inclusive. Both boundary values are included.

The above query is equivalent to:

```
SELECT *
FROM Student
WHERE age >= 19 AND age <= 21;
```

You can also use `NOT BETWEEN`:

```
SELECT *
FROM Student
WHERE age NOT BETWEEN 19 AND 21;
```

This retrieves students younger than 19 or older than 21.


### **5. IN Operator**

The `IN` operator checks whether a value matches any value in a specified list.

Instead of writing multiple OR conditions:

```
SELECT *
FROM Student
WHERE city = 'Ranchi'
OR city = 'Delhi'
OR city = 'Patna';
```

You can write:

```
SELECT *
FROM Student
WHERE city IN ('Ranchi', 'Delhi', 'Patna');
```

Much cleaner.

**NOT IN**

Find students who are not from Ranchi or Delhi:

```
SELECT *
FROM Student
WHERE city NOT IN ('Ranchi', 'Delhi');
```

This is particularly useful when filtering records against several possible values.

One caution: `NOT IN` can produce unexpected results if its list or subquery contains NULL. We'll revisit this when we study subqueries.


### **6. LIKE Operator — Pattern Matching**

The `LIKE` operator searches for a specified pattern in text values.

It uses two important wildcard characters:

|Wildcard|Meaning|
|---|---|
|`%`|Zero or more characters|
|`_`|Exactly one character|

Let's understand with examples.

**A. Names starting with A**

```
SELECT *
FROM Student
WHERE name LIKE 'A%';
```

Matches:

- Aman
    
- Arjun

`A%` means the name must start with A, followed by zero or more characters.

**B. Names ending with n**

```
SELECT *
FROM Student
WHERE name LIKE '%n';
```

Matches:

- Aman
    
- Arjun

**C. Names containing the letter a**

```
SELECT *
FROM Student
WHERE name LIKE '%a%';
```

Matches any name containing `a` anywhere.

**D. Names with exactly five characters**

```
SELECT *
FROM Student
WHERE name LIKE '_____';
```

Five underscores mean exactly five characters.

**E. Names where the second character is a**

```
SELECT *
FROM Student
WHERE name LIKE '_a%';
```

The first character can be anything, the second must be `a`, and the rest can contain any number of characters.

#### **LIKE vs ILIKE in PostgreSQL**

PostgreSQL provides `ILIKE` for case-insensitive pattern matching.

```
SELECT *
FROM Student
WHERE name ILIKE 'a%';
```

This can match names beginning with either `a` or `A`.

|Operator|Matching|
|---|---|
|`LIKE`|Case-sensitive in PostgreSQL|
|`ILIKE`|Case-insensitive in PostgreSQL|

`ILIKE` is PostgreSQL-specific, not standard SQL.


### **7. IS NULL and IS NOT NULL**

Remember our earlier lesson on NULL values?

You cannot check NULL using the normal equality operator.

==🔴Incorrect==

```
SELECT *
FROM Student
WHERE city = NULL;
```

This does not correctly identify NULL values because comparisons with NULL produce UNKNOWN.

**Correct approach:**

```
SELECT *
FROM Student
WHERE city IS NULL;
```

To retrieve records where city is available:

```
SELECT *
FROM Student
WHERE city IS NOT NULL;
```

Remember:

- `IS NULL` checks for missing/NULL values.
    
- `IS NOT NULL` checks for non-NULL values.
    
- `= NULL` is not the correct way to test NULL.


### **8. ORDER BY — Sorting Records**

So far, our queries have filtered records. But what if we want the results arranged in a particular order?

That's where `ORDER BY` comes in.

**Syntax:**

```
SELECT column1, column2
FROM table_name
ORDER BY column_name;
```

SQL supports two main sorting directions:

- `ASC` — Ascending order.
    
- `DESC` — Descending order.

**A. Ascending order**

```
SELECT *
FROM Student
ORDER BY age ASC;
```

Students are arranged from youngest to oldest.

`ASC` is the default, so this is equivalent:

```
SELECT *
FROM Student
ORDER BY age;
```

**B. Descending order**

```
SELECT *
FROM Student
ORDER BY age DESC;
```

Students are arranged from oldest to youngest.

**C. Sorting text**

```
SELECT *
FROM Student
ORDER BY name ASC;
```

Names are arranged alphabetically according to the database's collation rules.

**D. Sorting using multiple columns**

Suppose we want to sort students by department first and age second.

```
SELECT *
FROM Student
ORDER BY department ASC, age DESC;
```

How does this work?

1. First, students are grouped in sorted order by department.
    
2. Within each department, students are sorted by age from highest to lowest.

The second sorting column is used when values in the first sorting column are equal.

**E. NULL sorting in PostgreSQL**

PostgreSQL has specific NULL ordering rules:

- `ASC` places NULL values last by default.
    
- `DESC` places NULL values first by default.

You can control this explicitly:

```
SELECT *
FROM Student
ORDER BY age ASC NULLS LAST;
```

Or:

```
SELECT *
FROM Student
ORDER BY age DESC NULLS FIRST;
```


### **9. LIMIT — Restricting the Number of Results**

Suppose your database contains 10,000 students, but your application only needs the first 10 records.

Use `LIMIT`.

**Syntax:**

```
SELECT *
FROM Student
LIMIT number;
```

Example:

```
SELECT *
FROM Student
LIMIT 3;
```

This returns at most three rows.

**Combining LIMIT with ORDER BY**

Suppose we want the three oldest students.

```
SELECT *
FROM Student
ORDER BY age DESC
LIMIT 3;
```

The order is important conceptually:

1. Sort the records by age.
    
2. Return the first three.

Without `ORDER BY`, the database does not guarantee which three rows you'll receive.


### **10. OFFSET — Skipping Results**

`OFFSET` specifies how many rows should be skipped before returning results.

**Syntax:**

```
SELECT *
FROM Student
OFFSET number;
```

Example:

```
SELECT *
FROM Student
ORDER BY student_id
OFFSET 3;
```

This skips the first three rows and returns the remaining rows.

**Combining LIMIT and OFFSET**

```
SELECT *
FROM Student
ORDER BY student_id
LIMIT 3 OFFSET 3;
```

This skips three rows and returns the next three.

Let's understand using pagination.

Suppose your website displays three students per page.

|Page|LIMIT|OFFSET|
|---|---|---|
|Page 1|3|0|
|Page 2|3|3|
|Page 3|3|6|
|Page 4|3|9|

General formula:

	OFFSET = (Page Number - 1) x page Size

For example, page 4 with 10 records per page:

	(4 - 1) x 10 = 30

SQL:

```
SELECT *
FROM Student
ORDER BY student_id
LIMIT 10 OFFSET 30;
```

**Backend connection:** This is the foundation of offset-based pagination in web applications.

One limitation: large OFFSET values can become inefficient because the database may still need to scan and discard many rows. For large datasets, cursor/keyset pagination is often more efficient. We'll discuss it when we reach advanced SQL concepts.


### **11. Combining Everything**

Now let's write a more realistic query.

**Requirement:** Find the three oldest CSE students from Ranchi or Delhi, and display their names, ages and cities.

```
SELECT name, age, city
FROM Student
WHERE department = 'CSE'
AND city IN ('Ranchi', 'Delhi')
ORDER BY age DESC
LIMIT 3;
```

Let's break it down:

|SQL clause|Purpose|
|---|---|
|`SELECT`|Choose columns|
|`FROM`|Choose table|
|`WHERE`|Filter records|
|`AND`|Combine conditions|
|`IN`|Match multiple possible cities|
|`ORDER BY`|Sort records|
|`DESC`|Highest age first|
|`LIMIT`|Return at most three rows|


### **12. SQL Query Writing Order vs Logical Execution Order**

This is an important SQL concept.

You write queries in this order:

```
SELECT
FROM
WHERE
ORDER BY
LIMIT
OFFSET;
```

But conceptually, the database processes a query in a different logical order:

## **Logical query processing order**

**1**
**FROM**
Identify the source table(s).

**2**
**WHERE**
Filter rows.

**3**
**SELECT**
Determine the output columns and expressions.

**4**
**ORDER BY**
Sort the resulting rows.

**5**
**LIMIT / OFFSET**
Restrict or skip output rows.

This is a simplified logical model, not a description of the database's physical execution plan. The query optimizer can choose a different physical strategy.

**Why should you care?**

Because SQL is declarative. You describe the result you want, and the database determines how to retrieve it.

### **13. Quick Revision**

|Concept|What it does|
|---|---|
|`WHERE`|Filters records|
|`AND`|Both conditions must be true|
|`OR`|At least one condition must be true|
|`NOT`|Reverses a condition|
|`BETWEEN`|Checks an inclusive range|
|`IN`|Matches values from a list|
|`LIKE`|Pattern matching|
|`ILIKE`|Case-insensitive matching in PostgreSQL|
|`IS NULL`|Checks NULL values|
|`ORDER BY`|Sorts records|
|`ASC`|Ascending order|
|`DESC`|Descending order|
|`LIMIT`|Restricts returned rows|
|`OFFSET`|Skips rows|