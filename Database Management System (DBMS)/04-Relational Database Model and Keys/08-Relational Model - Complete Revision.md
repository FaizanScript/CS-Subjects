
This is the final topic of Phase 4. We'll consolidate everything we've covered about the relational model, its components, keys, constraints and NULL behaviour.

The goal is to connect these concepts so you can understand how relational databases maintain structured and consistent data.



## **1. What is the Relational Model?**

The **Relational Model** is a database model in which data is organized into relations (tables), consisting of tuples (rows) and attributes (columns).

It was proposed by **Edgar F. Codd in 1970**.

Examples of relational database management systems:

- PostgreSQL
    
- MySQL
    
- Oracle Database
    
- Microsoft SQL Server
    
- SQLite

### **Core components**

|Component|Meaning|Example|
|---|---|---|
|Relation|Table|Student|
|Tuple|Row or record|One student's record|
|Attribute|Column|student_id|
|Domain|Permitted values for an attribute|Non-negative integers|
|Degree|Number of attributes|4 columns|
|Cardinality|Number of tuples|100 rows|
|Schema|Structure of a relation|Student(id, name, age)|
|Instance|Actual data at a given time|Stored student records|


## **2. Properties of a Relation**

In the classical relational model, a relation follows these properties:

1. Every relation has a unique name within its schema.
    
2. Every attribute has a unique name within its relation.
    
3. Each attribute contains atomic values.
    
4. Duplicate tuples are not allowed.
    
5. The order of tuples does not matter.
    
6. The order of attributes does not matter logically.
    
7. Each attribute's values must belong to its defined domain.

**Important distinction:** Classical relational theory treats a relation as a set of tuples. SQL tables can contain duplicate rows unless constraints prevent them.


## **3. Types of Keys**

A key is an attribute or combination of attributes used to identify tuples uniquely.

Let's use this Student relation:

|student_id|email|phone|name|
|---|---|---|---|
|101|a@gmail.com|9876543210|Rahul|
|102|b@gmail.com|9876543211|Aman|
|103|c@gmail.com|9876543212|Priya|

Assume `student_id`, `email` and `phone` are individually unique.

### **Key hierarchy**

			Super Key
   Any attribute set that uniquely identifies a tuple

		Minimal super keys

		   Candidate Keys
   Minimal sets that uniquely identify tuples

	One candidate key is selected
			Primary Key
		Selected candidate key
   Other candidate keys become Alternate Keys.

|Key|Definition|
|---|---|
|Super Key|Any attribute set that uniquely identifies a tuple|
|Candidate Key|Minimal super key|
|Primary Key|Candidate key selected to identify tuples|
|Alternate Key|Candidate key not selected as primary key|
|Foreign Key|Attribute(s) referencing a candidate key in another or the same relation|
|Composite Key|Key consisting of two or more attributes|

### **Important relationships between keys**

- Every candidate key is a super key.
    
- Every primary key is a candidate key.
    
- Every alternate key is a candidate key.
    
- A composite key can also be a candidate key, primary key or super key.
    
- A foreign key does not necessarily have to be a primary key in its own table.

### **Example: Composite Primary Key**

Consider:

**Enrollment**

|student_id|course_id|enrollment_date|
|---|---|---|
|101|501|2026-08-01|
|101|502|2026-08-02|
|102|501|2026-08-03|

Neither `student_id` nor `course_id` alone uniquely identifies a row.

But together, they do, assuming a student can enroll in a particular course only once.

```
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE,

    PRIMARY KEY (student_id, course_id)
);
```

This is a composite primary key.


## **4. Integrity Constraints**

**Integrity constraints** are rules applied to a database to maintain data accuracy, validity and consistency.

There are four major categories.

**Four major integrity constraints**

**1. Domain Constraint**
Attribute values must belong to their permitted domain.
`CHECK (age >= 0)`

**2. Key Constraint**
Candidate key values must uniquely identify tuples.
`UNIQUE (email)`

**3. Entity Integrity**
Primary key values cannot be NULL.
`PRIMARY KEY (student_id)`

**4. Referential Integrity**
A non-NULL foreign key value must reference an existing candidate key value.
`FOREIGN KEY (department_id) REFERENCES Department(id)`

### **A. Domain Constraint**

Controls which values an attribute can contain.

```
age INT CHECK (age >= 0 AND age <= 120)
```

This prevents negative ages and ages greater than 120.

### **B. Key Constraint**

Ensures uniqueness.

```
email VARCHAR(100) UNIQUE
```

This prevents duplicate non-NULL email values under PostgreSQL's default UNIQUE behaviour.

### **C. Entity Integrity**

A primary key must be unique and non-NULL.

```
student_id INT PRIMARY KEY
```

### **D. Referential Integrity**

A foreign key must reference an existing candidate key value, unless it is NULL and NULL is allowed.

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

An employee cannot reference a department that does not exist.


## **5. Primary Key vs Foreign Key**

This is a particularly important interview and examination concept.

| Primary Key                                    | Foreign Key                                                |
| ---------------------------------------------- | ---------------------------------------------------------- |
| Uniquely identifies tuples in its own relation | References a candidate key in another or the same relation |
| Must be unique                                 | Duplicates are generally allowed                           |
| Cannot contain NULL                            | Can contain NULL unless prohibited                         |
| One primary key constraint per table           | Multiple foreign key constraints allowed                   |
| Enforces entity integrity                      | Enforces referential integrity                             |

Example:

**Department**

|department_id (PK)|name|
|---|---|
|1|IT|
|2|HR|

**Employee**

|employee_id (PK)|name|department_id (FK)|
|---|---|---|
|101|Rahul|1|
|102|Aman|1|
|103|Priya|2|
|104|Rohan|NULL|

Multiple employees can reference the same department. Rohan has no department assigned.

Notice that `department_id` is unique in Department, but it can repeat in Employee.


## **6. NULL Values and Three-Valued Logic**

NULL represents missing, unknown or inapplicable information.

It is not equivalent to:

- Zero (`0`)
    
- Empty string (`''`)
    
- FALSE

SQL uses three-valued logic:

|Result|Meaning|
|---|---|
|TRUE|Condition is satisfied|
|FALSE|Condition is not satisfied|
|UNKNOWN|Cannot determine the result|

For example:

```
SELECT *
FROM Student
WHERE marks = NULL;
```

This is incorrect.

Use:

```
SELECT *
FROM Student
WHERE marks IS NULL;
```

### **Aggregate function behaviour**

|Function|NULL behaviour|
|---|---|
|`COUNT(*)`|Counts all rows|
|`COUNT(column)`|Ignores NULL|
|`SUM(column)`|Ignores NULL|
|`AVG(column)`|Ignores NULL|
|`MIN(column)`|Ignores NULL|
|`MAX(column)`|Ignores NULL|

Remember: `WHERE` returns only rows for which the condition evaluates to TRUE. FALSE and UNKNOWN are filtered out.


## **7. Relational Schema and Relational Instance**

|Relational Schema|Relational Instance|
|---|---|
|Blueprint or structure|Actual data|
|Defines relation and attributes|Contains tuples|
|Relatively stable|Changes frequently|
|Example: `Student(id, name, age)`|Actual student records|

### **Degree and Cardinality**

- Degree = Number of columns.
    
- Cardinality = Number of rows.

Example: A table with 5 columns and 100 rows has degree 5 and cardinality 100.

SQL operations:

|Operation|Effect|
|---|---|
|INSERT|Changes instance|
|UPDATE|Changes instance|
|DELETE|Changes instance|
|ALTER TABLE|Changes schema|
|DROP TABLE|Removes the relation and its schema|


## **8. Putting Everything Together: A Practical Database**

Let's create a small college database using PostgreSQL.

This example combines keys, domain constraints, entity integrity and referential integrity.

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 16 AND age <= 100),
    department_id INT,

    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);

CREATE TABLE Course (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    credits INT CHECK (credits > 0)
);

CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE DEFAULT CURRENT_DATE,

    PRIMARY KEY (student_id, course_id),

    FOREIGN KEY (student_id)
        REFERENCES Student(student_id),

    FOREIGN KEY (course_id)
        REFERENCES Course(course_id)
);
```

### **What constraints are being applied?**

|Table|Constraint|Purpose|
|---|---|---|
|Department|Primary Key|Unique department identification|
|Department|UNIQUE|No duplicate department names|
|Student|Primary Key|Unique student identification|
|Student|NOT NULL|Student name is mandatory|
|Student|UNIQUE|Email uniqueness|
|Student|CHECK|Age must be within the allowed range|
|Student|Foreign Key|Department must exist|
|Course|CHECK|Credits must be positive|
|Enrollment|Composite Primary Key|Student-course combination must be unique|
|Enrollment|Foreign Keys|Referenced student and course must exist|

This is how relational database concepts work together in a real application.


## **9. Phase 4 — Complete Revision Cheat Sheet**

|Concept|Remember|
|---|---|
|Relational Model|Data organized into relations|
|Relation|Table|
|Tuple|Row|
|Attribute|Column|
|Domain|Permitted attribute values|
|Degree|Number of columns|
|Cardinality|Number of rows|
|Super Key|Unique, may contain unnecessary attributes|
|Candidate Key|Minimal super key|
|Primary Key|Selected candidate key|
|Alternate Key|Unselected candidate key|
|Foreign Key|References a candidate key|
|Composite Key|Contains multiple attributes|
|Domain Constraint|Restricts permitted values|
|Key Constraint|Enforces uniqueness|
|Entity Integrity|PK cannot be NULL|
|Referential Integrity|FK must reference an existing key or be NULL if allowed|
|NULL|Missing, unknown or inapplicable value|
|Three-Valued Logic|TRUE, FALSE, UNKNOWN|
|Relational Schema|Structure|
|Relational Instance|Actual data|