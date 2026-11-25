
So far, we've learned how to create tables, insert records, update data, retrieve records, and filter results.

But there's an important problem we need to solve.

**How do we prevent invalid data from entering our database?**

Imagine a college database where:

- Two students accidentally receive the same student ID.
    
- A student's age is entered as `-5`.
    
- An employee is assigned to a department that doesn't exist.
    
- A student's name is left empty even though it's mandatory.
    

These are data integrity problems.

SQL constraints help prevent them.



### **1. What are SQL Constraints?**

**SQL Constraints are rules applied to database columns or tables to restrict the type of data that can be inserted, updated, or stored.**

Their primary purpose is to maintain data accuracy, validity and integrity.

For example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INT CHECK (age >= 16),
    email VARCHAR(150) UNIQUE
);
```

Here:

- `PRIMARY KEY` prevents duplicate and NULL student IDs.
    
- `NOT NULL` ensures that the student's name is provided.
    
- `CHECK` ensures that age is at least 16.
    
- `UNIQUE` prevents duplicate non-NULL email values under PostgreSQL's default behavior.


### **2. Types of SQL Constraints**

The main constraints we'll study are:

|Constraint|Purpose|
|---|---|
|`NOT NULL`|Prevents NULL values|
|`UNIQUE`|Prevents duplicate values|
|`PRIMARY KEY`|Uniquely identifies each row|
|`FOREIGN KEY`|Maintains relationships between tables|
|`CHECK`|Enforces a condition|
|`DEFAULT`|Supplies a default value when applicable|

We've already encountered most of these in earlier DBMS topics. Now let's understand their SQL implementation in detail.


### **3. NOT NULL Constraint**

The `NOT NULL` constraint ensures that a column cannot contain NULL values.

**Syntax:**

```
column_name datatype NOT NULL
```

Example:

```
CREATE TABLE Student (
    student_id INT,
    name VARCHAR(100) NOT NULL,
    age INT
);
```

Here, `name` cannot be NULL.

This insertion will fail:

```
INSERT INTO Student (student_id, name, age)
VALUES (101, NULL, 20);
```

But this is valid:

```
INSERT INTO Student (student_id, name, age)
VALUES (101, 'Faizan', 20);
```

**Important distinction: NULL vs Empty String**

Consider:

```
INSERT INTO Student (student_id, name, age)
VALUES (102, '', 20);
```

This is allowed by `NOT NULL` because an empty string (`''`) is not NULL.

If your application also needs to prevent empty or whitespace-only names, you can use:

```
name VARCHAR(100) NOT NULL
    CHECK (length(trim(name)) > 0)
```

This rejects both empty strings and strings containing only whitespace.


### **4. UNIQUE Constraint**

The `UNIQUE` constraint ensures that values in a column, or combination of columns, are unique.

Example:

```
CREATE TABLE Student (
    student_id INT,
    email VARCHAR(150) UNIQUE
);
```

Now, two students cannot have the same non-NULL email address.

For example:

```
INSERT INTO Student VALUES (101, 'faizan@example.com');
```

This works.

But:

```
INSERT INTO Student VALUES (102, 'faizan@example.com');
```

This fails because that email already exists.

**A. UNIQUE on multiple columns**

You can also enforce uniqueness on a combination of columns.

```
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    semester INT,

    UNIQUE (student_id, course_id, semester)
);
```

This prevents duplicate combinations of student, course and semester.

For example:

|student_id|course_id|semester|Valid?|
|---|---|---|---|
|101|201|1|Yes|
|101|201|1|No|
|101|201|2|Yes|
|101|202|1|Yes|

**B. UNIQUE vs PRIMARY KEY**

|PRIMARY KEY|UNIQUE|
|---|---|
|Uniqueness enforced|Uniqueness enforced|
|NULL not allowed|NULL generally allowed|
|One PK constraint per table|Multiple UNIQUE constraints allowed|
|Identifies each row|Enforces uniqueness on selected columns|

PostgreSQL-specific detail: A normal `UNIQUE` constraint allows multiple NULL values by default because NULL values are treated as distinct for uniqueness. PostgreSQL also supports `UNIQUE NULLS NOT DISTINCT` when NULL values should be treated as equal for uniqueness.


### **5. PRIMARY KEY Constraint**

A `PRIMARY KEY` uniquely identifies each row in a table.

It combines two important properties:

1. Every primary key value must be unique.
    
2. Every primary key value must be non-NULL.

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
```

This is valid:

```
INSERT INTO Student VALUES (101, 'Faizan', 20);
INSERT INTO Student VALUES (102, 'Aman', 21);
```

This fails because the ID is duplicated:

```
INSERT INTO Student VALUES (101, 'Rahul', 19);
```

This fails because the primary key cannot be NULL:

```
INSERT INTO Student VALUES (NULL, 'Rahul', 19);
```

**A. Composite Primary Key**

A primary key can contain multiple columns.

```
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE,

    PRIMARY KEY (student_id, course_id)
);
```

Here, neither `student_id` nor `course_id` is necessarily unique individually.

But their combination must be unique.

For example:

|student_id|course_id|Valid?|
|---|---|---|
|101|201|Yes|
|101|202|Yes|
|102|201|Yes|
|101|201|No|

The last row violates the composite primary key.

**Important:** Every component of a composite primary key must be non-NULL.


### **6. FOREIGN KEY Constraint**

A `FOREIGN KEY` establishes a relationship between two tables and maintains referential integrity.

Let's create two tables.

**Department**

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);
```

**Student**

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

Here:

- `Department.department_id` is the primary key.
    
- `Student.department_id` is the foreign key.
    
- The foreign key references the department table.

Suppose the Department table contains:

|department_id|department_name|
|---|---|
|1|Computer Science|
|2|Information Technology|
|3|Electronics|

Now this insertion is valid:

```
INSERT INTO Student
VALUES (101, 'Faizan', 1);
```

Why?

Because department ID `1` exists in the Department table.

But this insertion fails:

```
INSERT INTO Student
VALUES (102, 'Aman', 99);
```

Why?

Because department ID `99` doesn't exist in the referenced table.

**Important:** A foreign key can contain NULL unless the column is also declared `NOT NULL`.

For example, if a student hasn't been assigned a department yet, NULL can be permitted depending on the business rules.

#### **A. ON DELETE Actions**

What happens when someone tries to delete a department that students are referencing?

SQL provides different actions.

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
ON DELETE CASCADE
```

Common options:

|Action|Behaviour|
|---|---|
|`RESTRICT`|Prevents deletion while references exist|
|`NO ACTION`|Rejects the operation if the constraint is violated at check time|
|`CASCADE`|Deletes referencing rows|
|`SET NULL`|Sets referencing foreign keys to NULL|
|`SET DEFAULT`|Sets referencing foreign keys to their default value|

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
        ON DELETE SET NULL
);
```

If a department is deleted, the associated students remain, but their `department_id` becomes NULL.

**Be careful with CASCADE.** Deleting a parent record can automatically delete many dependent records.

#### **B. ON UPDATE Actions**

You can also define what happens when a referenced key changes.

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
ON UPDATE CASCADE
```

If a department ID changes, PostgreSQL automatically updates the corresponding foreign key values in Student.


### **7. CHECK Constraint**

The `CHECK` constraint ensures that data satisfies a specified condition.

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT CHECK (age >= 16)
);
```

Now:

```
INSERT INTO Student
VALUES (101, 'Faizan', 20);
```

Valid.

But:

```
INSERT INTO Student
VALUES (102, 'Aman', 12);
```

Invalid because age is below 16.

**A. CHECK with BETWEEN**

```
age INT CHECK (age BETWEEN 16 AND 100)
```

Age must be between 16 and 100, inclusive.

**B. CHECK with IN**

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),

    status VARCHAR(20)
        CHECK (status IN ('active', 'inactive', 'graduated'))
);
```

Only the three specified status values are accepted when status is non-NULL.

**C. CHECK using multiple columns**

Suppose we have an Order table.

```
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    quantity INT,
    price NUMERIC(10,2),
    discount NUMERIC(10,2),

    CHECK (quantity > 0),
    CHECK (price >= 0),
    CHECK (discount >= 0 AND discount <= quantity * price)
);
```

Here, the discount cannot exceed the total price.

**Important NULL behaviour in PostgreSQL:**

A CHECK constraint rejects a row only when its condition evaluates to FALSE. TRUE and UNKNOWN are accepted.

For example:

```
age INT CHECK (age >= 18)
```

This alone permits NULL because `NULL >= 18` evaluates to UNKNOWN.

If age is mandatory, write:

```
age INT NOT NULL CHECK (age >= 18)
```

This combines both rules.


### **8. DEFAULT Constraint**

The `DEFAULT` constraint supplies a predefined value when an INSERT statement omits a column or explicitly requests `DEFAULT`.

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    city VARCHAR(50) DEFAULT 'Hazaribagh'
);
```

Now:

```
INSERT INTO Student (student_id, name)
VALUES (101, 'Faizan');
```

Result:

|student_id|name|city|
|---|---|---|
|101|Faizan|Hazaribagh|

The city was automatically assigned the default value.

You can also explicitly request it:

```
INSERT INTO Student
VALUES (102, 'Aman', DEFAULT);
```

**Important:** DEFAULT does not mean NOT NULL.

For example:

```
INSERT INTO Student
VALUES (103, 'Rahul', NULL);
```

This explicitly inserts NULL. The default is not used.


### **9. Naming Constraints**

You can give constraints meaningful names using `CONSTRAINT`.

Why?

Because named constraints are easier to identify, modify and remove.

Example:

```
CREATE TABLE Student (
    student_id INT,

    name VARCHAR(100) NOT NULL,

    age INT,

    CONSTRAINT pk_student
        PRIMARY KEY (student_id),

    CONSTRAINT chk_student_age
        CHECK (age >= 16)
);
```

Here:

- `pk_student` is the primary key constraint name.
    
- `chk_student_age` is the CHECK constraint name.

You can name UNIQUE and FOREIGN KEY constraints too.

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    email VARCHAR(150),

    CONSTRAINT uq_student_email
        UNIQUE (email)
);
```


### **10. Adding Constraints to Existing Tables**

Sometimes a table already exists, and you want to add a constraint later.

Use `ALTER TABLE`.

**A. Add UNIQUE**

```
ALTER TABLE Student
ADD CONSTRAINT uq_student_email
UNIQUE (email);
```

**B. Add CHECK**

```
ALTER TABLE Student
ADD CONSTRAINT chk_student_age
CHECK (age >= 16);
```

**C. Add FOREIGN KEY**

```
ALTER TABLE Student
ADD CONSTRAINT fk_student_department
FOREIGN KEY (department_id)
REFERENCES Department(department_id);
```

**D. Add PRIMARY KEY**

```
ALTER TABLE Student
ADD CONSTRAINT pk_student
PRIMARY KEY (student_id);
```

This will fail if existing data violates the new primary key rules.

**E. Add NOT NULL**

In PostgreSQL, NOT NULL is added using `ALTER COLUMN`:

```
ALTER TABLE Student
ALTER COLUMN name SET NOT NULL;
```

Remove it using:

```
ALTER TABLE Student
ALTER COLUMN name DROP NOT NULL;
```


### **11. Removing Constraints**

Use `ALTER TABLE ... DROP CONSTRAINT`.

Example:

```
ALTER TABLE Student
DROP CONSTRAINT chk_student_age;
```

This removes the named CHECK constraint.

Similarly:

```
ALTER TABLE Student
DROP CONSTRAINT uq_student_email;
```

This removes the UNIQUE constraint.

For a primary key:

```
ALTER TABLE Student
DROP CONSTRAINT pk_student;
```

For a foreign key:

```
ALTER TABLE Student
DROP CONSTRAINT fk_student_department;
```

PostgreSQL also allows `IF EXISTS`:

```
ALTER TABLE Student
DROP CONSTRAINT IF EXISTS chk_student_age;
```

**Remember:** Removing a constraint does not necessarily remove the underlying column. It removes the rule enforced by that constraint.


### **12. Complete Practical Example**

Let's build a small college database using the constraints we've learned.

**Department table**

```
CREATE TABLE Department (
    department_id INT,

    department_name VARCHAR(100) NOT NULL,

    CONSTRAINT pk_department
        PRIMARY KEY (department_id),

    CONSTRAINT uq_department_name
        UNIQUE (department_name)
);
```

**Student table**

```
CREATE TABLE Student (
    student_id INT,

    name VARCHAR(100) NOT NULL,

    email VARCHAR(150) NOT NULL,

    age INT NOT NULL,

    department_id INT,

    status VARCHAR(20) DEFAULT 'active',

    CONSTRAINT pk_student
        PRIMARY KEY (student_id),

    CONSTRAINT uq_student_email
        UNIQUE (email),

    CONSTRAINT chk_student_age
        CHECK (age BETWEEN 16 AND 100),

    CONSTRAINT chk_student_status
        CHECK (status IN ('active', 'inactive', 'graduated')),

    CONSTRAINT fk_student_department
        FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
        ON DELETE SET NULL
);
```

This schema protects the database against several types of invalid data.

|Rule|Protection|
|---|---|
|`pk_student`|Duplicate or NULL student IDs|
|`uq_student_email`|Duplicate email addresses|
|`chk_student_age`|Invalid age range|
|`chk_student_status`|Invalid student status|
|`fk_student_department`|References to nonexistent departments|
|`NOT NULL`|Missing mandatory values|
|`DEFAULT`|Automatically supplies default status|

This is how constraints help maintain database integrity at the database level, rather than relying entirely on your backend application to validate every input.


### **13. Quick Revision**

|Constraint|Main responsibility|
|---|---|
|NOT NULL|No NULL values|
|UNIQUE|No duplicate values under its uniqueness rules|
|PRIMARY KEY|Unique, non-NULL row identifier|
|FOREIGN KEY|Referential integrity|
|CHECK|Condition-based validation|
|DEFAULT|Supplies a default value|

**One important backend engineering principle:**

Validate data in your application, but enforce critical integrity rules in your database too.

Frontend validation can be bypassed. Backend validation can contain bugs. Database constraints provide another layer of protection.