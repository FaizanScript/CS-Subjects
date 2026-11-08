
### 1. What is Three-Schema Architecture?

The Three-Schema Architecture is a database architecture that divides a database system into three different levels of abstraction:

1. **External Level**
    
2. **Conceptual Level**
    
3. **Internal Level**

It is also known as the ANSI/SPARC Three-Schema Architecture.

The main purpose is to separate how different users view data from how the database is logically organized and how it is physically stored.

Let's understand why this separation is necessary.

### 2. Why do we need three levels?

Imagine a college management system containing:

- Student information.
    
- Teacher information.
    
- Course details.
    
- Examination results.
    
- Fee records.
    
- Employee salaries.

Different people need different information from the same database.

For example:

- Students need to see their own marks and fee details.
    
- Teachers need to see students' marks and course information.
    
- The principal needs access to college-wide reports.
    
- The database system needs to manage how all this information is physically stored.

Should every user see the entire database and understand how its data is stored internally?

No.

A student doesn't need to know how database files are organized on a disk. Similarly, a database administrator doesn't need to expose every internal storage detail to application users.

This is why DBMS separates the database into three levels.


### 3. The three levels of architecture

## Three-Schema Architecture

				Level 1
			   External Level
		   Individual user views
		What does each user see?

				Level 2
			Conceptual Level
	Complete logical structure of the database
	What data is stored and how is it related?

				Level 3
			  Internal Level
		Physical storage structure

How is the data actually stored?

Simplified representation of the three levels and their mappings.

#### A. External Level (View Level)

The External Level is the highest level of database abstraction. It describes how individual users or groups of users view the database.

Different users can have different views of the same database.

Let's use our college example.

Suppose the college database contains a Students table:

|Student ID|Name|Course|Marks|Fees|
|---|---|---|---|---|
|101|Ali|BCA|85|25,000|
|102|Rahul|BCA|78|20,000|
|103|Sara|BBA|91|22,000|

Different users require different information.

Student's view:

|Student ID|Name|Marks|
|---|---|---|
|101|Ali|85|

The student may only be allowed to view their own result.

Teacher's view:

|Student ID|Name|Course|Marks|
|---|---|---|---|
|101|Ali|BCA|85|
|102|Rahul|BCA|78|

The teacher may need student names, courses and marks.

Accounts department's view:

|Student ID|Name|Fees|
|---|---|---|
|101|Ali|25,000|
|102|Rahul|20,000|
|103|Sara|22,000|

The accounts department needs fee-related information.

Notice that all these views are based on the same underlying database, but each user sees a different representation.

Important characteristics:

- It is the highest level of abstraction.
    
- Different users can have different views.
    
- Users don't need to understand the complete database structure.
    
- It helps provide customized access to information.

Key point: External Level = What a particular user sees.

#### B. Conceptual Level (Logical Level)

The Conceptual Level describes the complete logical structure of the entire database.

It defines:

- What data is stored.
    
- What tables or entities exist.
    
- What attributes each table contains.
    
- Relationships between tables.
    
- Constraints and rules applied to the data.

Unlike the external level, which provides individual views, the conceptual level describes the database as a whole.

For example, our college database might have the following tables:

Students

|student_id|name|course_id|
|---|---|---|
|101|Ali|1|
|102|Rahul|1|

Courses

|course_id|course_name|
|---|---|
|1|BCA|
|2|BBA|

Fees

|fee_id|student_id|amount|
|---|---|---|
|501|101|25,000|
|502|102|20,000|

At the conceptual level, the database defines:

- Students, Courses and Fees as tables.
    
- `student_id` as the primary key of Students.
    
- `course_id` as a foreign key in Students.
    
- `student_id` as a foreign key in Fees.
    
- The relationships between these tables.

The conceptual level doesn't concern itself with exactly where these records are stored on the disk.

Important characteristics:

- Describes the complete logical structure.
    
- Defines entities, attributes and relationships.
    
- Defines constraints and data types.
    
- Hides physical storage details.
    
- Usually has one conceptual schema for the database.

Key point: Conceptual Level = What data exists and how it is logically organized.

#### C. Internal Level (Physical Level)

The Internal Level is the lowest level of database abstraction.

It describes how data is physically stored and organized inside the storage system.

It deals with implementation details such as:

- File organization.
    
- Storage blocks and pages.
    
- Record placement.
    
- Index structures.
    
- Data compression.
    
- Access paths.
    
- Storage allocation.

For example, consider the Students table.

At the conceptual level, we know that the table contains student IDs, names and course IDs.

But at the internal level, the DBMS handles how those records are stored on storage devices, how indexes are organized, and how records can be located efficiently.

A database might use a B-tree index to speed up searches using `student_id`.

The internal level deals with the physical organization and access mechanisms behind that operation.

Important characteristics:

- Lowest level of abstraction.
    
- Describes physical data storage.
    
- Deals with files, pages, indexes and access paths.
    
- Hides storage implementation details from higher levels.

Key point: Internal Level = How data is physically stored.


### 4. Complete comparison of the three levels

|Feature|External|Conceptual|Internal|
|---|---|---|---|
|Also called|View level|Logical level|Physical level|
|Focus|User views|Overall logical structure|Physical storage|
|Describes|What users see|What data exists|How data is stored|
|Main concern|Individual views|Tables, relationships, constraints|Files, indexes, pages|
|Abstraction|Highest|Middle|Lowest|
|Typical audience|End users|Designers and developers|DBAs and system specialists|

Remember these three questions:

- **External:** What does the user see?
    
- **Conceptual:** What is stored in the database?
    
- **Internal**: How is it stored?

### 5. Schema and Instance

Before we finish this topic, understand two important DBMS terms.

###### **Schema**
A schema is the overall design or structure of a database. It defines how data is organized, including tables, columns, relationships and constraints.

Example:

```
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    course_id INT
);
```

This defines the structure of a Students table.

###### **Instance**
A database instance refers to the actual data stored in the database at a particular moment in time.

For example:

|student_id|name|course_id|
|---|---|---|
|101|Ali|1|
|102|Rahul|1|

These actual records represent the data in the database at that moment.

Difference:

|Schema|Instance|
|---|---|
|Database structure|Actual database data|
|Changes relatively infrequently|Changes frequently|
|Defined using DDL|Modified using database operations|
|Example: Students table definition|Example: Student records|

Think of it this way:

- Schema = Blueprint of a house.
    
- Instance = The actual house and its current contents.

The database schema defines the structure; the instance contains the actual data following that structure.

### 6. Schema mappings

The three levels are not independent databases. They are connected through mappings.

There are two important mappings:

1. **External–Conceptual Mapping**

Connects an individual user's external view to the conceptual database structure.

For example, a student sees only their own marks, while the underlying conceptual structure contains complete student and examination information.

2. **Conceptual–Internal Mapping**

Connects the conceptual database structure to the internal physical storage structure.

For example, a logical Students table is mapped to the physical records, pages and indexes used by the DBMS.

These mappings help the DBMS translate requests between different levels.

We'll use these ideas again when studying data independence.

### 7. Quick revision

**Three-Schema Architecture**

1. External Level: Individual user views of the database.
    
2. Conceptual Level: Complete logical structure of the database.
    
3. Internal Level: Physical storage and organization of data.

Other important terms

- Schema = Database structure.
    
- Instance = Actual data at a particular moment.
    
- External–Conceptual Mapping = Connects user views to logical structure.
    
- Conceptual–Internal Mapping = Connects logical structure to physical storage.