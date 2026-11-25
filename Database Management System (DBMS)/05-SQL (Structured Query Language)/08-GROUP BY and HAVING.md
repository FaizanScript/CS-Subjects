
In the previous topic, we learned how aggregate functions calculate a single result from multiple rows.

But here's a problem:

Suppose a college database has 100 students across 5 departments. You want to know:

- How many students are in each department?
    
- What is the average age in each department?
    
- Which departments have more than 10 students?

Using aggregate functions alone, we can calculate the total for the entire table. But we need **GROUP BY** to calculate results separately for each department.

Let's understand how.


## **1. What is GROUP BY?**

**GROUP BY** is an SQL clause used to group rows that have the same values in one or more columns. Aggregate functions can then perform calculations separately on each group.

Think of it this way:

**Student table — before grouping**

|Student|Department|
|---|---|
|Faizan|CSE|
|Aman|IT|
|Rahul|CSE|
|Priya|ECE|
|Rohit|IT|
|Neha|CSE|

**After GROUP BY department**

==🔵**CSE**==                              ==🟢**IT**==                       ==🟠**ECE**==
# ==🔵**3**==                     ==🟢**2**==                ==🟠**1**==
==🔵Students==                 ==🟢Students==                ==🟠Student==

Rows with the same department value are placed into the same group.

Notice that we have divided six individual student records into three groups.


## **2. Basic GROUP BY syntax**

```
SELECT column_name, AGGREGATE_FUNCTION(column_name)
FROM table_name
GROUP BY column_name;
```

Let's write an actual query.

**Example: Count students in each department.**

```
SELECT department, COUNT(*) AS total_students
FROM Student
GROUP BY department;
```

Output:

|department|total_students|
|---|---|
|CSE|3|
|IT|2|
|ECE|1|

What's happening?

1. `FROM Student` — accesses the Student table.
    
2. `GROUP BY department` — creates separate groups for each department.
    
3. `COUNT(*)` — counts the number of rows in each group.
    
4. `SELECT` — displays the department and its student count.

**Important:** Without GROUP BY, `COUNT(*)` would return one count for the entire table. With GROUP BY, it returns one count per department.


## **3. Using multiple aggregate functions with GROUP BY**

You aren't limited to using just one aggregate function.

You can use COUNT(), AVG(), MIN(), MAX(), and SUM() together.

**Example: Calculate department-wise statistics.**

```
SELECT
    department,
    COUNT(*) AS total_students,
    AVG(age) AS average_age,
    MIN(age) AS youngest_student,
    MAX(age) AS oldest_student
FROM Student
GROUP BY department;
```

Output:

|department|total_students|average_age|youngest|oldest|
|---|---|---|---|---|
|CSE|3|19.33|19|20|
|IT|2|20.50|20|21|
|ECE|1|22.00|22|22|

Each aggregate function operates separately on each department's group.


## **4. GROUP BY with multiple columns**

You can group records using more than one column.

For example:

```
SELECT department, city, COUNT(*) AS total_students
FROM Student
GROUP BY department, city;
```

Here, SQL creates groups based on **unique combinations** of department and city.

Suppose the records are:

|Student|Department|City|
|---|---|---|
|Faizan|CSE|Hazaribagh|
|Aman|CSE|Ranchi|
|Rahul|CSE|Hazaribagh|
|Priya|IT|Ranchi|
|Rohit|IT|Ranchi|

The result:

|Department|City|Count|
|---|---|---|
|CSE|Hazaribagh|2|
|CSE|Ranchi|1|
|IT|Ranchi|2|

Notice that CSE appears twice because it has students from two different cities.

**Remember:** `GROUP BY department, city` doesn't mean grouping by department first and then independently grouping by city. It groups by the combined department-city pair.


## **5. What is HAVING?**

Now let's introduce another important SQL clause: **HAVING**.

Suppose you want to find departments that have at least two students.

We can group students by department and count them. But how do we filter the resulting groups?

That's where HAVING comes in.

**Definition:** HAVING is an SQL clause used to filter groups created by GROUP BY, usually based on aggregate conditions.

Example:

```
SELECT department, COUNT(*) AS total_students
FROM Student
GROUP BY department
HAVING COUNT(*) >= 2;
```

Output:

|department|total_students|
|---|---|
|CSE|3|
|IT|2|

ECE is excluded because it has only one student.

The important point:

- GROUP BY creates groups.
    
- HAVING filters those groups.


## **6. WHERE vs HAVING**

This is one of the most important distinctions in SQL.

Both WHERE and HAVING filter data, but they operate at different stages.

|WHERE|HAVING|
|---|---|
|Filters individual rows|Filters groups|
|Works before GROUP BY|Works after GROUP BY|
|Usually filters using individual column values|Often filters using aggregate results|
|Cannot directly use aggregate functions at the same query level|Can use aggregate functions|

Let's understand with an example.

**WHERE example:**

Find students whose age is greater than or equal to 20, before grouping them.

```
SELECT department, COUNT(*) AS total
FROM Student
WHERE age >= 20
GROUP BY department;
```

Here, SQL removes students younger than 20 first. It then groups the remaining students by department.

**HAVING example:**

Find departments that have at least two students.

```
SELECT department, COUNT(*) AS total
FROM Student
GROUP BY department
HAVING COUNT(*) >= 2;
```

Here, SQL first creates department groups and then removes groups that don't satisfy the condition.


## **7. Using WHERE and HAVING together**

You can use both clauses in the same query.

Consider this requirement:

Find departments where:

- Only students aged 20 or above are considered.
    
- After filtering, the department must have at least two students.

SQL:

```
SELECT
    department,
    COUNT(*) AS total_students,
    AVG(age) AS average_age
FROM Student
WHERE age >= 20
GROUP BY department
HAVING COUNT(*) >= 2;
```

Let's break down the execution:

1. `FROM Student` — access student records.
    
2. `WHERE age >= 20` — remove students younger than 20.
    
3. `GROUP BY department` — create department groups from remaining records.
    
4. `HAVING COUNT(*) >= 2` — keep only groups with at least two students.
    
5. `SELECT` — display the requested results.


## **8. Logical order of SQL query execution**

This is an important concept because SQL's written order isn't always its logical processing order.

For queries involving grouping, the simplified logical order is:

1. **FROM**
    
    Identify the source table.
    
2. **WHERE**
    
    Filter individual rows.
    
3. **GROUP BY**
    
    Create groups from the remaining rows.
    
4. **HAVING**
    
    Filter the groups.
    
5. **SELECT**
    
    Choose the columns and calculate expressions.
    
6. **ORDER BY**
    
    Sort the resulting records.
    
7. **LIMIT / OFFSET**
    
    Restrict the returned rows.
    

This is the simplified logical order, not necessarily the physical execution strategy chosen by the database optimizer.


## **9. Important GROUP BY rules**

These rules will save you from common SQL errors.

**Rule 1: Every selected non-aggregated column must generally be included in GROUP BY.**

Incorrect:

```
SELECT department, name, COUNT(*)
FROM Student
GROUP BY department;
```

Why is this incorrect?

Because a department may contain multiple students. Which student's name should SQL display for that department?

There is no single unambiguous answer.

Correct:

```
SELECT department, COUNT(*)
FROM Student
GROUP BY department;
```

Or, if you want individual student names, include `name` in the grouping — but that changes what each group represents.

**Rule 2: GROUP BY does not guarantee sorting.**

If you want sorted results, use ORDER BY.

```
SELECT department, COUNT(*)
FROM Student
GROUP BY department
ORDER BY department ASC;
```

**Rule 3: NULL values form their own group.**

If some students have NULL as their department, GROUP BY places all those NULL department values into one group.

**Rule 4: Aggregate functions ignore NULL values where applicable.**

For example, `COUNT(age)` excludes rows with NULL age, but `COUNT(*)` counts every row in the group.


## **10. Practical applications in backend development**

GROUP BY and HAVING are heavily used in real-world applications.

|Application|SQL use case|
|---|---|
|E-commerce dashboard|Total orders per status|
|Admin dashboard|Users registered per month|
|College management|Students per department|
|Sales analytics|Revenue per product|
|User analytics|Number of users per country|

For example, imagine you're building an admin dashboard for an e-commerce application.

You want to display how many orders are pending, completed, and cancelled.

```
SELECT status, COUNT(*) AS total_orders
FROM orders
GROUP BY status;
```

This is the kind of SQL you'll regularly write in backend development.


## **Quick revision**

|Concept|Meaning|
|---|---|
|GROUP BY|Groups rows with matching values|
|HAVING|Filters groups|
|WHERE|Filters individual rows|
|Multiple-column GROUP BY|Groups by unique combinations|
|Aggregate with GROUP BY|Calculates results separately per group|