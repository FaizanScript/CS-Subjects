

### 1. What are Database Languages?

Database Languages are a set of commands used to define, manipulate, retrieve, control and manage data within a database.

We use these commands to communicate with a DBMS.

The most widely used database language is SQL (Structured Query Language).

For example, suppose we have a college database containing a `Students` table.

We might need to:

- Create the Students table.
    
- Insert new student records.
    
- Retrieve student information.
    
- Update existing records.
    
- Delete records.
    
- Control who can access the database.
    
- Manage transactions.

SQL provides commands for all these operations.

Traditionally, SQL commands are classified into five categories:

1. **DDL — Data Definition Language**
    
2. **DML — Data Manipulation Language**
    
3. **DQL — Data Query Language**
    
4. **DCL — Data Control Language**
    
5. **TCL — Transaction Control Language**


### 2. DDL — Data Definition Language

**Definition**: DDL is a category of SQL commands used to define, create and modify the structure or schema of database objects, such as tables, views and indexes.

In simple terms, DDL deals with the structure of a database, not primarily with the actual records stored inside it.

Important DDL commands:

|Command|Purpose|
|---|---|
|`CREATE`|Creates database objects.|
|`ALTER`|Modifies the structure of existing objects.|
|`DROP`|Deletes database objects.|
|`TRUNCATE`|Removes all records from a table while keeping its structure.|
|`RENAME`|Renames a database object (syntax varies by DBMS).|

Examples:

1. **CREATE — Create a table**

```
CREATE TABLE Students (
    student_id INT,
    name VARCHAR(100),
    age INT
);
```

This creates a table named `Students` with three columns.

2. **ALTER — Modify table structure**

```
ALTER TABLE Students
ADD email VARCHAR(100);
```

This adds a new `email` column to the existing table.

3. **DROP — Delete a table**

```
DROP TABLE Students;
```

This removes the entire `Students` table, including its structure and records.

4. **TRUNCATE — Remove all records**

```
TRUNCATE TABLE Students;
```

This removes all records but keeps the table structure.

Important distinction: DROP vs TRUNCATE vs DELETE

|Command|What happens?|
|---|---|
|`DROP`|Removes the entire table.|
|`TRUNCATE`|Removes all rows, keeps table structure.|
|`DELETE`|Removes selected rows or all rows, keeps table structure.|

Transaction and rollback behavior varies by DBMS. For example, PostgreSQL supports transactional TRUNCATE, while other systems may behave differently.


### 3. DML — Data Manipulation Language

**Definition**: DML is a category of SQL commands used to insert, modify and delete records stored in database tables.

In simple terms, DML deals with the actual data inside the database.

**Important DML commands:**

|Command|Purpose|
|---|---|
|`INSERT`|Adds new records.|
|`UPDATE`|Modifies existing records.|
|`DELETE`|Removes records.|

Examples:

Assume we already have a `Students` table.

1. **INSERT — Add a new student**

```
INSERT INTO Students (student_id, name, age)
VALUES (101, 'Faizan', 20);
```

This inserts a new student record.

2. **UPDATE — Modify existing data**

```
UPDATE Students
SET age = 21
WHERE student_id = 101;
```

This updates the age of the student whose ID is 101.

3. **DELETE — Remove a record**

```
DELETE FROM Students
WHERE student_id = 101;
```

This deletes the record of the student whose ID is 101.

Important: Always understand the role of `WHERE`.

Without `WHERE`, an `UPDATE` or `DELETE` statement can affect every row in the table.


### 4. DQL — Data Query Language

**Definition**: DQL is commonly used as a classification for SQL commands that retrieve or query data from a database without modifying the stored records.

Its primary command is:

**`SELECT`**

Example:

```
SELECT * FROM Students;
```

This retrieves all columns and records from the `Students` table.

Another example:

```
SELECT name, age
FROM Students
WHERE age > 18;
```

This retrieves the names and ages of students older than 18.

Important distinction:

- DML: Inserts, updates and deletes records.
    
- DQL: Retrieves records.

A technical note: Some textbooks classify `SELECT` under DML rather than treating DQL as a separate category. Both classifications are encountered in SQL education.


### 5. DCL — Data Control Language

**Definition**: DCL is a category of SQL commands used to control database access and permissions by granting or revoking privileges to database users.

In simple terms, DCL deals with who is allowed to perform which operations.

Important DCL commands:

|Command|Purpose|
|---|---|
|`GRANT`|Gives permissions to users.|
|`REVOKE`|Removes previously granted permissions.|

Examples:

1. GRANT

```
GRANT SELECT
ON Students
TO student_user;
```

This grants `student_user` permission to retrieve records from the `Students` table.

2. REVOKE

```
REVOKE SELECT
ON Students
FROM student_user;
```

This removes that SELECT permission.

Real database permissions can be more detailed, including permissions for inserting, updating, deleting and modifying objects.


### 6. TCL — Transaction Control Language

**Definition**: TCL is a category of SQL commands used to manage transactions and control whether database changes are permanently saved or rolled back.

First, understand what a transaction is.

A transaction is a logical unit of database operations that is treated as one complete operation.

For example, consider transferring ₹1,000 from Account A to Account B.

Two operations are required:

1. Deduct ₹1,000 from Account A.
    
2. Add ₹1,000 to Account B.

Both operations should succeed together. If one fails, we should avoid leaving the accounts in an inconsistent state.

This is where transactions become important.

Important TCL commands:

|Command|Purpose|
|---|---|
|`COMMIT`|Permanently saves the transaction's changes.|
|`ROLLBACK`|Undoes uncommitted changes.|
|`SAVEPOINT`|Creates a point within a transaction to which we can roll back.|

Examples:

1. **COMMIT**

```
BEGIN;

UPDATE Accounts
SET balance = balance - 1000
WHERE account_id = 101;

UPDATE Accounts
SET balance = balance + 1000
WHERE account_id = 102;

COMMIT;
```

`COMMIT` saves both updates as part of the transaction.

2. ROLLBACK

```
BEGIN;

UPDATE Accounts
SET balance = balance - 1000
WHERE account_id = 101;

ROLLBACK;
```

`ROLLBACK` undoes the uncommitted update.

3. SAVEPOINT

```
BEGIN;

UPDATE Students
SET age = 21
WHERE student_id = 101;

SAVEPOINT sp1;

UPDATE Students
SET age = 22
WHERE student_id = 102;

ROLLBACK TO SAVEPOINT sp1;

COMMIT;
```

Here:

- First update is performed.
    
- A savepoint is created.
    
- Second update is performed.
    
- We roll back to `sp1`, undoing the second update.
    
- `COMMIT` saves the first update.


### 7. Complete Comparison of SQL Command Categories

| Category | Full Form                    | Main Purpose               | Commands                      |
| -------- | ---------------------------- | -------------------------- | ----------------------------- |
| DDL      | Data Definition Language     | Defines database structure | CREATE, ALTER, DROP, TRUNCATE |
| DML      | Data Manipulation Language   | Modifies stored records    | INSERT, UPDATE, DELETE        |
| DQL      | Data Query Language          | Retrieves records          | SELECT                        |
| DCL      | Data Control Language        | Controls permissions       | GRANT, REVOKE                 |
| TCL      | Transaction Control Language | Manages transactions       | COMMIT, ROLLBACK, SAVEPOINT   |

Easy way to remember

- DDL → Structure
    
- DML → Data modification
    
- DQL → Data retrieval
    
- DCL → Permissions
    
- TCL → Transactions

## Quick Revision

Before moving ahead, make sure you can explain these distinctions:

1. DDL vs DML: Structure vs stored records.
    
2. DML vs DQL: Modifying data vs retrieving data.
    
3. DCL: Managing user permissions.
    
4. TCL: Managing transactions.
    
5. DROP vs TRUNCATE vs DELETE: Removing a table vs removing all rows vs removing selected or all rows.