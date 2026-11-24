

## **1. What is DDL?**

**DDL (Data Definition Language) is a category of SQL commands used to define, create and modify the structure of database objects.**

Database objects include:

- Databases
    
- Tables
    
- Schemas
    
- Views
    
- Indexes
    
- Other database structures

DDL primarily deals with the structure of a database, not the individual records stored inside tables.

### **Main DDL commands**

|Command|Purpose|
|---|---|
|CREATE|Creates a database object|
|ALTER|Modifies an existing object's structure|
|DROP|Deletes a database object|
|TRUNCATE|Removes all records from a table|
|RENAME|Changes an object's name|

We'll learn each one.


## **2. CREATE DATABASE**

The `CREATE DATABASE` command creates a new database.

**Syntax:**

```
CREATE DATABASE database_name;
```

Example:

```
CREATE DATABASE college_db;
```

This creates a database named `college_db`.

### **How to connect to it?**

Creating a database and connecting to it are two different operations.

In PostgreSQL, if you're using the `psql` terminal:

```
\c college_db
```

This connects your current session to `college_db`.

If you're using pgAdmin, you can select the database through its interface.

PostgreSQL note: `\c` is a psql meta-command, not standard SQL. PostgreSQL also does not allow CREATE DATABASE inside a transaction block.


## **3. CREATE TABLE**

A table stores data in the form of rows and columns.

The `CREATE TABLE` command creates a new table inside a database.

**Syntax:**

```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype
);
```

Let's create a Student table.

```
CREATE TABLE Student (
    student_id INT,
    name VARCHAR(100),
    age INT,
    email VARCHAR(150)
);
```

Here:

|Column|Data type|Meaning|
|---|---|---|
|student_id|INT|Integer value|
|name|VARCHAR(100)|Text up to 100 characters|
|age|INT|Integer value|
|email|VARCHAR(150)|Text up to 150 characters|

We have created the table structure, but it contains no records yet.

### **Common PostgreSQL data types**

Understanding data types is important because they determine what kind of values a column can store.

|Data type|Purpose|Example|
|---|---|---|
|SMALLINT|Small-range integer|100|
|INT / INTEGER|Integer|25000|
|BIGINT|Large-range integer|9876543210|
|DECIMAL(p,s)|Exact decimal number|99.99|
|NUMERIC(p,s)|Exact decimal number|1500.50|
|REAL|Approximate floating-point number|3.14|
|DOUBLE PRECISION|Higher-precision approximate floating-point|3.141592|
|CHAR(n)|Fixed-length character string|'A'|
|VARCHAR(n)|Variable-length string with limit|'Faizan'|
|TEXT|Variable-length text without a declared limit|'Hello World'|
|BOOLEAN|True/false value|TRUE|
|DATE|Calendar date|'2026-11-24'|
|TIME|Time of day|'14:30:00'|
|TIMESTAMP|Date and time|'2026-11-24 14:30:00'|

A few distinctions:

**VARCHAR vs TEXT**

- `VARCHAR(100)` limits the value to 100 characters.
    
- `TEXT` does not require a declared maximum length.
    
- PostgreSQL handles both efficiently for ordinary text storage.

**CHAR vs VARCHAR**

- `CHAR(n)` is fixed-length and blank-pads shorter values.
    
- `VARCHAR(n)` stores variable-length strings without that fixed-width padding behaviour.

**DECIMAL vs REAL**

- DECIMAL/NUMERIC stores exact decimal values.
    
- REAL and DOUBLE PRECISION store approximate floating-point values.

For financial amounts, prefer `NUMERIC` or `DECIMAL` rather than floating-point types.

Example:

```
salary NUMERIC(10, 2)
```

This means:

- Total precision: 10 digits.
    
- Digits after decimal: 2.
    
- Maximum positive value: 99,999,999.99.


## **4. CREATE TABLE with Constraints**

Earlier, in Phase 4, we studied integrity constraints.

Now we'll implement them in SQL.

Consider this improved Student table:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE,
    age INT CHECK (age >= 16),
    city VARCHAR(50) DEFAULT 'Hazaribagh'
);
```

Let's understand each constraint.

|Constraint|Purpose|
|---|---|
|PRIMARY KEY|Uniquely identifies each record and prohibits NULL|
|NOT NULL|Prevents NULL values|
|UNIQUE|Prevents duplicate values under the database's uniqueness rules|
|CHECK|Enforces a specified condition|
|DEFAULT|Supplies a default value when a column is omitted or DEFAULT is requested|

### **Example: Foreign Key**

Let's create a Department table and connect it to Student.

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);
```

Now modify our Student table design to include a foreign key:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

The foreign key ensures that a non-NULL `department_id` in Student refers to an existing department.

**Important:** In a real project, decide the complete table structure before creating it. We are using separate examples here to understand the syntax.


## **5. ALTER TABLE**

The `ALTER TABLE` command modifies the structure of an existing table.

You can:

- Add a column.
    
- Change a column's data type.
    
- Rename a column.
    
- Drop a column.
    
- Add or remove constraints.

### **A. Add a column**

Suppose our Student table does not have a phone number column.

```
ALTER TABLE Student
ADD COLUMN phone_number VARCHAR(15);
```

Now Student has an additional column.

### **B. Rename a column**

```
ALTER TABLE Student
RENAME COLUMN phone_number TO mobile_number;
```

### **C. Change a column's data type**

```
ALTER TABLE Student
ALTER COLUMN age TYPE SMALLINT;
```

PostgreSQL changes the age column's type, provided the existing data can be converted.

### **D. Set a default value**

```
ALTER TABLE Student
ALTER COLUMN city SET DEFAULT 'Patna';
```

### **E. Remove a default value**

```
ALTER TABLE Student
ALTER COLUMN city DROP DEFAULT;
```

### **F. Add a constraint**

```
ALTER TABLE Student
ADD CONSTRAINT check_age
CHECK (age >= 16);
```

Here:

- `check_age` is the constraint name.
    
- `CHECK (age >= 16)` is the condition.

### **G. Drop a column**

```
ALTER TABLE Student
DROP COLUMN mobile_number;
```

This removes the column and its stored values.


## **6. DROP TABLE**

The `DROP TABLE` command permanently removes a table from the database.

**Syntax:**

```
DROP TABLE table_name;
```

Example:

```
DROP TABLE Student;
```

This removes:

- The Student table structure.
    
- All records stored in that table.
    
- Associated objects owned by that table, subject to dependency rules.

### **DROP TABLE IF EXISTS**

```
DROP TABLE IF EXISTS Student;
```

This prevents an error if the table does not exist.

### **CASCADE and RESTRICT**

PostgreSQL supports dependency handling:

```
DROP TABLE Department CASCADE;
```

`CASCADE` also removes dependent objects, such as foreign key constraints referencing that table.

```
DROP TABLE Department RESTRICT;
```

`RESTRICT` prevents dropping the table if dependent objects exist. This is the default behaviour.

**Be careful with CASCADE.** It can remove more than the object you initially intended to delete.


## **7. TRUNCATE TABLE**

The `TRUNCATE` command removes all records from a table while preserving its structure.

**Syntax:**

```
TRUNCATE TABLE table_name;
```

Example:

```
TRUNCATE TABLE Student;
```

After execution:

- All rows are removed.
    
- Table structure remains.
    
- Columns remain.
    
- Constraints remain.
    
- Indexes remain.

### **TRUNCATE with RESTART IDENTITY**

PostgreSQL supports:

```
TRUNCATE TABLE Student RESTART IDENTITY;
```

This also resets identity sequences owned by the table's columns.

Without `RESTART IDENTITY`, associated sequences are not reset.

### **TRUNCATE with CASCADE**

```
TRUNCATE TABLE Department CASCADE;
```

This also truncates tables that have foreign key references to the specified table, potentially removing a large amount of data.

Use it carefully.


## **8. DROP vs TRUNCATE vs DELETE**

This is one of the most frequently asked SQL interview and examination comparisons.

|Basis|DROP|TRUNCATE|DELETE|
|---|---|---|---|
|Purpose|Removes table|Removes all rows|Removes selected or all rows|
|Structure|Removed|Preserved|Preserved|
|WHERE clause|No|No|Yes|
|Can remove selected rows?|No|No|Yes|
|Table remains?|No|Yes|Yes|
|SQL category|DDL|DDL|DML|
|PostgreSQL rollback|Yes, in a transaction|Yes, in a transaction|Yes, in a transaction|

### **Example 1: DELETE**

```
DELETE FROM Student
WHERE age < 18;
```

Only students younger than 18 are deleted.

### **Example 2: TRUNCATE**

```
TRUNCATE TABLE Student;
```

All records are deleted, but the table remains.

### **Example 3: DROP**

```
DROP TABLE Student;
```

The entire table is removed.

**Visual comparison**

**Before**

|id|name|
|---|---|
|1|Rahul|
|2|Aman|
|3|Priya|

**DELETE**
Selected rows removed
==🟢**Table remains**==

**TRUNCATE**
All rows removed
==🟢**Table remains**==

**DROP**
Entire table removed
==🔴**No table**==

**Important PostgreSQL detail:** DELETE and TRUNCATE are both transactional in PostgreSQL. Their changes can be rolled back before committing. TRUNCATE is generally faster for removing all rows because it does not process each row individually in the same way as DELETE.


## **9. RENAME TABLE**

Renaming a table changes its name without removing its records.

In PostgreSQL:

```
ALTER TABLE Student
RENAME TO College_Student;
```

The table is now called `College_Student`.

The records remain unchanged.

You can also rename a column:

```
ALTER TABLE College_Student
RENAME COLUMN name TO student_name;
```


## **10. Practical Example — College Database**

Let's combine the concepts.

Suppose we're creating a small college management database.

### **Step 1: Create database**

```
CREATE DATABASE college_db;
```

Connect to it using pgAdmin or `psql`.

### **Step 2: Create Department table**

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL UNIQUE
);
```

### **Step 3: Create Student table**

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE,
    age INT CHECK (age >= 16),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

### **Step 4: Add a column**

```
ALTER TABLE Student
ADD COLUMN phone_number VARCHAR(15);
```

### **Step 5: Rename the column**

```
ALTER TABLE Student
RENAME COLUMN phone_number TO mobile_number;
```

Now you have created a database, defined tables, applied constraints and modified a table structure.

## **11. Quick Revision**

|Command|Remember|
|---|---|
|CREATE DATABASE|Creates a database|
|CREATE TABLE|Creates a table|
|ALTER TABLE|Modifies table structure|
|ADD COLUMN|Adds a column|
|RENAME COLUMN|Renames a column|
|DROP COLUMN|Removes a column|
|DROP TABLE|Removes the entire table|
|TRUNCATE TABLE|Removes all rows|
|RENAME TO|Renames a table|