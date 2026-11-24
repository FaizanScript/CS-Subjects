

### **1. What is DML?**

**Data Manipulation Language (DML)** is a category of SQL commands used to insert, modify, and delete records stored in database tables.

The three primary DML commands are:

|Command|Purpose|
|---|---|
|`INSERT`|Add new records|
|`UPDATE`|Modify existing records|
|`DELETE`|Remove records|

Remember the distinction:

- **DDL** changes the database structure.
    
- **DML** changes the data stored inside that structure.

For example:

```
-- DDL: Create a table
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);

-- DML: Insert a record
INSERT INTO Student (student_id, name, age)
VALUES (101, 'Faizan', 20);
```

The first statement creates the table. The second inserts a record into it.


### **2. INSERT — Adding Records**

The `INSERT` command adds new rows to an existing table.

**Basic syntax:**

```
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

Let's use this table:

**Student**

|student_id|name|age|city|
|---|---|---|---|
|101|Faizan|20|Hazaribagh|
|102|Aman|21|Ranchi|

#### **A. Inserting a single record**

```
INSERT INTO Student (student_id, name, age, city)
VALUES (103, 'Rahul', 19, 'Patna');
```

This inserts one new row.

**Why specify column names?**

You should generally specify column names explicitly because:

- You can insert values into selected columns.
    
- You don't have to depend on the table's physical column order.
    
- Your query is easier to understand and maintain.
    
- Adding a new column to the table is less likely to break your query.

#### **B. Inserting multiple records**

You can insert multiple rows using one `INSERT` statement.

```
INSERT INTO Student (student_id, name, age, city)
VALUES
    (104, 'Priya', 20, 'Delhi'),
    (105, 'Rohit', 22, 'Mumbai'),
    (106, 'Ankit', 19, 'Ranchi');
```

This inserts three records in one statement.

#### **C. Inserting into selected columns**

Suppose you don't know a student's city yet.

You can omit the `city` column:

```
INSERT INTO Student (student_id, name, age)
VALUES (107, 'Neha', 20);
```

What happens to `city`?

It depends on its definition:

- If it has a `DEFAULT` value, that default is used.
    
- If it allows `NULL` and has no default, it becomes `NULL`.
    
- If it is `NOT NULL` and has no default, the insertion fails.

#### **D. Inserting NULL explicitly**

```
INSERT INTO Student (student_id, name, age, city)
VALUES (108, 'Karan', 21, NULL);
```

This explicitly inserts `NULL` into `city`.

Remember: `NULL` means an unknown, missing, or inapplicable value. It is not the same as an empty string (`''`).

#### **E. Using DEFAULT**

You can explicitly request a column's default value:

```
INSERT INTO Student (student_id, name, age, city)
VALUES (109, 'Meera', 20, DEFAULT);
```

This uses the default defined for `city`, if one exists.

#### **F. INSERT INTO ... SELECT**

This is particularly useful when copying data from one table into another.

```
INSERT INTO Graduated_Students (student_id, name)
SELECT student_id, name
FROM Student
WHERE age >= 21;
```

Here:

1. `SELECT` retrieves qualifying records from `Student`.
    
2. `INSERT` adds those records to `Graduated_Students`.

The selected columns must match the target columns in number and compatible data types.

#### **G. PostgreSQL RETURNING**

PostgreSQL provides a useful `RETURNING` clause.

```
INSERT INTO Student (student_id, name, age, city)
VALUES (110, 'Arjun', 20, 'Delhi')
RETURNING *;
```

Instead of executing a separate `SELECT`, PostgreSQL returns the inserted row.

You can also return specific columns:

```
INSERT INTO Student (student_id, name, age, city)
VALUES (111, 'Isha', 19, 'Ranchi')
RETURNING student_id, name;
```

**Backend connection:** This is useful when your Node.js backend inserts a record and immediately needs the generated ID or inserted data to send back to the frontend.


### **3. UPDATE — Modifying Existing Records**

The `UPDATE` command modifies existing rows in a table.

**Basic syntax:**

```
UPDATE table_name
SET column1 = value1,
    column2 = value2
WHERE condition;
```

#### **A. Updating a single record**

Suppose we want to change Rahul's city from Patna to Delhi.

```
UPDATE Student
SET city = 'Delhi'
WHERE student_id = 103;
```

The `WHERE` condition identifies which row should be updated.

#### **B. Updating multiple columns**

```
UPDATE Student
SET age = 21,
    city = 'Mumbai'
WHERE student_id = 104;
```

Both columns are updated for the matching student.

#### **C. Updating multiple records**

Suppose we want to increase the age of every student whose age is below 20.

```
UPDATE Student
SET age = age + 1
WHERE age < 20;
```

Notice something important:

```
SET age = age + 1
```

The database uses the existing value of `age` and increases it by one.

#### **D. The danger of forgetting WHERE**

Consider:

```
UPDATE Student
SET city = 'Delhi';
```

What happens?

Important

**Every row in the Student table gets updated.**

Without `WHERE`, the command applies to all records.

Before running an UPDATE on important data, first inspect the rows:

```
SELECT *
FROM Student
WHERE student_id = 103;
```

Then execute the corresponding UPDATE.

#### **E. UPDATE with RETURNING**

PostgreSQL also supports `RETURNING` with UPDATE.

```
UPDATE Student
SET city = 'Ranchi'
WHERE student_id = 103
RETURNING *;
```

This returns the modified record.


### **4. DELETE — Removing Records**

The `DELETE` command removes existing rows from a table.

**Basic syntax:**

```
DELETE FROM table_name
WHERE condition;
```

#### **A. Deleting a specific record**

```
DELETE FROM Student
WHERE student_id = 103;
```

This removes the student whose ID is 103.

#### **B. Deleting multiple records**

```
DELETE FROM Student
WHERE age < 18;
```

This removes every student whose age is below 18.

#### **C. Deleting all records**

```
DELETE FROM Student;
```

This removes every row from the table.

However:

- The table still exists.
    
- Its columns remain.
    
- Its constraints remain.
    
- Its indexes remain.

Compare this with `DROP TABLE` and `TRUNCATE`, which we studied in Topic 5.2.

#### **D. DELETE with RETURNING**

```
DELETE FROM Student
WHERE student_id = 103
RETURNING *;
```

PostgreSQL returns the deleted row.

This can be useful when your backend needs to know exactly which record was deleted.


### **5. INSERT vs UPDATE vs DELETE**

|Feature|INSERT|UPDATE|DELETE|
|---|---|---|---|
|Purpose|Add rows|Modify rows|Remove rows|
|Works on existing rows?|No|Yes|Yes|
|Uses `WHERE`?|Usually no|Yes, when targeting rows|Yes, when targeting rows|
|Changes table structure?|No|No|No|
|Supports PostgreSQL `RETURNING`?|Yes|Yes|Yes|

All three commands manipulate records, not the table's schema.


### **6. Important practical concepts**

**A. Foreign key restrictions**

Suppose `Student.department_id` references `Department.department_id`.

If you try to delete a department that still has students referencing it, the database may reject the deletion.

What happens depends on the foreign key's configured action:

- `RESTRICT` / `NO ACTION`: prevent the deletion if references remain.
    
- `CASCADE`: delete referencing student rows too.
    
- `SET NULL`: set their department IDs to NULL.

This is referential integrity in action.

**B. Transactions and rollback**

PostgreSQL allows you to execute DML statements inside transactions.

```
BEGIN;

DELETE FROM Student
WHERE student_id = 103;

ROLLBACK;
```

`ROLLBACK` cancels the uncommitted deletion.

If you want to permanently commit the transaction:

```
BEGIN;

UPDATE Student
SET city = 'Delhi'
WHERE student_id = 103;

COMMIT;
```

We'll study transactions, ACID properties and concurrency control in Phase 8.

**C. A safe habit**

Before running a potentially destructive UPDATE or DELETE:

1. Write the condition.
    
2. Run a `SELECT` with the same condition.
    
3. Verify which rows will be affected.
    
4. Execute the modification.
    
5. Check the result.

This habit prevents avoidable database mistakes.


### **7. Quick Revision**

- `INSERT` adds new rows.
    
- `UPDATE` modifies existing rows.
    
- `DELETE` removes rows.
    
- `WHERE` identifies which existing rows should be modified or deleted.
    
- Omitting `WHERE` from UPDATE or DELETE affects all rows.
    
- `RETURNING` is a PostgreSQL feature that returns affected rows.
    
- DML changes records, not table structure.
    
- DML statements can be rolled back before committing a PostgreSQL transaction.