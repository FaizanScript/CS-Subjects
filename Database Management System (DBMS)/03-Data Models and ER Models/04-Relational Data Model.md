

### 1. What is the Relational Data Model?

**Definition**: The Relational Data Model is a database model in which data is organized into tables (relations), consisting of rows (tuples) and columns (attributes), with relationships between tables established using keys.

In simple terms, data is stored in tables, and different tables can be connected through relationships.

Examples of relational DBMS:

- PostgreSQL
    
- MySQL
    
- Oracle Database
    
- Microsoft SQL Server
    
- SQLite


### 2. Basic Components of Relational Data Model

There are several important terms you must understand.

Let's use a `Students` table as our example.

|student_id|name|age|course|
|---|---|---|---|
|101|Faizan|20|BCA|
|102|Aman|21|BCA|
|103|Ravi|19|MCA|

#### A. Relation (Table)

**Definition**: A Relation is a two-dimensional structure consisting of rows and columns that represents a collection of related data.

In practical SQL terminology, we generally call it a table.

Example: `Students` is a relation.

#### B. Tuple (Row)

**Definition**: A Tuple is a single record or row in a relation, representing one complete set of related attribute values.

For example:

|student_id|name|age|course|
|---|---|---|---|
|101|Faizan|20|BCA|

This row represents one tuple.


#### C. Attribute (Column)

**Definition**: An Attribute is a named property or characteristic of an entity represented by a column in a relation.

Examples:

- `student_id`
    
- `name`
    
- `age`
    
- `course`

Each column represents a particular property of the stored records.

#### D. Domain

**Definition**: A Domain is the set of permitted atomic values that an attribute can contain.

For example:

|Attribute|Possible domain|
|---|---|
|age|Positive integers within an allowed range|
|name|Valid character strings|
|student_id|Positive integers|
|gender|Values from an allowed predefined set, if the design restricts it|

For example, if the domain of `age` is defined as integers between 16 and 100, then values such as `-5` or `150` are invalid.

Important: A domain defines which values are permitted for an attribute, not just its data type.

#### E. Degree (Arity)

**Definition**: The Degree of a relation is the total number of attributes (columns) present in that relation.

Our Students table has:

- student_id
    
- name
    
- age
    
- course

Therefore:

Degree = 4

#### F. Cardinality

**Definition**: The Cardinality of a relation is the total number of tuples (rows) present in that relation.

Our Students table contains three records.

Therefore:

Cardinality = 3

Don't confuse Degree and Cardinality.

# 4                                   3
Degree                                              Cardinality
Number of columns                          Number of rows


### 3. Properties of a Relation

In the formal relational model, a relation follows certain properties.

1. **Unique relation name**

Each relation should have a distinct name within its database schema.

2. **Unique attribute names**

Each attribute in a relation must have a unique name.

For example, a table cannot have two columns both named `student_id`.

3. **Atomic values**

Each attribute value should be atomic (indivisible) in the relational model.

For example:

==🟢Correct representation==

|student_id|name|phone|
|---|---|---|
|101|Faizan|9876543210|
|102|Aman|9123456780|

==🔴Non-atomic representation==

|student_id|name|phone|
|---|---|---|
|101|Faizan|9876543210, 9123456780|

The second example stores multiple phone values in one cell, violating the atomic-value requirement of the classical relational model.

4. **No duplicate tuples**

A relation is a set of tuples, so duplicate rows are not permitted in the formal relational model.

5. **Order of rows does not matter**

The logical meaning of a relation does not depend on row order.

6. **Order of columns does not matter**

The logical meaning of a relation does not depend on the physical ordering of its attributes.

7. **Values belong to their respective domains**

An attribute should contain values permitted by its domain.



### 4. Keys in the Relational Data Model

Keys are extremely important because they help uniquely identify records and establish relationships between tables.

Let's understand them using another example.

Students table

| student_id | name   | email              |
| ---------- | ------ | ------------------ |
| 101        | Faizan | faizan@example.com |
| 102        | Aman   | aman@example.com   |
| 103        | Ravi   | ravi@example.com   |

#### A. Super Key

**Definition**: A Super Key is a set of one or more attributes that uniquely identifies each tuple in a relation.

Examples:

- `{student_id}`
    
- `{email}`
    
- `{student_id, name}`
    
- `{student_id, email}`

All these can uniquely identify a student, assuming the relevant values are unique and non-null.

A super key may contain unnecessary attributes.

#### B. Candidate Key

**Definition**: A Candidate Key is a minimal super key, meaning no attribute can be removed from it without losing its ability to uniquely identify each tuple.

Examples:

- `student_id`
    
- `email`

Both are candidate keys if each is unique and non-null.

But `{student_id, name}` is not a candidate key because `name` is unnecessary when `student_id` alone is sufficient.

#### C. Primary Key

**Definition**: A Primary Key is the candidate key selected to uniquely identify each tuple in a relation.

Example:

```
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(150) UNIQUE
);
```

Here:

- `student_id` is the primary key.
    
- `email` is another candidate key if it is also constrained to be non-null and unique.

**Important properties:**

- Must uniquely identify each row.
    
- Cannot contain NULL values.
    
- A table has only one primary key constraint, although that key may consist of multiple columns.

#### D. Alternate Key

**Definition**: An Alternate Key is a candidate key that was not selected as the primary key.

In our example:

- Primary key: `student_id`
    
- Alternate key: `email`

#### E. Foreign Key

**Definition**: A Foreign Key is an attribute or set of attributes in one relation that references a candidate key (usually the primary key) in another relation, helping establish a relationship between the two relations.

Example:

Courses table

|course_id|course_name|
|---|---|
|1|BCA|
|2|MCA|

Students table

|student_id|name|course_id|
|---|---|---|
|101|Faizan|1|
|102|Aman|1|
|103|Ravi|2|

Here:

- `Courses.course_id` is the primary key.
    
- `Students.course_id` is the foreign key.

The foreign key connects each student record to an existing course.

SQL implementation:

```
CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    course_id INT,
    FOREIGN KEY (course_id)
        REFERENCES Courses(course_id)
);
```

Important: A foreign key does not necessarily have to reference a primary key; it can reference another candidate key supported by a suitable uniqueness constraint.

#### F. Composite Key

**Definition**: A Composite Key is a key consisting of two or more attributes that together uniquely identify a tuple.

Example: Consider an `Enrollments` table.

|student_id|course_id|enrollment_date|
|---|---|---|
|101|1|2026-07-01|
|101|2|2026-07-02|
|102|1|2026-07-03|

Neither `student_id` nor `course_id` alone uniquely identifies an enrollment.

But together:

(student_id, course_id)

They uniquely identify each enrollment, assuming a student can enroll in a particular course only once.

SQL:

```
CREATE TABLE Enrollments (
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    PRIMARY KEY (student_id, course_id)
);
```

Key hierarchy — important for interviews

			   Super Key
   Any set of attributes that uniquely identifies a row

		   Minimal super keys

			Candidate Keys
		   Minimal super keys

	   One candidate key is selected

			Primary Key
		Selected candidate key

Other candidate keys are called alternate keys. A composite key describes the number of attributes making up a key, not a separate position in this hierarchy.


### 5. Relationships Between Tables

One of the biggest strengths of the relational model is that it allows different tables to be connected.

There are three common relationship types:

**A. One-to-One (1:1)**

One record in Table A is associated with at most one record in Table B, and vice versa.

Example:

- One person has one passport.
    
- One passport belongs to one person.

**B. One-to-Many (1:M)**

One record in Table A can be associated with multiple records in Table B.

Example:

- One department has multiple students.
    
- Each student belongs to one department in this example.

**C. Many-to-Many (M:N)**

Multiple records in Table A can be associated with multiple records in Table B.

Example:

- One student enrolls in multiple courses.
    
- One course has multiple students.

A Many-to-Many relationship is generally implemented using an intermediate table, also called a junction table or associative table.

Example:

The `Enrollments` table connects Students and Courses through foreign keys.

We'll study these relationships in greater depth when we reach Topic 3.7.


### 6. Advantages of the Relational Data Model

|Advantage|Explanation|
|---|---|
|Simple structure|Data is organized into understandable tables.|
|Data integrity|Keys and constraints help maintain valid data.|
|Reduced redundancy|Good relational design and normalization reduce unnecessary duplication.|
|Flexible querying|SQL allows data retrieval using different conditions and joins.|
|Data independence|Logical structure can be changed with appropriate abstraction and mappings.|
|Relationships|Tables can be connected using keys.|
|Security|Permissions can control access to database objects and operations.|

### 7. Limitations of the Relational Data Model

|Limitation|Explanation|
|---|---|
|Complex joins|Queries involving many tables can become complicated.|
|Schema requirements|Traditional relational databases generally require a defined schema.|
|Horizontal scaling complexity|Distributing relational data across multiple machines can require additional design and infrastructure.|
|Object-relational mismatch|Application objects do not always map naturally to relational tables.|
|Less natural for deeply connected data|Some graph-oriented use cases are easier to represent in graph databases.|

These limitations don't make relational databases outdated. PostgreSQL and MySQL remain highly relevant for modern backend development.

## 8. Important Terminology — Revision Table

|Term|Meaning|
|---|---|
|Relation|Table|
|Tuple|Row|
|Attribute|Column|
|Domain|Permitted values for an attribute|
|Degree|Number of columns|
|Cardinality|Number of rows|
|Super Key|Any attribute set that uniquely identifies a row|
|Candidate Key|Minimal super key|
|Primary Key|Selected candidate key|
|Alternate Key|Candidate key not selected as primary|
|Foreign Key|Attribute(s) referencing a candidate key in another relation|
|Composite Key|Key made up of multiple attributes|
