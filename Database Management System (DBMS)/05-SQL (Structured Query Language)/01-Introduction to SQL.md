
## **Phase overview**

SQL stands for **Structured Query Language**. It is the standard language used to define, manipulate, retrieve and control data in relational database systems.

We'll use **PostgreSQL** as our primary database throughout this phase.

Why PostgreSQL?

- Widely used in real-world backend development.
    
- Supports advanced SQL features.
    
- Excellent support for relational data, constraints, transactions and indexing.
    
- Useful with Node.js, Java, Python and other backend technologies.

The SQL concepts we learn will largely transfer to MySQL, SQL Server and other relational databases, although syntax and behaviour can differ.

### **Complete Phase 5 roadmap**

1. **5.1 Introduction to SQL**
    SQL fundamentals, characteristics, SQL commands and SQL syntax.
    
2. **5.2 Database and Table Creation (DDL)**
    CREATE DATABASE, CREATE TABLE, ALTER TABLE, DROP, TRUNCATE, RENAME.
    
3. **5.3 Data Manipulation (DML)**
    INSERT, UPDATE and DELETE.
    
4. **5.4 Data Retrieval (DQL)**
    SELECT, DISTINCT, column selection and aliases.
    
5. **5.5 Filtering and Sorting**
    WHERE, comparison operators, logical operators, BETWEEN, IN, LIKE, ORDER BY and LIMIT.
    
6. **5.6 SQL Constraints**
    PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK and DEFAULT in practical SQL.
    
7. **5.7 Aggregate Functions**
    COUNT, SUM, AVG, MIN and MAX.
    
8. **5.8 Grouping Data**
    GROUP BY and HAVING.
    
9. **5.9 SQL Joins**
    INNER, LEFT, RIGHT, FULL OUTER, CROSS and SELF JOIN.
    
10. **5.10 Subqueries**
    Single-row, multiple-row, correlated subqueries, EXISTS, IN and ANY/ALL.
    
11. **5.11 Set Operations**
    UNION, UNION ALL, INTERSECT and EXCEPT.
    
12. **5.12 SQL Functions**
    String, numeric, date/time and NULL-handling functions.
    
13. **5.13 Advanced SQL Concepts**
    Views, indexes introduction, CASE, CTEs and window functions.
    
14. **5.14 Practical SQL Project + Revision**
    Build and query a small college database.

We'll learn each topic in sequence. No jumping into advanced queries before understanding the fundamentals.



## **1. What is SQL?**

**SQL (Structured Query Language) is a standard language used to communicate with relational database management systems (RDBMS) to define, manipulate, retrieve and control data.**

In simple terms:

SQL allows us to communicate with databases.

For example, imagine you're building a college management application.

You need to:

- Create a database for the college.
    
- Create tables for students and courses.
    
- Insert student records.
    
- Find students who scored above 80.
    
- Update student information.
    
- Delete records when necessary.

SQL provides commands for all these operations.

Example:

```
SELECT *
FROM Student;
```

This query retrieves all columns and rows from the `Student` table.


## **2. Why do we need SQL?**

Consider a database containing 10,000 student records.

Suppose you want to find students whose marks are greater than 80.

Without a query language, you would need to manually inspect records or write custom code to process them.

Using SQL:

```
SELECT *
FROM Student
WHERE marks > 80;
```

The DBMS processes the query and returns matching records.

SQL makes it possible to interact with large databases efficiently without manually handling every record.


## **3. Characteristics of SQL**

Important characteristics include:

|Characteristic|Explanation|
|---|---|
|Declarative language|You specify what data you want, not every step to retrieve it.|
|Standardized|SQL is based on standards maintained by ISO and ANSI.|
|Database language|Used to interact with relational databases.|
|Supports data definition|Allows creation and modification of database structures.|
|Supports data manipulation|Allows inserting, updating and deleting records.|
|Supports retrieval|Allows querying and filtering data.|
|Supports security|Allows permissions and access control.|
|Supports transactions|Allows transaction management using COMMIT and ROLLBACK.|

### **Declarative vs Procedural Language**

This distinction is particularly important.

**Declarative approach:** Tell the database what you want.

```
SELECT *
FROM Student
WHERE marks > 80;
```

You specify the desired result.

You don't specify:

- How to scan the table.
    
- Which index to use.
    
- How to retrieve each record.
    
- How to organize the execution internally.

The DBMS query optimizer decides an execution strategy.

**Procedural approach:** You specify the steps or procedure required to accomplish a task.

For example, a Java program might iterate through student records, check each student's marks and collect those above 80.

|Declarative|Procedural|
|---|---|
|Specifies what to do|Specifies how to do it|
|SQL is primarily declarative|Java and C are primarily procedural|
|DBMS determines execution strategy|Programmer writes the execution steps|

Note: SQL also has procedural extensions, such as PL/pgSQL in PostgreSQL. But ordinary SQL queries are primarily declarative.


## **4. Categories of SQL Commands**

SQL commands are commonly divided into five categories.

**1. DDL — Data Definition Language**

**==🔵Structure==**

Defines and modifies database objects such as databases, tables and schemas.

```
CREATE
ALTER
DROP
TRUNCATE
```

**2. DML — Data Manipulation Language**

**==🔵Data modification==**

Inserts, modifies and deletes records.

```
INSERT
UPDATE
DELETE
```

**3. DQL — Data Query Language**

**==🔵Data retrieval==**

Retrieves data from database tables.

```
SELECT
```

**4. DCL — Data Control Language**

**==🔵Permissions==**

Controls database access and permissions.

```
GRANT
REVOKE
```

**5. TCL — Transaction Control Language**

**==🔵Transactions==**

Manages database transactions.

```
COMMIT
ROLLBACK
SAVEPOINT
```

### **Quick examples**

|Command|Purpose|Category|
|---|---|---|
|CREATE TABLE|Create a table|DDL|
|ALTER TABLE|Modify table structure|DDL|
|INSERT INTO|Insert records|DML|
|UPDATE|Modify records|DML|
|DELETE|Delete records|DML|
|SELECT|Retrieve records|DQL|
|GRANT|Give permissions|DCL|
|REVOKE|Remove permissions|DCL|
|COMMIT|Permanently commit a transaction|TCL|
|ROLLBACK|Undo uncommitted transaction changes|TCL|

**Important textbook note:** Some books classify `SELECT` under DML rather than treating DQL as a separate category. Both classifications are encountered in academic material. We'll use the five-category classification above.


## **5. SQL Syntax and Basic Rules**

Before writing complex queries, understand a few basic rules.

### **A. SQL keywords**

SQL keywords are reserved words that have predefined meanings.

Examples:

```
SELECT
FROM
WHERE
INSERT
UPDATE
DELETE
CREATE
TABLE
```

SQL keywords are generally case-insensitive in PostgreSQL.

Both queries work:

```
SELECT * FROM Student;
```

```
select * from Student;
```

However, using uppercase for SQL keywords is a common formatting convention because it improves readability.

### **B. SQL statements end with a semicolon**

```
SELECT *
FROM Student;
```

A semicolon (`;`) marks the end of an SQL statement in common SQL clients.

### **C. SQL comments**

Comments are ignored by the SQL parser.

Single-line comment:

```
-- Retrieve all student records
SELECT *
FROM Student;
```

Multi-line comment:

```
/*
This query retrieves
all student records.
*/

SELECT *
FROM Student;
```

### **D. Identifiers**

Identifiers are names assigned to database objects, such as:

- Database names
    
- Table names
    
- Column names
    
- Index names

Example:

```
CREATE TABLE Student (
    student_id INT,
    name VARCHAR(100)
);
```

Here:

- `Student` is a table identifier.
    
- `student_id` and `name` are column identifiers.

PostgreSQL folds unquoted identifiers to lowercase. Quoted identifiers preserve their case and become case-sensitive.

For example:

```
CREATE TABLE Student (...);
```

Creates an unquoted table identifier that PostgreSQL treats as `student`.

### **E. String values**

Text values are generally enclosed in single quotes.

```
SELECT *
FROM Student
WHERE name = 'Rahul';
```

Do not confuse:

- `'Rahul'` — string value.
    
- `name` — column identifier.
    
- `"name"` — quoted identifier in PostgreSQL.


## **6. SQL vs PostgreSQL vs DBMS**

These terms are related but not interchangeable.

|Term|Meaning|
|---|---|
|SQL|Language used to communicate with relational databases|
|DBMS|Software used to manage databases|
|RDBMS|DBMS based on the relational model|
|PostgreSQL|An open-source relational database management system|
|MySQL|Another relational database management system|

Think of it this way:

- SQL is the language.
    
- PostgreSQL is the software that understands and executes SQL commands.
    
- The database is where the actual data is stored.

Other RDBMS products also understand SQL, but they may implement different dialects and features.

## **7. Quick Revision**

1. SQL stands for Structured Query Language.
    
2. SQL is primarily declarative.
    
3. SQL is used to define, manipulate, retrieve and control relational data.
    
4. DDL handles database structure.
    
5. DML handles data modification.
    
6. DQL handles data retrieval.
    
7. DCL handles permissions.
    
8. TCL handles transactions.
    
9. SQL keywords are generally case-insensitive.
    
10. SQL statements commonly end with semicolons.