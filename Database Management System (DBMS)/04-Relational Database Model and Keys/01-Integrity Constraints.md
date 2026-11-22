

## **1. What are Integrity Constraints?**

**Integrity Constraints are rules imposed on a database to ensure that the data stored in it remains accurate, valid, consistent, and reliable.**

In simple terms, they prevent invalid data from entering your database.

Imagine we're building a college management system.

Our Student table looks like this:

|student_id|name|age|email|
|---|---|---|---|
|101|Faizan|20|faizan@example.com|
|102|Rahul|19|rahul@example.com|

Now imagine the database allows these records:

|student_id|name|age|email|
|---|---|---|---|
|101|Faizan|20|faizan@example.com|
|101|Aman|21|aman@example.com|
|NULL|Rahul|-5|rahul@example.com|

We have multiple problems:

- Duplicate `student_id`: two students share the same ID.
    
- NULL `student_id`: a student has no identity.
    
- Negative age: invalid according to our assumed business rules.

A database should prevent such invalid records.

That's exactly why integrity constraints exist.


## **2. Types of Integrity Constraints**

There are four important categories to understand:

**1. Domain Constraint**:

Ensures attribute values belong to an allowed domain.

Example: age must be a non-negative integer.

**2. Key Constraint**:

Ensures candidate key values uniquely identify tuples.

Example: two students cannot share the same student ID.

**3. Entity Integrity Constraint**:

Ensures a primary key cannot contain NULL values.

Example: every student must have a valid student ID.

**4. Referential Integrity Constraint**:

Ensures a foreign key references an existing valid record in the referenced table, subject to NULL rules.

Example: an employee cannot reference a department that doesn't exist.

We'll study each separately, including how SQL implements them.


## **3. Domain Constraint**

**Definition:** A domain constraint ensures that every attribute value belongs to the set of permitted values defined for that attribute.

A domain defines what values an attribute is allowed to contain.

For example:

|Attribute|Allowed domain|
|---|---|
|age|Integer between 0 and 120|
|gender|Values permitted by the application's rules|
|email|Valid email format, if enforced|
|salary|Non-negative numeric values|
|date_of_birth|Valid date values|

In PostgreSQL, we can enforce some of these rules using data types and `CHECK` constraints.

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT CHECK (age >= 0)
);
```

Here:

- `INT` restricts age to integer values.
    
- `CHECK (age >= 0)` prevents negative ages.

We can also restrict a column to specific values:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    status VARCHAR(20)
        CHECK (status IN ('active', 'inactive'))
);
```

Now the `status` column can contain only `active` or `inactive`, apart from NULL because a CHECK condition that evaluates to UNKNOWN does not cause rejection.

**Important distinction:** A data type restricts the kind of value, while a CHECK constraint can restrict the actual permitted values further.


## **4. Key Constraint**

**Definition:** A key constraint ensures that values of a candidate key uniquely identify tuples (rows) within a relation.

Consider:

|student_id|email|name|
|---|---|---|
|101|faizan@example.com|Faizan|
|102|rahul@example.com|Rahul|
|103|aman@example.com|Aman|

Here, both `student_id` and `email` could uniquely identify students.

If we try inserting another student with `student_id = 101`, the database should reject it.

In SQL:

```
CREATE TABLE Student (
    student_id INT UNIQUE,
    email VARCHAR(100) UNIQUE,
    name VARCHAR(100)
);
```

Both columns have `UNIQUE` constraints.

A `UNIQUE` constraint enforces uniqueness, but its NULL behaviour varies across database systems. PostgreSQL, for example, allows multiple NULLs by default in a UNIQUE column.

We'll examine the distinction between `UNIQUE` and `PRIMARY KEY` in a later topic.


## **5. Entity Integrity Constraint**

**Definition:** Entity integrity states that no attribute participating in a relation's primary key can contain a NULL value.

Why?

Because a primary key identifies each tuple.

If a student's primary key is NULL, how can we uniquely identify that student?

Example:

|student_id (PK)|name|
|---|---|
|101|Faizan|
|102|Rahul|
|NULL|Aman|

The third row violates entity integrity.

In SQL:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

A primary key automatically enforces:

1. Uniqueness.
    
2. NOT NULL.

This also applies to composite primary keys: none of their participating columns can be NULL.


## **6. Referential Integrity Constraint**

**Definition:** Referential integrity ensures that a foreign key value in one relation matches an existing referenced candidate key value in the referenced relation, or is NULL where NULL is permitted.

Let's understand this through an example.

We have two tables:

**Department**

|department_id (PK)|department_name|
|---|---|
|10|IT|
|20|HR|
|30|Finance|

**Employee**

|employee_id (PK)|name|department_id (FK)|
|---|---|---|
|1|Faizan|10|
|2|Rahul|20|
|3|Aman|30|

The `department_id` in Employee references the `department_id` in Department.

Now suppose we insert:

|employee_id|name|department_id|
|---|---|---|
|4|Arjun|50|

Department 50 doesn't exist.

This violates referential integrity.

SQL implementation:

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

The database will reject an employee referencing a nonexistent department.

### **What happens when a referenced record is deleted?**

Suppose Department 10 has employees referencing it.

What happens if we delete Department 10?

SQL supports different foreign-key actions:

|Action|Behaviour|
|---|---|
|RESTRICT|Prevents deletion while references exist|
|NO ACTION|Rejects the operation if the constraint is violated|
|CASCADE|Deletes referencing rows automatically|
|SET NULL|Sets referencing FK values to NULL|
|SET DEFAULT|Sets referencing FK values to their default|

Example:

```
CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
        ON DELETE SET NULL
);
```

If a department is deleted, the employees referencing it remain, but their `department_id` becomes NULL.

This requires the FK column to allow NULL.


## **7. Quick Comparison**

|Constraint|What does it protect?|
|---|---|
|Domain|Valid attribute values|
|Key|Uniqueness|
|Entity Integrity|Primary key cannot be NULL|
|Referential Integrity|Valid foreign-key references|

**Remember these four distinctions:**

- Domain → Is this value allowed?
    
- Key → Is this value unique?
    
- Entity Integrity → Does every row have a primary key?
    
- Referential Integrity → Does the referenced record exist?