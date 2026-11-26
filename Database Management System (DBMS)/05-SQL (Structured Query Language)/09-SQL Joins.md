
This is one of the most important topics in SQL.

So far, most of our queries have worked with a single table. But real-world databases rarely store everything in one table.

Consider an e-commerce application:

- `Customers` stores customer information.
    
- `Orders` stores order information.
    
- `Products` stores product information.
    
- `Payments` stores payment information.

These tables are connected through relationships.

But suppose you want to display:

**A customer's name, their order ID, and the order amount.**

The customer's name is in `Customers`, while the order details are in `Orders`.

How do we retrieve information from both tables together?

That's exactly what SQL JOINs are designed for.



## **1. What is a JOIN?**

**Definition:** A JOIN is an SQL operation used to combine rows from two or more tables based on a related column or a specified join condition.

In simple terms, JOIN allows us to retrieve related information stored in different tables.

Let's understand this practically.

### **Our sample database**

We'll use two tables throughout this topic.

**Customers table**

|customer_id|name|
|---|---|
|1|Faizan|
|2|Aman|
|3|Rahul|
|4|Priya|

**Orders table**

|order_id|customer_id|amount|
|---|---|---|
|101|1|5000|
|102|2|3000|
|103|1|2000|
|104|5|4000|

Notice something important:

- `customer_id` in Customers is the Primary Key.
    
- `customer_id` in Orders is a Foreign Key in our intended design.
    
- Customer 1 has placed two orders.
    
- Customer 3 and 4 have not placed any orders.
    
- Order 104 references customer 5, who doesn't exist in Customers.

**Important:** That last row would violate a properly enforced foreign key constraint. We'll keep it temporarily to demonstrate how joins behave when there are unmatched records.

**How the tables are related**

**Customers**                                     **orders**
# **PK**              ->/<-        FK
customer_id                                  customer_id


One customer can have multiple orders: a one-to-many relationship.


## **2. Types of SQL JOINs**

There are several types of JOINs:

1. INNER JOIN
    
2. LEFT JOIN (LEFT OUTER JOIN)
    
3. RIGHT JOIN (RIGHT OUTER JOIN)
    
4. FULL OUTER JOIN
    
5. CROSS JOIN
    
6. SELF JOIN

We'll understand each one individually.


## **3. INNER JOIN**

**Definition:** INNER JOIN returns only those rows where the join condition matches in both tables.

In simpler words:

It returns only the matching records from both tables.

**Syntax:**

```
SELECT columns
FROM table1
INNER JOIN table2
ON table1.common_column = table2.common_column;
```

Let's apply it to our tables.

```
SELECT
    Customers.customer_id,
    Customers.name,
    Orders.order_id,
    Orders.amount
FROM Customers
INNER JOIN Orders
ON Customers.customer_id = Orders.customer_id;
```

**Output:**

|customer_id|name|order_id|amount|
|---|---|---|---|
|1|Faizan|101|5000|
|2|Aman|102|3000|
|1|Faizan|103|2000|

Why didn't Rahul and Priya appear?

Because they don't have matching orders.

Why didn't order 104 appear?

Because customer_id 5 doesn't exist in Customers.

Only matching records are returned.

**Remember:** `INNER JOIN` and `JOIN` mean the same thing in SQL. Writing INNER is optional.


## **4. LEFT JOIN (LEFT OUTER JOIN)**

Now suppose the requirement changes:

You want to display **all customers, including customers who haven't placed any orders.**

INNER JOIN won't work because it excludes unmatched customers.

We need LEFT JOIN.

**Definition:** LEFT JOIN returns all rows from the left table and matching rows from the right table. If no match exists, the right table's columns contain NULL.

**Syntax:**

```
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.common_column = table2.common_column;
```

Example:

```
SELECT
    Customers.customer_id,
    Customers.name,
    Orders.order_id,
    Orders.amount
FROM Customers
LEFT JOIN Orders
ON Customers.customer_id = Orders.customer_id;
```

**Output:**

|customer_id|name|order_id|amount|
|---|---|---|---|
|1|Faizan|101|5000|
|1|Faizan|103|2000|
|2|Aman|102|3000|
|3|Rahul|NULL|NULL|
|4|Priya|NULL|NULL|

Notice:

- All four customers appear.
    
- Faizan appears twice because he has two orders.
    
- Rahul and Priya appear with NULL order details because they have no matching orders.

**Important:** The left table is the table written immediately after FROM.

In our query:

```
FROM Customers
LEFT JOIN Orders
```

Customers is the left table.


## **5. RIGHT JOIN (RIGHT OUTER JOIN)**

RIGHT JOIN is the opposite of LEFT JOIN.

**Definition:** RIGHT JOIN returns all rows from the right table and matching rows from the left table. If no match exists, the left table's columns contain NULL.

Example:

```
SELECT
    Customers.customer_id,
    Customers.name,
    Orders.order_id,
    Orders.amount
FROM Customers
RIGHT JOIN Orders
ON Customers.customer_id = Orders.customer_id;
```

**Output:**

|customer_id|name|order_id|amount|
|---|---|---|---|
|1|Faizan|101|5000|
|2|Aman|102|3000|
|1|Faizan|103|2000|
|NULL|NULL|104|4000|

Why does order 104 appear?

Because RIGHT JOIN preserves every row from Orders, even if no matching customer exists.

**Practical note:** RIGHT JOIN is less commonly used because you can usually rewrite it as LEFT JOIN by swapping the table order.


## **6. FULL OUTER JOIN**

Now suppose you want:

- Every customer, whether they have an order or not.
    
- Every order, whether it has a matching customer or not.

We need FULL OUTER JOIN.

**Definition:** FULL OUTER JOIN returns all matching and unmatched rows from both tables. Unmatched columns from the opposite table are filled with NULL.

Example:

```
SELECT
    Customers.customer_id,
    Customers.name,
    Orders.order_id,
    Orders.amount
FROM Customers
FULL OUTER JOIN Orders
ON Customers.customer_id = Orders.customer_id;
```

**Output:**

|customer_id|name|order_id|amount|
|---|---|---|---|
|1|Faizan|101|5000|
|1|Faizan|103|2000|
|2|Aman|102|3000|
|3|Rahul|NULL|NULL|
|4|Priya|NULL|NULL|
|NULL|NULL|104|4000|

It includes:

- Matching records.
    
- Customers without orders.
    
- Orders without matching customers.

**PostgreSQL supports FULL OUTER JOIN directly.** Some databases, such as MySQL, do not support this syntax natively.


## **7. Comparing the four main JOINs**

This is worth remembering.

|JOIN|What it returns|
|---|---|
|INNER JOIN|Only matching rows|
|LEFT JOIN|All left rows + matching right rows|
|RIGHT JOIN|All right rows + matching left rows|
|FULL OUTER JOIN|All rows from both tables|


## **8. CROSS JOIN**

Until now, our joins have used matching conditions.

But what if you want every possible combination of rows from two tables?

That's where CROSS JOIN comes in.

**Definition:** CROSS JOIN returns the Cartesian product of two tables, combining every row from the first table with every row from the second table.

Example tables:

**Colors**

|color|
|---|
|Red|
|Blue|

**Sizes**

|size|
|---|
|Small|
|Medium|
|Large|

Query:

```
SELECT
    Colors.color,
    Sizes.size
FROM Colors
CROSS JOIN Sizes;
```

Output:

|color|size|
|---|---|
|Red|Small|
|Red|Medium|
|Red|Large|
|Blue|Small|
|Blue|Medium|
|Blue|Large|

Total combinations:

	2 x 3 = 6

If table A has 5 rows and table B has 4 rows:

	5 x 4 = 20 rows

**Practical use cases:**

- Generating all product-size combinations.
    
- Generating possible date and time combinations.
    
- Creating combinations for testing.

Be careful: CROSS JOIN can produce a huge number of rows.


## **9. SELF JOIN**

Sometimes a table contains a relationship with itself.

Consider an Employee table:

|employee_id|name|manager_id|
|---|---|---|
|1|Faizan|NULL|
|2|Aman|1|
|3|Rahul|1|
|4|Priya|2|

Here:

- `employee_id` identifies an employee.
    
- `manager_id` references another employee's `employee_id`

For example, Aman and Rahul report to Faizan.

But how do we retrieve both employee names and their manager names from the same table?

We use SELF JOIN.

**Definition:** SELF JOIN is a technique where a table is joined with itself to compare or retrieve related rows within that same table.

Example:

```
SELECT
    Employee.name AS employee_name,
    Manager.name AS manager_name
FROM Employee
INNER JOIN Employee AS Manager
ON Employee.manager_id = Manager.employee_id;
```

Output:

|employee_name|manager_name|
|---|---|
|Aman|Faizan|
|Rahul|Faizan|
|Priya|Aman|

What's happening?

We're using the same Employee table twice:

- `Employee` represents the employee.
    
- `Manager` represents the manager.

These are table aliases. They let us refer to the same physical table as two separate logical roles in the query.

Notice that Faizan doesn't appear because his manager_id is NULL, and we used INNER JOIN.

If we wanted to include Faizan too, we could use LEFT JOIN.


## **10. JOIN with WHERE**

You can combine JOINs with WHERE to filter joined results.

Example: Find Faizan's orders only.

```
SELECT
    Customers.name,
    Orders.order_id,
    Orders.amount
FROM Customers
INNER JOIN Orders
ON Customers.customer_id = Orders.customer_id
WHERE Customers.name = 'Faizan';
```

Output:

|name|order_id|amount|
|---|---|---|
|Faizan|101|5000|
|Faizan|103|2000|

JOIN combines related information. WHERE filters the resulting rows.


## **11. JOIN with three or more tables**

In real backend applications, you'll frequently join multiple tables.

Let's introduce another table.

**Products**

|product_id|product_name|
|---|---|
|1|Laptop|
|2|Mouse|

**OrderItems**

|order_id|product_id|quantity|
|---|---|---|
|101|1|1|
|102|2|2|
|103|2|1|

Suppose you want to display:

- Customer name.
    
- Order ID.
    
- Product name.
    
- Quantity.

You need four tables:

1. Customers
    
2. Orders
    
3. OrderItems
    
4. Products

Query:

```
SELECT
    Customers.name,
    Orders.order_id,
    Products.product_name,
    OrderItems.quantity
FROM Customers
INNER JOIN Orders
    ON Customers.customer_id = Orders.customer_id
INNER JOIN OrderItems
    ON Orders.order_id = OrderItems.order_id
INNER JOIN Products
    ON OrderItems.product_id = Products.product_id;
```

The result combines information across all four tables.

This is a common pattern in backend development.


## **12. Important concept: ON vs WHERE**

Both ON and WHERE can contain conditions, but they have different responsibilities.

- `ON` specifies the condition used to match rows between tables.
    
- `WHERE` filters rows from the joined result.

For INNER JOIN, some conditions can be moved between ON and WHERE without changing the result.

However, with OUTER JOINs, their placement can significantly change the result.

For example:

```
SELECT *
FROM Customers
LEFT JOIN Orders
    ON Customers.customer_id = Orders.customer_id
    AND Orders.amount > 3000;
```

This preserves every customer, but only matches orders above ₹3,000.

Compare that with:

```
SELECT *
FROM Customers
LEFT JOIN Orders
    ON Customers.customer_id = Orders.customer_id
WHERE Orders.amount > 3000;
```

The WHERE condition removes rows where `Orders.amount` is NULL.

Therefore, customers without qualifying orders disappear. This query effectively behaves like an INNER JOIN for that condition.

**This distinction is extremely important.** Don't blindly move conditions between ON and WHERE.


## **13. Common JOIN mistakes**

|Mistake|Why it's a problem|
|---|---|
|Forgetting the ON condition|Can produce unintended Cartesian combinations|
|Joining unrelated columns|Produces incorrect matches|
|Using SELECT * everywhere|Can produce duplicate column names and unnecessary data|
|Assuming one row per customer|One-to-many relationships can produce multiple rows|
|Using INNER JOIN when unmatched rows matter|Unmatched records disappear|
|Putting outer-join filters in WHERE accidentally|Can remove NULL-extended rows|

One more important point: JOIN doesn't automatically eliminate duplicate-looking rows. If a customer has five orders, their information can appear five times. That's expected behavior, not necessarily a database error.


## **14. Quick revision**

|JOIN|Core idea|
|---|---|
|INNER|Matching records only|
|LEFT|All left-side records|
|RIGHT|All right-side records|
|FULL OUTER|All records from both sides|
|CROSS|Every possible combination|
|SELF|Table joined with itself|

**For backend development, prioritize these:**

1. INNER JOIN
    
2. LEFT JOIN
    
3. Joining three or more tables
    
4. Understanding ON vs WHERE
    
5. SELF JOIN
    

RIGHT JOIN and FULL OUTER JOIN are also worth understanding, but you'll encounter INNER and LEFT JOIN much more frequently.