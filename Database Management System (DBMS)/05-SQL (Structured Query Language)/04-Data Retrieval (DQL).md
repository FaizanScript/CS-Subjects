

### **1. What is DQL?**

**Data Query Language (DQL)** is the category of SQL commands used to retrieve data from database tables.

The primary command associated with DQL is:

```
SELECT
```

For example:

```
SELECT *
FROM Student;
```

This retrieves all columns and rows from the `Student` table.

One important distinction:

- `INSERT`, `UPDATE`, `DELETE` modify records.
    
- `SELECT` retrieves records without modifying the underlying data.

Note: Some textbooks classify `SELECT` under DML instead of treating DQL as a separate category. Both classifications are encountered in DBMS education.


### **2. Understanding the SELECT Statement**

The general syntax is:

```
SELECT column1, column2, ...
FROM table_name;
```

Let's use this sample table throughout the lesson.

**Student**

|student_id|name|age|city|department|
|---|---|---|---|---|
|101|Faizan|20|Hazaribagh|CSE|
|102|Aman|21|Ranchi|IT|
|103|Rahul|19|Patna|CSE|
|104|Priya|22|Delhi|ECE|
|105|Rohit|20|Mumbai|IT|
|106|Neha|19|Ranchi|CSE|


### **3. Retrieving All Columns**

Use the asterisk (`*`) to retrieve every column.

```
SELECT *
FROM Student;
```

Here:

- `SELECT` specifies what to retrieve.
    
- `*` means all columns.
    
- `FROM` specifies the source table.
    
- `Student` is the table name.

Result: all six records with all five columns.

**Practical advice:** Although `SELECT *` is convenient when exploring a database, prefer explicitly naming columns in production queries.

For example:

```
SELECT student_id, name, age
FROM Student;
```

This retrieves only three columns.

Why is this better?

1. Avoids retrieving unnecessary data.
    
2. Reduces network transfer when results are sent to your backend.
    
3. Makes queries easier to understand.
    
4. Prevents unexpected extra columns from being returned when the schema changes.


### **4. Retrieving Specific Columns**

You can retrieve one or multiple columns.

**A. One column**

```
SELECT name
FROM Student;
```

Result:

|name|
|---|
|Faizan|
|Aman|
|Rahul|
|Priya|
|Rohit|
|Neha|

**B. Multiple columns**

```
SELECT name, age
FROM Student;
```

Result:

|name|age|
|---|---|
|Faizan|20|
|Aman|21|
|Rahul|19|
|Priya|22|
|Rohit|20|
|Neha|19|

The order of columns in the SELECT statement determines the order in the query result.

For example:

```
SELECT age, name
FROM Student;
```

The result will show `age` first and `name` second.


### **5. Column Aliases — AS**

An alias gives a temporary alternative name to a column in the query result.

Syntax:

```
SELECT column_name AS alias_name
FROM table_name;
```

Example:

```
SELECT
    name AS student_name,
    age AS student_age
FROM Student;
```

Result:

|student_name|student_age|
|---|---|
|Faizan|20|
|Aman|21|
|Rahul|19|
|Priya|22|
|Rohit|20|
|Neha|19|

Notice that the actual table columns remain `name` and `age`.

Only their displayed names have changed in the result.

You can also omit `AS`:

```
SELECT name student_name
FROM Student;
```

Both forms work in PostgreSQL.

**Important:** A column alias does not permanently rename the column in the database. For that, we use `ALTER TABLE ... RENAME COLUMN`.


### **6. DISTINCT — Removing Duplicate Results**

Suppose we want to find all the cities where students live.

A normal query:

```
SELECT city
FROM Student;
```

Result:

|city|
|---|
|Hazaribagh|
|Ranchi|
|Patna|
|Delhi|
|Mumbai|
|Ranchi|

Notice that Ranchi appears twice.

What if we want each city only once?

Use `DISTINCT`.

```
SELECT DISTINCT city
FROM Student;
```

Result:

|city|
|---|
|Hazaribagh|
|Ranchi|
|Patna|
|Delhi|
|Mumbai|

`DISTINCT` eliminates duplicate combinations from the selected columns.

#### **DISTINCT with multiple columns**

Consider:

```
SELECT DISTINCT city, department
FROM Student;
```

Here, PostgreSQL considers the combination of `city` and `department`.

For example:

|city|department|
|---|---|
|Ranchi|IT|
|Ranchi|CSE|

Both rows remain because the combinations are different.

**Important distinction:**

```
SELECT DISTINCT city
FROM Student;
```

Removes duplicate city values.

```
SELECT DISTINCT city, department
FROM Student;
```

Removes duplicate `(city, department)` combinations, not duplicates in each column independently.


### **7. Expressions and Calculated Columns**

SQL allows you to perform calculations directly inside a SELECT statement.

Suppose we want to display every student's age after five years.

```
SELECT
    name,
    age,
    age + 5 AS future_age
FROM Student;
```

Result:

|name|age|future_age|
|---|---|---|
|Faizan|20|25|
|Aman|21|26|
|Rahul|19|24|
|Priya|22|27|
|Rohit|20|25|
|Neha|19|24|

The `future_age` column is calculated when the query runs.

It is not stored in the original table.

You can use arithmetic operators:

|Operator|Meaning|
|---|---|
|`+`|Addition|
|`-`|Subtraction|
|`*`|Multiplication|
|`/`|Division|
|`%`|Modulo (remainder)|

Example:

```
SELECT
    name,
    age,
    age * 12 AS age_in_months
FROM Student;
```

PostgreSQL also supports expressions combining columns:

```
SELECT
    product_name,
    quantity * price AS total_price
FROM Products;
```

This is especially useful in backend applications.


### **8. SELECT with String Operations**

You can also combine strings using SQL.

In PostgreSQL, the `||` operator concatenates strings.

```
SELECT
    name || ' - ' || city AS student_information
FROM Student;
```

Example output:

|student_information|
|---|
|Faizan - Hazaribagh|
|Aman - Ranchi|
|Rahul - Patna|

You can also use built-in string functions, which we'll cover in Topic 5.12.


### **9. WHERE — A Preview of Filtering**

Although we'll study filtering properly in Topic 5.5, you should understand the basic purpose of `WHERE`.

`WHERE` restricts the rows returned by a query based on a condition.

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

Only students whose age is greater than 20 are returned.

We'll explore comparison operators, logical operators, `BETWEEN`, `IN`, `LIKE`, `IS NULL`, and much more in the next topic.


### **10. Important SQL Concept: Query Result vs Stored Data**

One thing you must understand clearly:

A SELECT query produces a result set. It does not create or modify the original table.

For example:

```
SELECT
    name,
    age + 5 AS future_age
FROM Student;
```

The database calculates `future_age` for the result.

It does not update the actual `age` values.

If Faizan's stored age is 20, it remains 20.

This distinction becomes important when working with calculated values, views and backend APIs.


### **11. Quick Revision**

| Concept       | Purpose                           |
| ------------- | --------------------------------- |
| `SELECT *`    | Retrieve all columns              |
| `SELECT name` | Retrieve specific columns         |
| `AS`          | Give temporary aliases            |
| `DISTINCT`    | Remove duplicate result rows      |
| Expressions   | Calculate values in query results |
| `WHERE`       | Filter rows based on conditions   |
| `FROM`        | Specify the source table          |
