

## **1. What is a Key?**

**A key is an attribute or a combination of attributes used to uniquely identify a tuple (row) in a relation (table).**

Consider this Student table:

|student_id|email|phone|name|
|---|---|---|---|
|101|faizan@example.com|9876543210|Faizan|
|102|rahul@example.com|9876543211|Rahul|
|103|aman@example.com|9876543212|Aman|

Suppose `student_id`, `email`, and `phone` are all unique.

Each of these attributes can independently identify a student.

But `name` cannot necessarily identify a student because two students can have the same name.

This is where different types of keys come into the picture.


## **2. Super Key**

**Definition:** A Super Key is any set of one or more attributes that uniquely identifies every tuple in a relation.

The important word is _any_.

A super key can contain unnecessary attributes as long as the complete set uniquely identifies every row.

Suppose our Student table has:

- `student_id`
    
- `email`
    
- `name`

Assume `student_id` and `email` are individually unique.

Then these are all super keys:

- `{student_id}`
    
- `{email}`
    
- `{student_id, name}`
    
- `{email, name}`
    
- `{student_id, email}`
    
- `{student_id, email, name}`

Why?

Because every set contains at least one attribute that already uniquely identifies a student.

For example:

`{student_id, name}`

Even if multiple students have the same name, the combination remains unique because `student_id` is unique.

**Key takeaway:** A super key may contain redundant attributes.


## **3. Candidate Key**

**Definition:** A Candidate Key is a minimal super key that uniquely identifies every tuple in a relation.

There are two requirements:

1. It must uniquely identify every row.
    
2. It must be minimal — removing any attribute from it must destroy its uniqueness.

Let's use the same example.

|student_id|email|name|
|---|---|---|
|101|faizan@example.com|Faizan|
|102|rahul@example.com|Rahul|
|103|aman@example.com|Aman|

Assuming both `student_id` and `email` are individually unique:

Candidate keys:

- `{student_id}`
    
- `{email}`

Why isn't `{student_id, name}` a candidate key?

Because `student_id` alone is already unique.

So `name` is unnecessary.

Therefore, `{student_id, name}` is a super key, but not a candidate key.

### **Super Key vs Candidate Key**

|Super Key|Candidate Key|
|---|---|
|Uniquely identifies rows|Uniquely identifies rows|
|Can contain extra attributes|Must be minimal|
|May contain unnecessary attributes|Cannot contain unnecessary attributes|
|Every candidate key is a super key|Every super key is not necessarily a candidate key|

**Relationship between them**

			Super Keys
   All attribute sets that uniquely identify rows

			Candidate Keys
		   Minimal super keys


## **4. Primary Key**

**Definition:** A Primary Key is the candidate key selected by the database designer to uniquely identify tuples in a relation.

Suppose we have two candidate keys:

- `student_id`
    
- `email`

We select `student_id` as the primary key.

Then:

- `student_id` → Primary Key
    
- `email` → Alternate Key

Important rules:

- A table can have only one primary-key constraint.
    
- A primary key can contain one or multiple attributes.
    
- It must be unique and NOT NULL.
    
- Every candidate key is eligible to be selected as the primary key.


## **5. Alternate Key**

**Definition:** An Alternate Key is a candidate key that was not selected as the primary key.

Example:

|student_id|email|name|
|---|---|---|
|101|faizan@example.com|Faizan|
|102|rahul@example.com|Rahul|

Suppose:

- `student_id` is selected as the primary key.
    
- `email` is another candidate key.

Then `email` becomes an alternate key.

In SQL:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    name VARCHAR(100)
);
```

The `UNIQUE` constraint on email enforces its uniqueness. If email is mandatory, we should also specify `NOT NULL`.


## **6. Foreign Key**

**Definition:** A Foreign Key is an attribute or combination of attributes in one relation that references a candidate key in another or the same relation.

Example:

**Department**

|department_id (PK)|department_name|
|---|---|
|10|IT|
|20|HR|

**Employee**

|employee_id (PK)|name|department_id (FK)|
|---|---|---|
|101|Faizan|10|
|102|Rahul|20|

Here:

- `Employee.employee_id` is the primary key.
    
- `Department.department_id` is the primary key.
    
- `Employee.department_id` is a foreign key.

It establishes a relationship between Employee and Department.

Remember: a foreign key can reference a primary key or another candidate key, provided the referenced columns have the required uniqueness.


## **7. Composite Key**

**Definition:** A Composite Key is a key consisting of two or more attributes that collectively identify a tuple uniquely.

Example:

**Enrollment**

|student_id|course_id|enrollment_date|
|---|---|---|
|101|1|2026-11-01|
|101|2|2026-11-02|
|102|1|2026-11-03|

Neither `student_id` nor `course_id` alone is unique.

But together:

			(student_id, course_id)

uniquely identifies each enrollment under the assumption that duplicate enrollment in the same course is not allowed.

This combination can be the primary key.

```
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE,

    PRIMARY KEY (student_id, course_id)
);
```

**Important:** Composite describes the number of attributes in a key. It doesn't mean a completely separate level of key like super key or candidate key.

A composite key can itself be a candidate key, a primary key, or a super key.


## **8. How Do We Identify Candidate Keys?**

This is where many students get confused.

Let's work through a more formal example.

Suppose a relation has:

			   R(A, B, C)

And we know:

- A is unique.
    
- B is unique.
    
- C is not unique.

Which are the candidate keys?

Step 1: Check uniqueness.

- `{A}` uniquely identifies every row.
    
- `{B}` uniquely identifies every row.
    
- `{C}` does not.

Step 2: Check minimality.

- `{A}` is minimal because it contains only A.
    
- `{B}` is minimal because it contains only B.

Therefore:

			Candidate Keys = {A}, {B}

Other super keys include:

- `{A, B}`
    
- `{A, C}`
    
- `{B, C}`
    
- `{A, B, C}`

These are not candidate keys because they contain unnecessary attributes.

### **A more challenging example**

Suppose we have:

			R(A, B, C)

And the following functional dependencies:

				A -> B

				B -> A

				A -> C

Let's calculate the candidate keys.

**Step 1: Find the attributes that determine all attributes.**

Since:

		A -> B, A -> c

A determines A, B and C.

Therefore:

		A^+ = {A,B,C}

So A is a candidate key.

Now consider B.

		B -> A

And because A determines C:

		B -> A -> c

Therefore:

		 B^+ = {A,B,C}

So B is also a candidate key.

**Final answer:**

		candidate Keys = {A}, {B}

We'll study Functional Dependencies and Attribute Closure in much greater depth during Normalization. For now, understand the principle: candidate keys must uniquely determine all attributes and be minimal.


## **9. Complete Comparison of Keys**

|Key|Main purpose|
|---|---|
|Super Key|Uniquely identifies tuples; may contain extra attributes|
|Candidate Key|Minimal super key|
|Primary Key|Selected candidate key|
|Alternate Key|Candidate key not selected as primary|
|Foreign Key|References a candidate key in another or same relation|
|Composite Key|Consists of multiple attributes|

One important distinction: these labels describe different aspects of keys. For example, a primary key can also be composite.