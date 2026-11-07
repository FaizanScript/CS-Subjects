
## 1. Learning objective

In this topic, we'll understand:

- What DBMS means.
    
- What RDBMS means.
    
- How relational databases organize data.
    
- The differences between DBMS and RDBMS.
    
- Why relational databases are widely used in backend development.

This is an important topic because you'll work with databases such as PostgreSQL and MySQL during backend development.


## 2. What is DBMS?

**Definition**: A Database Management System (DBMS) is software that allows users and applications to define, create, store, retrieve, update and manage databases.

A DBMS provides mechanisms for:

- Data storage and retrieval.
    
- Data security.
    
- Data integrity.
    
- Transaction management.
    
- Concurrency control.
    
- Backup and recovery.

Examples include:

- SQLite.
    
- MongoDB.
    
- PostgreSQL.
    
- MySQL.
    
- Oracle Database.

Important: DBMS is a broad category of database management software. Not every DBMS organizes data using relational tables.


## 3. What is RDBMS?

**Definition**: A Relational Database Management System (RDBMS) is a type of DBMS that organizes data primarily into tables (relations) consisting of rows and columns, and supports relationships between tables.

Let's understand this through an example.

Imagine you're developing a college management system.

You have two tables:

Students

|student_id|name|course|
|---|---|---|
|101|Faizan|BCA|
|102|Rahul|BCA|
|103|Aman|BCA|

Fees

|fee_id|student_id|amount|
|---|---|---|
|1|101|15000|
|2|102|12000|
|3|103|15000|

Notice something important:

- `Students` stores student information.
    
- `Fees` stores fee information.
    
- Both tables contain `student_id`.

The `student_id` column in the `Students` table can be a Primary Key.

The `student_id` column in the `Fees` table can be a Foreign Key referencing the Students table.

This establishes a relationship between the two tables.

Relational database structure

**Students:-**

|student_id (PK)|name|
|---|---|
|101|Faizan|
|102|Rahul|
|103|Aman|

student_id relationship
Primary Key → Foreign Key

**Fees:-**

|fee_id (PK)|student_id (FK)|amount|
|---|---|---|
|1|101|15000|
|2|102|12000|
|3|103|15000|

This is the fundamental idea behind relational databases: data is organized into related tables rather than keeping all information in one unnecessarily large table.


## 4. Important terminology in RDBMS

Before comparing DBMS and RDBMS, understand these terms.

### 4.1 Table (Relation)

A table is a collection of related data organized into rows and columns.

Example: `Students` table.

### 4.2 Row (Tuple / Record)

A row represents one individual record in a table.

Example:

|student_id|name|course|
|---|---|---|
|101|Faizan|BCA|

This represents one student.


### 4.3 Column (Attribute / Field)

A column represents a particular property of the records stored in a table.

Examples:

- `student_id`
    
- `name`
    
- `course`

### 4.4 Primary Key

**Definition**: A Primary Key is a column or combination of columns that uniquely identifies each row in a table.

Example:

|student_id (PK)|name|
|---|---|
|101|Faizan|
|102|Rahul|
|103|Aman|

Here, `student_id` uniquely identifies every student.

Important properties:

- Must contain unique values.
    
- Cannot contain NULL values.
    
- Each table can have only one primary key constraint, though it may consist of multiple columns (composite primary key).

### 4.5 Foreign Key

**Definition**: A Foreign Key is a column or combination of columns in one table that references a candidate key, commonly the primary key, of another table.

Example:

`Fees.student_id` references `Students.student_id`.

This helps maintain referential integrity.

For example, the fees table should not reference a student ID that doesn't exist in the Students table, provided the foreign key constraint is enforced.

### 4.6 Relationship

A relationship represents an association between records in different tables.

The three common relationship types are:

1. One-to-One (1:1): One record in Table A is associated with at most one record in Table B.
    
2. One-to-Many (1:M): One record in Table A can be associated with multiple records in Table B.
    
3. Many-to-Many (M:N): Multiple records in Table A can be associated with multiple records in Table B.

Example:

- One student → Multiple fee payments: One-to-Many.
    
- Students ↔ Courses: Many-to-Many, if multiple students can enroll in multiple courses.

We'll study relationships in greater detail when we reach the ER model.


## 5. DBMS vs RDBMS: Major differences

Now let's compare them.

|Basis|DBMS|RDBMS|
|---|---|---|
|Meaning|General database management software|Relational database management software|
|Data organization|Depends on the database model|Primarily tables|
|Relationships|Depends on the model|Supported through relational structures|
|Keys|Depends on the model|Primary and foreign keys are commonly used|
|Data integrity|Depends on implementation|Supports relational constraints|
|Normalization|Not universally applicable|Commonly used to reduce redundancy|
|Query language|Depends on the DBMS|SQL is commonly used|
|Examples|MongoDB, SQLite, PostgreSQL|PostgreSQL, MySQL, Oracle Database|

Important clarification: RDBMS is a subtype of DBMS. Every RDBMS is a DBMS, but not every DBMS is an RDBMS.

Also, some traditional textbooks make broad claims such as "DBMS does not support relationships" or "DBMS does not support multiple users." These are oversimplifications. Non-relational DBMSs can support relationships and concurrent access too; their data models and mechanisms differ.


## 6. Types of DBMS based on data models

To understand why DBMS and RDBMS are not identical, let's briefly look at different database models.

1. **Hierarchical DBMS**

Organizes data in a tree-like structure with parent-child relationships.

Example: IBM Information Management System (IMS).

2. **Network DBMS**

Organizes records using interconnected relationships, allowing a record to have multiple parent relationships.

Example: IDMS.

3. **Relational DBMS (RDBMS)**

Organizes data into tables and establishes relationships using keys.

Examples: PostgreSQL, MySQL and Oracle Database.

4. **NoSQL DBMS**

Includes non-relational data models such as document, key-value, wide-column and graph databases.

Example: MongoDB.

This is a simplified classification of database models. Modern database systems can support features from multiple models.

## 7. Practical connection: PostgreSQL vs MongoDB

Since you're learning MERN, this distinction matters.

**PostgreSQL**
**==🟢RDBMS==**

- Tables and rows.
    
- SQL queries.
    
- Primary and foreign keys.
    
- Relational constraints.

**MongoDB**
**==🔵NoSQL DBMS==**

- Collections and documents.
    
- BSON document format.
    
- Flexible schemas.
    
- Supports references and embedded documents.

For example, in PostgreSQL, student records are commonly represented as rows in tables.

In MongoDB, student records are commonly represented as BSON documents inside collections.

Both can manage complex applications, but they organize and model data differently.

## 8. Quick revision

Remember these five points:

1. DBMS is the broader category of database management software.
    
2. RDBMS is a type of DBMS based on the relational model.
    
3. RDBMS primarily organizes data into tables.
    
4. Primary keys identify records; foreign keys establish references between tables.
    
5. PostgreSQL and MySQL are RDBMSs, whereas MongoDB is a document-oriented NoSQL DBMS.