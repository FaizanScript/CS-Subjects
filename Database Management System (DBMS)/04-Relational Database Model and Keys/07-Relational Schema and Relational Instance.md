

## **1. What is a Relational Schema?**

A **Relational Schema** is the logical structure or blueprint of a relation (table). It defines the relation's name, attributes (columns), and associated constraints.

In simple terms, it describes **how a table is designed, not the actual records stored inside it.**

For example:

```
Student(student_id, name, email, age)
```

This is a relational schema.

It tells us:

- Relation name: `Student`
    
- Attributes: `student_id`, `name`, `email`, `age`
    
- It describes the structure of the Student relation.

A more detailed schema can also specify data types and constraints:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 0)
);
```

Here, we define the structure and rules of the Student table.

### **Important distinction**

A schema does not normally change every time we insert, update or delete a record.

For example, inserting a new student changes the data, not the table's structure.


## **2. What is a Relational Instance?**

A **Relational Instance** is the actual set of tuples (rows) stored in a relation at a particular point in time.

In simple terms, it represents **the actual data currently present in a table.**

Consider the Student relation:

|student_id|name|email|age|
|---|---|---|---|
|101|Rahul|rahul@gmail.com|20|
|102|Aman|aman@gmail.com|21|
|103|Priya|priya@gmail.com|19|

This table represents a relational instance.

It contains the actual records stored in the Student relation at that moment.

If we insert another student, the relational instance changes.

```
INSERT INTO Student
VALUES (104, 'Rohan', 'rohan@gmail.com', 22);
```

Now the relation contains four tuples instead of three.

**The schema remains the same, but the instance changes.**


## **3. Schema vs Instance**

This is the central distinction of this topic.

|Basis|Relational Schema|Relational Instance|
|---|---|---|
|Meaning|Structure or blueprint of a relation|Actual data stored in a relation|
|Defines|Relation name, attributes, constraints|Tuples or records|
|Changes|Relatively infrequently|Frequently|
|Nature|Relatively static|Dynamic|
|Example|`Student(id, name, age)`|Rows containing student details|
|Analogy|Design of a form|Filled-in form|

**Visual understanding**

**==🔵Schema==**
**Student(student****_****id, name, age)**
Defines the table's structure

	Stores actual records

**==🟢Instance==**

|student_id|name|age|
|---|---|---|
|101|Rahul|20|
|102|Aman|21|
|103|Priya|19|

Think of it this way:

- Schema = Blueprint of a house.
    
- Instance = The actual house built using that blueprint.

The blueprint defines the structure; the actual house is the result.


## **4. Degree and Cardinality of a Relation**

Two important terms describe the structure and size of a relation.

### **A. Degree (Arity)**

**Degree** is the total number of attributes (columns) in a relation.

Example:

|student_id|name|age|email|
|---|---|---|---|
|101|Rahul|20|rahul@gmail.com|
|102|Aman|21|aman@gmail.com|

Degree = 4.

Because the relation has four attributes:

1. student_id
    
2. name
    
3. age
    
4. email

### **B. Cardinality**

**Cardinality** is the total number of tuples (rows) in a relation at a particular time.

Using the same example:

Cardinality = 2.

Because the relation contains two records.

### **Degree vs Cardinality**

|Degree|Cardinality|
|---|---|
|Number of columns|Number of rows|
|Describes attributes|Describes tuples|
|Usually changes through structural modification|Changes through INSERT and DELETE operations|

Exam tip

Degree = Columns. Cardinality = Rows.


## **5. How Schema and Instance Change**

Let's understand this through SQL operations.

Assume we have:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);
```

The schema is:

```
Student(student_id, name, age)
```

### **Case 1: INSERT**

```
INSERT INTO Student
VALUES (101, 'Rahul', 20);
```

- Schema changes? No.
    
- Instance changes? Yes.

A new tuple is added.

### **Case 2: UPDATE**

```
UPDATE Student
SET age = 21
WHERE student_id = 101;
```

- Schema changes? No.
    
- Instance changes? Yes.

The existing tuple is modified.

### **Case 3: DELETE**

```
DELETE FROM Student
WHERE student_id = 101;
```

- Schema changes? No.
    
- Instance changes? Yes.

A tuple is removed.

### **Case 4: ALTER TABLE**

```
ALTER TABLE Student
ADD COLUMN email VARCHAR(100);
```

- Schema changes? Yes.
    
- Instance? Its structure changes; existing rows now have an email attribute, initially NULL unless a default or other rule applies.

This is a structural modification.


## **6. Relation, Relation Schema and Relation Instance**

Let's connect all three terms.

|Term|Meaning|
|---|---|
|Relation|A table in the relational model|
|Relation Schema|The definition or structure of that relation|
|Relation Instance|The actual tuples present in that relation at a particular time|

For example:

```
Relation: Student

Schema:
Student(student_id, name, age)

Instance:
(101, Rahul, 20)
(102, Aman, 21)
(103, Priya, 19)
```

One more important point: In the formal relational model, a relation is a set of tuples, so duplicate tuples are not allowed and row order has no logical significance. SQL tables, however, can contain duplicate rows unless constraints prevent them, and query results have no guaranteed order without `ORDER BY`.


## **7. Quick Revision**

- Relational Schema = Structure of a relation.
    
- Relational Instance = Actual data at a particular time.
    
- Schema is relatively stable; instance changes frequently.
    
- INSERT, UPDATE and DELETE modify the instance.
    
- ALTER TABLE modifies the schema.
    
- Degree = Number of attributes (columns).
    
- Cardinality = Number of tuples (rows).