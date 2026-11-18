

## **1. What is a Relationship?**

A **Relationship** is an association between two or more entities that describes how they are connected to each other.

For example:

- A student enrolls in a course.
    
- A teacher teaches a course.
    
- An employee works in a department.
    
- A person owns a passport.

Here, `STUDENT`, `COURSE`, `TEACHER`, `EMPLOYEE`, and `DEPARTMENT` are entities.

`ENROLLS IN`, `TEACHES`, `WORKS IN`, and `OWNS` are relationships.


## **2. Degree of Relationship**

The **Degree of a Relationship** refers to the number of entity types participating in that relationship.

**There are three commonly discussed types:**

### **A. Unary Relationship (Recursive Relationship)**

A **Unary Relationship** is a relationship in which an entity is associated with another entity of the same entity type.

It is also called a recursive relationship.

Example: An employee manages another employee.

**EMPLOYEE manages EMPLOYEE**

			EMPLOYEE
			MANAGES
			EMPLOYEE

The same entity type participates twice, with different roles: manager and subordinate.

Other examples:

- A person is married to another person.
    
- An employee supervises another employee.
    
- A course has another course as its prerequisite.


### **B. Binary Relationship**

A **Binary Relationship** is a relationship in which two entity types participate.

This is the most common type of relationship in database design.

Examples:

- Student enrolls in Course.
    
- Employee works in Department.
    
- Customer places Order.

**STUDENT — ENROLLS IN — COURSE**

**STUDENT**              ENROLLS IN             **COURSE**

Two entity types participate in the relationship.


### **C. Ternary Relationship**

A **Ternary Relationship** is a relationship in which three entity types participate simultaneously.

Example: A supplier supplies a particular product to a particular project.

The three entities are:

- Supplier
    
- Product
    
- Project

The relationship is `SUPPLIES`.

**SUPPLIES**

			SUPPLIES
**SUPPLIER**         **PRODUCT**         **PROJECT**

The supply association involves all three entities together.

For example, Supplier A supplies Product X to Project P.

The meaning of the relationship depends on the combination of all three entities, not just independent pairwise connections.

**Remember:**

|Degree|Number of participating entity types|
|---|---|
|Unary|1|
|Binary|2|
|Ternary|3|

We can also have relationships of higher degree, but unary, binary and ternary are the main ones to understand.


## **3. Cardinality (Mapping Cardinality)**

Now comes a very important concept.

**Cardinality** describes how many instances of one entity can be associated with instances of another entity through a relationship.

For example, consider Students and Courses.

We need to answer:

- Can one student enroll in multiple courses?
    
- Can one course have multiple students?

The answers help us determine the relationship type.

There are four common cardinality types.

### **A. One-to-One (1:1) Relationship**

A **One-to-One Relationship** means one instance of Entity A is associated with at most one instance of Entity B, and vice versa.

Example: Person and Passport.

- One person can have at most one passport in this simplified model.
    
- One passport belongs to at most one person.

**PERSON — HAS — PASSPORT**

Person A       **->**        Passport A        
Person B       **->**        Passport B
Person C       ->        Passport C

Each person is matched with one passport in this example.

**Database implementation:**

We can implement a 1:1 relationship using a foreign key with a `UNIQUE` constraint.

```
CREATE TABLE Person (
    person_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Passport (
    passport_id INT PRIMARY KEY,
    passport_number VARCHAR(30) UNIQUE,
    person_id INT UNIQUE REFERENCES Person(person_id)
);
```

The `UNIQUE` constraint on `person_id` prevents multiple passport records from referencing the same person.

### **B. One-to-Many (1:M) Relationship**

A **One-to-Many Relationship** means one instance of Entity A can be associated with multiple instances of Entity B, but each instance of Entity B is associated with at most one instance of Entity A.

Example: Department and Employees.

- One department can have many employees.
    
- Each employee belongs to one department in this example.

**DEPARTMENT — HAS — EMPLOYEES**

**IT Department**         **->**       Employee A
                **->**       Employee B
                **->**       Employee C

One department is associated with multiple employees.

**Database implementation:**

The foreign key is placed on the Many side.

```
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT REFERENCES Department(department_id)
);
```

Notice that `department_id` is stored in the Employee table.

Why?

Because many employees can reference the same department.

This is one of the most important practical rules of relational database design.

### **C. Many-to-One (M:1) Relationship**

A **Many-to-One Relationship** means multiple instances of Entity A can be associated with one instance of Entity B.

Example: Employees and Department.

- Many employees can belong to one department.
    
- Each employee belongs to one department in this example.

**EMPLOYEES — BELONG TO — DEPARTMENT**

Employee A               **->**        
Employee B               **->**           **IT Department**
Employee C               **->**

Multiple employees are associated with one department.

**Important:** One-to-Many and Many-to-One describe the same relationship from opposite directions.

|Direction|Cardinality|
|---|---|
|Department → Employees|1:M|
|Employees → Department|M:1|

The SQL implementation is the same as the previous example.

### **D. Many-to-Many (M:N) Relationship**

A **Many-to-Many Relationship** means multiple instances of Entity A can be associated with multiple instances of Entity B.

Example: Students and Courses.

- One student can enroll in multiple courses.
    
- One course can have multiple students.

**STUDENT — ENROLLS IN — COURSE**

**Students**               **Courses**

Faizan                      Java
Aman                       DBMS
Ravi                         Networking

Students can enroll in several courses, and courses can have several students.


**Database implementation:**

A Many-to-Many relationship is generally implemented using a **Junction Table** (also called a Bridge Table or Associative Table).

For example:

**Students**

|student_id|name|
|---|---|
|101|Faizan|
|102|Aman|

**Courses**

|course_id|course_name|
|---|---|
|1|Java|
|2|DBMS|

**Enrollments**

|student_id|course_id|
|---|---|
|101|1|
|101|2|
|102|1|

The Enrollments table connects Students and Courses.

SQL implementation:

```
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

CREATE TABLE Enrollments (
    student_id INT REFERENCES Students(student_id),
    course_id INT REFERENCES Courses(course_id),
    enrollment_date DATE,
    PRIMARY KEY (student_id, course_id)
);
```

Notice something important:

- `student_id` is a foreign key.
    
- `course_id` is a foreign key.
    
- Together, they form a composite primary key.

The `enrollment_date` is an attribute of the enrollment relationship.

This connects directly to what we learned in Topics 3.5 and 3.6.

## **4. Cardinality vs Degree — Don't confuse them**

These two concepts are frequently confused.

|Degree|Cardinality|
|---|---|
|Number of entity types participating|Number of entity instances associated|
|Unary, Binary, Ternary|1:1, 1:M, M:1, M:N|
|Focuses on participating entity types|Focuses on how many instances can be associated|

For example:

`STUDENT — ENROLLS IN — COURSE`

- Degree = Binary (two entity types).
    
- Cardinality = Many-to-Many (many students can enroll in many courses).

A relationship has both a degree and a cardinality.

## **5. Quick Revision**

|Relationship|Meaning|Example|
|---|---|---|
|Unary|Entity relates to same entity type|Employee manages Employee|
|Binary|Two entity types participate|Student enrolls in Course|
|Ternary|Three entity types participate|Supplier supplies Product to Project|
|1:1|One instance ↔ one instance|Person–Passport|
|1:M|One instance → many instances|Department–Employees|
|M:1|Many instances → one instance|Employees–Department|
|M:N|Many instances ↔ many instances|Students–Courses|

**Practical database rules:**

1. 1:1 — Use a foreign key with a UNIQUE constraint, depending on the design.
    
2. 1:M — Put the foreign key on the Many side.
    
3. M:N — Create a junction table containing foreign keys to both entities.
    
4. Unary relationships — Can be implemented using a self-referencing foreign key or a separate relationship table, depending on cardinality and requirements.