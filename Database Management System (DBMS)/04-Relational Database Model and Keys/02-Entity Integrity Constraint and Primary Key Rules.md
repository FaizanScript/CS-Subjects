
In the previous topic, we introduced Entity Integrity. Now we'll understand exactly how primary keys work, why NULL is prohibited, and how these rules apply to composite primary keys.



## **1. What is Entity Integrity?**

**Entity Integrity is a relational model constraint stating that no component of a primary key can contain a NULL value.**

The reason is straightforward: a primary key exists to uniquely identify every row in a table.

Consider this Student table:

|student_id (PK)|name|
|---|---|
|101|Faizan|
|102|Rahul|
|NULL|Aman|

The third row violates Entity Integrity.

Why?

Because we cannot identify that student's record using its primary key.

A primary key must satisfy two fundamental properties:

1. **Uniqueness:** No two rows can have the same primary key value.
    
2. **Non-NULL:** No primary key value can be NULL.

These two properties work together to guarantee identification.


## **2. Why Can't a Primary Key Be NULL?**

Let's understand this concept carefully.

Suppose we have:

|student_id|name|
|---|---|
|101|Faizan|
|102|Rahul|
|NULL|Aman|

What does NULL mean?

NULL represents a missing, unknown, or unavailable value. It is not the same as zero, an empty string, or a regular value.

If `student_id` is NULL, we don't know the student's identifier.

There is another important issue: SQL uses three-valued logic.

For example:

```
NULL = NULL
```

This does not evaluate to TRUE. It evaluates to UNKNOWN.

Therefore, NULL cannot reliably serve as a unique identifier.

That's why primary keys prohibit NULL values.


## **3. Primary Key Rules**

Let's establish the rules clearly.

|Rule|Explanation|
|---|---|
|Unique|Every row must have a different PK value|
|NOT NULL|No PK component can contain NULL|
|One PK constraint|A table can have only one primary-key constraint|
|Can be composite|A PK can consist of multiple columns|
|Stable identity|PK should ideally not change frequently|

One important clarification:

A table can have multiple candidate keys, but only one of them is selected as the primary key.

For example:

|student_id|email|name|
|---|---|---|
|101|faizan@example.com|Faizan|
|102|rahul@example.com|Rahul|

Both `student_id` and `email` could uniquely identify students.

We might select `student_id` as the primary key and keep `email` as an alternate key.


## **4. Composite Primary Key**

A **Composite Primary Key** is a primary key consisting of two or more attributes that collectively identify a row uniquely.

Consider an Enrollment table.

A student can enroll in multiple courses, and each course can have multiple students.

|student_id|course_id|enrollment_date|
|---|---|---|
|101|1|2026-11-01|
|101|2|2026-11-02|
|102|1|2026-11-03|

Here:

- `student_id` alone is not unique.
    
- `course_id` alone is not unique.
    
- But `(student_id, course_id)` together uniquely identify each enrollment, assuming a student can enroll in a particular course only once.

Therefore:

			PK = (student_id, course_id)

SQL:

```
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE,

    PRIMARY KEY (student_id, course_id)
);
```


### **Important: Entity Integrity applies to every component**

Consider this record:

|student_id|course_id|
|---|---|
|101|1|
|102|2|
|NULL|3|

The third record violates Entity Integrity because `student_id` is part of the composite primary key.

Similarly, this is invalid:

|student_id|course_id|
|---|---|
|101|NULL|

**Every attribute participating in a primary key must be non-NULL.**


## **5. Can a Table Have Multiple Primary Keys?**

No.

A relational table can have only one primary-key constraint.

However, that primary key can contain multiple columns.

For example:

```
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    semester VARCHAR(20),

    PRIMARY KEY (student_id, course_id, semester)
);
```

This is still just **one primary key**, consisting of three attributes.

You might also hear about:

- Single-column primary key
    
- Composite primary key

These describe the number of attributes inside the primary key, not the number of primary keys.


## **6. Primary Key vs UNIQUE Constraint**

This distinction is important in SQL.

|Primary Key|UNIQUE|
|---|---|
|Enforces uniqueness|Enforces uniqueness|
|Does not allow NULL|NULL behaviour depends on DBMS|
|Only one PK constraint per table|Multiple UNIQUE constraints allowed|
|Identifies rows as the table's primary identifier|Enforces alternate uniqueness rules|

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    phone_number VARCHAR(15) UNIQUE
);
```

Here:

- `student_id` is the primary key.
    
- `email` and `phone_number` have UNIQUE constraints.
    
- Multiple UNIQUE constraints are allowed.

In PostgreSQL, a regular UNIQUE constraint allows multiple NULL values by default. A primary key never allows NULL.


## **7. A Practical Design Question**

Suppose you're building a college management system.

You have these attributes:

- student_id
    
- email
    
- phone_number
    
- name

Your requirements:

1. Every student must have a unique student ID.
    
2. Every student must have a unique email.
    
3. Phone number is optional.
    
4. Every student must have a name.

A possible implementation:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone_number VARCHAR(15) UNIQUE,
    name VARCHAR(100) NOT NULL
);
```

Notice:

- `student_id`: automatically UNIQUE and NOT NULL.
    
- `email`: NOT NULL and UNIQUE.
    
- `phone_number`: UNIQUE but nullable.
    
- `name`: NOT NULL but not necessarily UNIQUE.

This is how theoretical integrity rules translate into actual database constraints.


## **8. Quick Revision**

- Entity Integrity prohibits NULL in primary-key attributes.
    
- A primary key must be unique and non-NULL.
    
- A table has only one primary-key constraint.
    
- A primary key can consist of multiple attributes.
    
- Every component of a composite primary key must be non-NULL.
    
- UNIQUE constraints can enforce uniqueness for additional attributes.