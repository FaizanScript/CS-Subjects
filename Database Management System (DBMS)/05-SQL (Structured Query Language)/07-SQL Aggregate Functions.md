
Until now, most of our SQL queries have retrieved individual records.

But what if we want answers like:

- How many students are registered in the college?
    
- What is the average age of students?
    
- What is the highest salary in a company?
    
- What is the total revenue generated from orders?
    
- What is the lowest product price?

Retrieving every row and calculating these values manually would be inefficient.

This is where **Aggregate Functions** come in.



### **1. What are Aggregate Functions?**

**Aggregate functions are SQL functions that perform calculations on a set of rows and return a single summarized value.**

For example:

```
SELECT COUNT(*)
FROM Student;
```

This returns the total number of rows in the Student table.

The five commonly used aggregate functions are:

|Function|Purpose|
|---|---|
|`COUNT()`|Counts rows or non-NULL values|
|`SUM()`|Calculates the total|
|`AVG()`|Calculates the average|
|`MIN()`|Finds the minimum value|
|`MAX()`|Finds the maximum value|

Let's understand each one.

We'll use these sample tables throughout the lesson.

**Student**

|student_id|name|age|department|
|---|---|---|---|
|101|Faizan|20|CSE|
|102|Aman|21|IT|
|103|Rahul|19|CSE|
|104|Priya|22|ECE|
|105|Rohit|20|IT|
|106|Neha|19|CSE|

**Orders**

|order_id|product|quantity|price|
|---|---|---|---|
|1|Laptop|2|50000|
|2|Mouse|3|500|
|3|Keyboard|2|1500|
|4|Monitor|1|12000|
|5|Headphones|4|2000|


### **2. COUNT() — Counting Records**

`COUNT()` is used to count rows or non-NULL values.

There are three useful forms.

**A. COUNT(*********)**

Counts all rows, including rows containing NULL values.

```
SELECT COUNT(*)
FROM Student;
```

Result: 6

There are six records in the Student table.

**B. COUNT(column****_****name)**

Counts only the non-NULL values in a particular column.

```
SELECT COUNT(age)
FROM Student;
```

If all six students have an age, the result is 6.

But suppose one student's age is NULL:

|student_id|name|age|
|---|---|---|
|101|Faizan|20|
|102|Aman|21|
|103|Rahul|NULL|
|104|Priya|22|

Then:

```
SELECT COUNT(*)
FROM Student;
```

Result: 4

```
SELECT COUNT(age)
FROM Student;
```

Result: 3

Why?

- `COUNT(*)` counts all rows.
    
- `COUNT(age)` ignores rows where age is NULL.

**C. COUNT(DISTINCT column****_****name)**

Counts unique, non-NULL values.

```
SELECT COUNT(DISTINCT department)
FROM Student;
```

Result: 3

Because the departments are CSE, IT and ECE.

Notice that CSE appears three times, but is counted once.

**Remember:**

|Query|What it counts|
|---|---|
|`COUNT(*)`|All rows|
|`COUNT(age)`|Non-NULL age values|
|`COUNT(DISTINCT age)`|Unique, non-NULL age values|


### **3. SUM() — Calculating Total**

`SUM()` calculates the total of numeric values in a column.

Example:

```
SELECT SUM(quantity)
FROM Orders;
```

Calculation:

	 2+3+2+1+4=12 

Result: 12

A total of 12 product units were ordered.

**Another example: Total order quantity value**

Our table has separate `quantity` and `price` columns.

We can calculate the total value of all orders using:

```
SELECT SUM(quantity * price)
FROM Orders;
```

Calculation:

	(2 x 50000) + (3 x 500) + (2 x 1500)

	+(1 x 12000) + (4 x 2000)

	= 123500

Result: ₹1,23,500

Notice that we used an expression inside the aggregate function.

**Important NULL behaviour:**

`SUM()` ignores NULL values.

For example, if the values are:

| amount |
| ------ |
| 100    |
| 200    |
| NULL   |
| 300    |

Then:

```
SELECT SUM(amount)
FROM Payments;
```

Result: 600.

If there are no non-NULL input values, `SUM()` returns NULL.


### **4. AVG() — Calculating Average**

`AVG()` calculates the arithmetic mean of numeric values.

Formula:

	Average = sum of values / number of non-null values

Example:

```
SELECT AVG(age)
FROM Student;
```

Calculation:

	20 + 21 + 19 + 22 + 20 + 19 / 6
	= 121 / 6
	= 20.17


Result: 20.1667...

PostgreSQL returns a numeric result for integer inputs, so you may see more decimal places than expected.

**A. AVG ignores NULL**

Suppose age values are:

|age|
|---|
|20|
|22|
|NULL|
|18|

Then:

```
SELECT AVG(age)
FROM Student;
```

Calculation:

	20 + 22 + 18 / 3 = 20

NULL is excluded from both the sum and the count.

**B. Rounding the average**

PostgreSQL provides the `ROUND()` function.

```
SELECT ROUND(AVG(age), 2)
FROM Student;
```

This rounds the result to two decimal places.

We'll explore SQL functions in more detail in Topic 5.12.


### **5. MIN() — Finding Minimum Value**

`MIN()` returns the smallest non-NULL value.

Example:

```
SELECT MIN(age)
FROM Student;
```

Result: 19

This means the youngest student in our sample is 19 years old.

You can also use it with dates, strings and other comparable data types.

Example:

```
SELECT MIN(price)
FROM Orders;
```

Result: 500.

This gives the lowest product price.


### **6. MAX() — Finding Maximum Value**

`MAX()` returns the largest non-NULL value.

Example:

```
SELECT MAX(age)
FROM Student;
```

Result: 22

The oldest student is 22 years old.

Another example:

```
SELECT MAX(price)
FROM Orders;
```

Result: 50000.

The most expensive product in the sample costs ₹50,000.


### **7. Using Multiple Aggregate Functions Together**

You don't need separate queries for every calculation.

You can use multiple aggregate functions in a single SELECT statement.

```
SELECT
    COUNT(*) AS total_students,
    AVG(age) AS average_age,
    MIN(age) AS youngest_student,
    MAX(age) AS oldest_student
FROM Student;
```

Result:

|total_students|average_age|youngest_student|oldest_student|
|---|---|---|---|
|6|20.1667...|19|22|

This query returns one row containing four calculated values.

**Backend connection:** This is useful for dashboard APIs.

Imagine an admin dashboard displaying:

- Total registered students.
    
- Average student age.
    
- Youngest student.
    
- Oldest student.

You can retrieve all four statistics using one query.


### **8. Aggregate Functions with WHERE**

Aggregate functions can work with filtered records.

For example, suppose we want to count only CSE students.

```
SELECT COUNT(*)
FROM Student
WHERE department = 'CSE';
```

Result: 3.

Another example: Find the average age of IT students.

```
SELECT AVG(age)
FROM Student
WHERE department = 'IT';
```

Calculation:

	20 + 21 / 2 = 20.5

Result: 20.5.

You can combine filtering and aggregation:

```
SELECT
    COUNT(*) AS total_students,
    AVG(age) AS average_age
FROM Student
WHERE age >= 20;
```

This calculates statistics only for students aged 20 or above.


### **9. Aggregate Functions and NULL — Important Rules**

This is one of the most important concepts in aggregate functions.

Most aggregate functions ignore NULL values.

Consider this table:

|employee_id|salary|
|---|---|
|101|30000|
|102|40000|
|103|NULL|
|104|50000|

Let's examine the results.

|Query|Result|
|---|---|
|`COUNT(*)`|4|
|`COUNT(salary)`|3|
|`SUM(salary)`|120000|
|`AVG(salary)`|40000|
|`MIN(salary)`|30000|
|`MAX(salary)`|50000|

**What if every value is NULL or there are no rows?**

|Function|Result|
|---|---|
|`COUNT(*)`|0 for no rows|
|`COUNT(column)`|0|
|`SUM(column)`|NULL|
|`AVG(column)`|NULL|
|`MIN(column)`|NULL|
|`MAX(column)`|NULL|

Why does COUNT return zero while SUM returns NULL?

Because COUNT measures the number of qualifying rows or values. SUM calculates a total, and with no non-NULL values, that total is undefined in SQL's aggregate behaviour.

If you want a zero instead of NULL, you can use `COALESCE()`:

```
SELECT COALESCE(SUM(salary), 0)
FROM Employee;
```

If SUM returns NULL, COALESCE gives you 0.


### **10. Important Rule: Aggregate Functions and SELECT**

Consider this query:

```
SELECT
    name,
    AVG(age)
FROM Student;
```

In PostgreSQL, this produces an error.

Why?

Because `AVG(age)` produces one summarized value, whereas `name` represents individual rows.

The database cannot determine which individual name should appear beside the overall average.

We'll solve this problem in the next topic using `GROUP BY`.

For now, remember:

- Aggregate functions summarize multiple rows.
    
- Normal columns represent individual row values.
    
- When you want aggregate results for separate categories, you generally use `GROUP BY`.


### **11. Quick Revision**

|Function|What it does|Example|
|---|---|---|
|`COUNT(*)`|Counts all rows|Total students|
|`COUNT(column)`|Counts non-NULL values|Students with an age recorded|
|`COUNT(DISTINCT column)`|Counts unique non-NULL values|Unique departments|
|`SUM()`|Calculates total|Total revenue|
|`AVG()`|Calculates average|Average salary|
|`MIN()`|Finds minimum|Lowest price|
|`MAX()`|Finds maximum|Highest salary|