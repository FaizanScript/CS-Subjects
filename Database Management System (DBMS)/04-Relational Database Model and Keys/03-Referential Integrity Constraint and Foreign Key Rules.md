
In the previous topics, we learned that a primary key uniquely identifies records and a foreign key establishes relationships between tables.

Now we'll understand the rules that govern foreign keys and how databases maintain consistency between related tables.



## **1. What is Referential Integrity?**

**Referential Integrity is a constraint that ensures every non-NULL foreign key value references an existing candidate key value in the referenced table.**

Let's understand this through an example.

Suppose we have two tables:

**Department**

|department_id (PK)|department_name|
|---|---|
|10|IT|
|20|HR|
|30|Finance|

**Employee**

|employee_id (PK)|name|department_id (FK)|
|---|---|---|
|101|Faizan|10|
|102|Rahul|20|
|103|Aman|30|

Here:

- `department_id` in Department is the primary key.
    
- `department_id` in Employee is the foreign key.
    
- The Employee table references the Department table.

Now imagine inserting:

|employee_id|name|department_id|
|---|---|---|
|104|Arjun|50|

Department 50 doesn't exist.

The database rejects this insertion because it violates referential integrity.

**The fundamental rule: A foreign key cannot reference a nonexistent record.**


## **2. How Does a Foreign Key Work?**

Let's examine the SQL implementation.

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

The important part is:

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
```

This tells the DBMS:

> Every non-NULL department_id in Employee must match an existing department_id in Department.

A foreign key generally references a primary key or another candidate key protected by a suitable uniqueness constraint.


## **3. Can a Foreign Key Contain NULL?**

**Yes. A foreign key can contain NULL unless the column is explicitly restricted using NOT NULL.**

Consider:

|employee_id|name|department_id|
|---|---|---|
|101|Faizan|10|
|102|Rahul|20|
|103|Aman|NULL|

What does NULL mean here?

It means the employee currently has no department assigned, or the department information is unknown.

This does not violate referential integrity because there is no non-NULL foreign key value pointing to a nonexistent department.

However, if the business requirement says every employee must belong to a department, we should use `NOT NULL`.

```
CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT NOT NULL,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

Now NULL is prohibited.

**Remember:**

- Primary key → NULL is never allowed.
    
- Foreign key → NULL is allowed unless NOT NULL is specified.


## **4. What Happens When We Delete a Referenced Record?**

This is an important real-world database problem.

Suppose Department 10 has three employees:

|employee_id|name|department_id|
|---|---|---|
|101|Faizan|10|
|102|Rahul|10|
|103|Aman|10|

Now someone tries to delete Department 10.

```
DELETE FROM Department
WHERE department_id = 10;
```

What should happen to the employees?

The DBMS needs a defined rule to handle this situation.

SQL provides several referential actions.

### **A. ON DELETE RESTRICT**

Prevents deletion of a referenced parent record while dependent records exist.

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
ON DELETE RESTRICT
```

**Result:** Department 10 cannot be deleted while employees reference it.

### **B. ON DELETE CASCADE**

Automatically deletes dependent records when the referenced parent record is deleted.

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
ON DELETE CASCADE
```

**Result:** Deleting Department 10 also deletes all employees belonging to Department 10.

Use carefully

This can cause substantial data loss if used incorrectly.

### **C. ON DELETE SET NULL**

Sets the foreign key to NULL when the referenced parent record is deleted.

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
ON DELETE SET NULL
```

**Result:** Employees remain, but their `department_id` becomes NULL.

The FK column must allow NULL.

### **D. ON DELETE SET DEFAULT**

Sets the foreign key to its default value when the referenced record is deleted.

```
CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT DEFAULT 0,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
        ON DELETE SET DEFAULT
);
```

**Important:** Department ID `0` must exist in the referenced table if the operation is to succeed.

### **Comparison**

|Action|What happens to child records?|
|---|---|
|RESTRICT|Parent deletion is prevented|
|NO ACTION|Parent deletion is rejected if the constraint remains violated|
|CASCADE|Child records are deleted|
|SET NULL|Child FK values become NULL|
|SET DEFAULT|Child FK values become their default|

**RESTRICT vs NO ACTION:** They are similar in common immediate-constraint cases. The distinction becomes important with deferred constraints: `NO ACTION` can allow the violation to be resolved before constraint checking, whereas `RESTRICT` does not defer the check.


## **5. What About Updating a Referenced Primary Key?**

So far, we've discussed deletion.

But what if someone changes a referenced department ID?

For example:

Department:

|department_id|department_name|
|---|---|
|10|IT|

Employee:

|employee_id|name|department_id|
|---|---|---|
|101|Faizan|10|
|102|Rahul|10|

Now we execute:

```
UPDATE Department
SET department_id = 50
WHERE department_id = 10;
```

What happens to the employees?

SQL provides `ON UPDATE` actions.

### **A. ON UPDATE CASCADE**

Automatically updates the foreign keys in referencing rows.

```
FOREIGN KEY (department_id)
REFERENCES Department(department_id)
ON UPDATE CASCADE
```

Result:

Department:

|department_id|department_name|
|---|---|
|50|IT|

Employee:

|employee_id|name|department_id|
|---|---|---|
|101|Faizan|50|
|102|Rahul|50|

### **B. ON UPDATE RESTRICT / NO ACTION**

Prevents the referenced key from being changed while dependent references exist, subject to the timing distinction discussed earlier.

### **C. ON UPDATE SET NULL**

Sets referencing foreign-key values to NULL when the referenced key changes.

### **D. ON UPDATE SET DEFAULT**

Sets referencing foreign-key values to their default values.

In practice, primary keys should generally be stable. Updating them frequently is usually a poor design choice, even though SQL supports these actions.

## **6. Composite Foreign Keys**

A foreign key does not have to consist of just one column. It can contain multiple columns.

Consider:

**CourseOffering**

|course_id (PK part)|semester (PK part)|instructor|
|---|---|---|
|101|Fall-2026|Amit|
|102|Spring-2027|Rahul|

Its primary key is:

`(course_id, semester)`

Now suppose another table references a particular course offering.

**Enrollment**

|student_id|course_id|semester|
|---|---|---|
|1|101|Fall-2026|
|2|102|Spring-2027|

We can define a composite foreign key:

```
CREATE TABLE CourseOffering (
    course_id INT,
    semester VARCHAR(30),
    instructor VARCHAR(100),

    PRIMARY KEY (course_id, semester)
);

CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    semester VARCHAR(30),

    FOREIGN KEY (course_id, semester)
        REFERENCES CourseOffering(course_id, semester)
);
```

Here, the combination `(course_id, semester)` must match an existing course offering.

The referenced columns must form a suitable unique key.

## **7. Complete Practical Example**

Let's put the concepts together.

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
        ON DELETE SET NULL
        ON UPDATE CASCADE
);
```

Let's interpret the rules:

1. Every employee has a unique employee ID.
    
2. An employee's department must exist if a department ID is provided.
    
3. An employee may have NULL as their department ID.
    
4. If a department is deleted, its employees remain, but their department IDs become NULL.
    
5. If a department ID changes, the corresponding employee references are updated automatically.

This is how referential integrity protects relationships across tables.

## **8. Quick Revision**

|Concept|Key point|
|---|---|
|Referential Integrity|FK references an existing candidate key value or is NULL|
|NULL foreign key|Allowed unless NOT NULL is specified|
|ON DELETE CASCADE|Deletes referencing rows|
|ON DELETE SET NULL|Sets referencing FK values to NULL|
|ON UPDATE CASCADE|Updates referencing FK values|
|Composite FK|Multiple columns together reference a unique key|
