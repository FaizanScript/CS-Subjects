

## **1. What is ER-to-Relational Mapping?**

**ER-to-Relational Mapping** is the process of converting an Entity-Relationship (ER) model into a set of relational tables while preserving its entities, attributes, relationships, and constraints.

Think of the database design workflow:

		    Requirements
   What information does the application need?

			   ER Model
    Entities, attributes and relationships

		    Relational Model
   Tables, columns, primary keys and foreign keys

			SQL Implementation
   CREATE TABLE, constraints and relationships

Let's understand the mapping rules one by one.


## **2. Mapping Strong Entities**

**Rule: Every strong entity becomes a separate relational table.**

- Entity attributes become table columns.
    
- The entity's key attribute becomes the table's primary key.
    
- Simple attributes become individual columns.
    
- Composite attributes are broken into their component attributes.

**Example:**

Suppose our ER model contains a `Student` entity:

- student_id (key)
    
- name (composite: first_name, last_name)
    
- email (simple)
    
- date_of_birth (simple)

We convert it into:

**Student table**

|student_id (PK)|first_name|last_name|email|date_of_birth|
|---|---|---|---|---|
|101|Faizan|Khan|faizan@example.com|2006-06-27|
|102|Rahul|Kumar|rahul@example.com|2005-09-12|

Notice that `name` itself isn't stored as one column here. Its components become separate columns.

**SQL implementation:**

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    date_of_birth DATE
);
```


## **3. Mapping Weak Entities**

**Rule: A weak entity becomes a separate table containing the primary key of its owner entity as a foreign key.**

The weak entity's partial key combines with the owner's primary key to form its composite primary key.

Consider:

- Employee is a strong entity.
    
- Dependent is a weak entity.
    
- A dependent belongs to an employee.
    
- `dependent_name` is the partial key.

**Employee**

|employee_id (PK)|employee_name|
|---|---|
|1|Faizan|
|2|Rahul|

One employee can have multiple dependents

**Dependent**

|employee_id (PK, FK)|dependent_name (PK)|age|
|---|---|---|
|1|Ali|10|
|1|Sara|15|
|2|Riya|12|

Why is the primary key composite?

Because `dependent_name` alone might not be unique across all employees.

But the combination:

`(employee_id, dependent_name)`

can uniquely identify each dependent under the assumed business rules.

**SQL implementation:**

```
CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100)
);

CREATE TABLE Dependent (
    employee_id INT,
    dependent_name VARCHAR(100),
    age INT,

    PRIMARY KEY (employee_id, dependent_name),

    FOREIGN KEY (employee_id)
        REFERENCES Employee(employee_id)
);
```

Here:

- `employee_id` is a foreign key referencing Employee.
    
- `employee_id` and `dependent_name` together form the primary key.


## **4. Mapping Relationships**

This is one of the most important parts of ER-to-relational mapping.

The mapping depends on the relationship's cardinality.

### **A. One-to-One (1:1)**

**Rule: Add a foreign key to either one of the two tables, preferably on the side with total participation when that avoids unnecessary NULL values. Add a UNIQUE constraint.**

Example: Person and Passport.

- One person can have one passport.
    
- One passport belongs to one person.

**Person table**

|person_id (PK)|name|
|---|---|
|1|Faizan|
|2|Rahul|

**Passport table**

|passport_id (PK)|person_id (FK, UNIQUE)|
|---|---|
|501|1|
|502|2|

Why `UNIQUE`?

Because without it, multiple passports could reference the same person, which would allow a one-to-many relationship instead.

```
CREATE TABLE Person (
    person_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Passport (
    passport_id INT PRIMARY KEY,
    person_id INT UNIQUE REFERENCES Person(person_id)
);
```

If every passport must belong to a person, make `person_id` `NOT NULL`.

### **B. One-to-Many (1:M)**

Very important

**Rule: Place the foreign key on the many side.**

Example: One department has many employees.

**Department**

|department_id (PK)|department_name|
|---|---|
|10|IT|
|20|HR|

**Employee**

|employee_id (PK)|name|department_id (FK)|
|---|---|---|
|1|Faizan|10|
|2|Rahul|10|
|3|Aman|20|

Why does the foreign key go into Employee?

Because multiple employees can belong to the same department. Each employee references their department.

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

If every employee must belong to a department, use `NOT NULL` on `department_id`.

### **C. Many-to-Many (M:N)**

Very important

**Rule: Create a separate junction table (also called a bridge or associative table).**

Example:

- One student can enroll in multiple courses.
    
- One course can have multiple students.

We cannot directly represent this relationship using just one foreign key in either Student or Course.

Instead, we create an `Enrollment` table.

**Student**

|student_id (PK)|name|
|---|---|
|101|Faizan|
|102|Rahul|

**Course**

|course_id (PK)|course_name|
|---|---|
|1|DBMS|
|2|Java|

**Enrollment**

|student_id (PK, FK)|course_id (PK, FK)|enrollment_date|
|---|---|---|
|101|1|2026-11-01|
|101|2|2026-11-02|
|102|1|2026-11-03|

Notice:

- `student_id` references Student.
    
- `course_id` references Course.
    
- Both together form a composite primary key.
    
- `enrollment_date` is an attribute of the relationship, so it belongs in Enrollment.

**SQL implementation:**

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Course (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE,

    PRIMARY KEY (student_id, course_id),

    FOREIGN KEY (student_id)
        REFERENCES Student(student_id),

    FOREIGN KEY (course_id)
        REFERENCES Course(course_id)
);
```

One important assumption: this primary key means a student can enroll in a particular course only once. If students can enroll again in different semesters, we need to include a semester/term identifier in the key or use a separate enrollment ID with suitable uniqueness constraints.


## **5. Mapping Multi-Valued Attributes**

**Rule: Create a separate table for every multi-valued attribute.**

Remember our Student entity with multiple phone numbers.

We should not store multiple phone numbers in one column like this:

|student_id|phone_numbers|
|---|---|
|101|9876..., 8765...|

That would violate the relational model's atomic-value principle.

Instead:

**Student**

|student_id (PK)|name|
|---|---|
|101|Faizan|

**StudentPhone**

|student_id (PK, FK)|phone_number (PK)|
|---|---|
|101|9876543210|
|101|8765432109|

```
CREATE TABLE StudentPhone (
    student_id INT,
    phone_number VARCHAR(15),

    PRIMARY KEY (student_id, phone_number),

    FOREIGN KEY (student_id)
        REFERENCES Student(student_id)
);
```

Here, the combination of student ID and phone number identifies each phone record.

If duplicate phone values need to be allowed for a particular business reason, use a separate phone record ID instead.


## **6. Mapping Derived Attributes**

**Rule: Derived attributes are generally not stored as regular columns. They are calculated from existing data when required.**

Example:

- `date_of_birth` is stored.
    
- `age` is derived from date of birth.

Instead of storing both, calculate age when needed.

In PostgreSQL:

```
SELECT
    student_id,
    date_of_birth,
    EXTRACT(YEAR FROM AGE(date_of_birth)) AS age
FROM Student;
```

Why avoid storing age?

Because age changes over time. If we store it directly, we would need to update it regularly.

There are exceptions: derived values can be deliberately stored for performance or reporting, but then the application must keep them consistent with their source data.


## **7. Complete Example — College Management Database**

Let's combine what we've learned.

### **Requirements**

Imagine we're designing a college management system.

1. A student has a unique ID, name, email and date of birth.
    
2. A student can have multiple phone numbers.
    
3. Age is calculated using date of birth.
    
4. A course has a unique ID, name and credits.
    
5. Students can enroll in multiple courses.
    
6. Courses can have multiple students.
    
7. Every enrollment has an enrollment date.

### **Step 1: Identify entities**

- Student
    
- Course

### **Step 2: Identify attributes**

**Student:**

- student_id — key
    
- first_name and last_name — components of composite name
    
- email — simple
    
- date_of_birth — stored
    
- phone_number — multi-valued
    
- age — derived

**Course:**

- course_id — key
    
- course_name
    
- credits

### **Step 3: Identify relationships**

Student ↔ Course = M:N

We need a separate `Enrollment` table.

### **Step 4: Convert into relational tables**

**Student**
One row per student
`student_id (PK), first_name, last_name, email, date_of_birth`

**Course**
One row per course
`course_id (PK), course_name, credits`

**StudentPhone**
One row per student phone number
`student_id (PK, FK), phone_number (PK)`

**Enrollment**
One row per student-course enrollment
`student_id (PK, FK), course_id (PK, FK), enrollment_date`

### **Step 5: Complete SQL implementation**

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    date_of_birth DATE
);

CREATE TABLE Course (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100),
    credits INT
);

CREATE TABLE StudentPhone (
    student_id INT,
    phone_number VARCHAR(15),

    PRIMARY KEY (student_id, phone_number),

    FOREIGN KEY (student_id)
        REFERENCES Student(student_id)
);

CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrollment_date DATE,

    PRIMARY KEY (student_id, course_id),

    FOREIGN KEY (student_id)
        REFERENCES Student(student_id),

    FOREIGN KEY (course_id)
        REFERENCES Course(course_id)
);
```

This is how a conceptual ER design becomes a relational database structure that can be implemented using SQL.


## **8. Quick Revision — All Mapping Rules**

| ER component           | Relational mapping                        |
| ---------------------- | ----------------------------------------- |
| Strong entity          | Separate table                            |
| Simple attribute       | Column                                    |
| Composite attribute    | Component columns                         |
| Key attribute          | Primary key                               |
| Weak entity            | Table with owner's FK and composite PK    |
| 1:1 relationship       | FK + UNIQUE                               |
| 1:M relationship       | FK on many side                           |
| M:N relationship       | Separate junction table                   |
| Multi-valued attribute | Separate table                            |
| Derived attribute      | Usually calculated, not stored            |
| Relationship attribute | Column in the relationship/junction table |
