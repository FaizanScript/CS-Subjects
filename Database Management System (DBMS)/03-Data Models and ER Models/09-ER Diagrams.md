
An **ER Diagram (Entity-Relationship Diagram)** is a visual representation of entities, their attributes, and the relationships between them in a database.

It helps database designers understand and plan the database structure before implementing it using SQL.


## **1. Why do we need ER Diagrams?**

Imagine you're building a College Management System.

You need to store:

- Students
    
- Teachers
    
- Courses
    
- Departments
    
- Enrollments

Without a proper design, you might create unnecessary tables, duplicate data, or establish incorrect relationships.

An ER Diagram helps you:

1. Visualize the database structure.
    
2. Identify entities and their attributes.
    
3. Understand relationships between entities.
    
4. Define cardinality and participation constraints.
    
5. Convert the conceptual design into relational tables.

Think of an ER Diagram as a blueprint of your database.


## **2. ER Diagram Symbols and Notations**

This is the most important part of this topic. You should be able to recognize every symbol.

We'll use the traditional **Chen Notation**, which is commonly taught in DBMS textbooks.

### **A. Entity — Rectangle**

An entity is represented using a rectangle.

###### **Rectangle = Entity**

Examples: STUDENT, COURSE, EMPLOYEE, DEPARTMENT.

### **B. Weak Entity — Double Rectangle**

A weak entity is represented using a double rectangle.

###### **Double rectangle = Weak Entity**

Example: A dependent whose identity depends on an employee.

### **C. Attribute — Oval (Ellipse)**

An attribute is represented using an oval connected to its entity.

###### **Oval = Attribute**

Examples: name, age, email, salary.

### **D. Key Attribute — Underlined Oval Label**

A key attribute uniquely identifies an entity.

It is represented by underlining the attribute name.
###### **Underlined attribute = Key Attribute**

### **E. Composite Attribute — Connected Ovals**

A composite attribute is represented by a main oval connected to its component ovals.

Example: Name consists of first_name, middle_name and last_name.

###### **Main oval connected to smaller component ovals = Composite Attribute**

### **F. Multi-Valued Attribute — Double Oval**

An attribute that can contain multiple values for one entity is represented using a double oval.

Example: Phone numbers.

###### **Double oval = Multi-Valued Attribute**

### **G. Derived Attribute — Dashed Oval**

An attribute whose value is calculated from other attributes is represented using a dashed oval.

Example: Age derived from Date of Birth.

###### **Dashed oval = Derived Attribute**

### **H. Relationship — Diamond**

A relationship is represented using a diamond.

Example: Student enrolls in Course.

###### **Diamond = Relationship**

### **I. Identifying Relationship — Double Diamond**

The identifying relationship connects a weak entity to its owner strong entity.

It is represented using a double diamond.

###### **Double diamond = Identifying Relationship**

### **J. Participation — Single and Double Lines**

###### - **Single line = Partial participation.**

###### - **Double line = Total participation.**

We've already studied this in Topic 3.8.

**Single line — Partial**

**Double line — Total**


## **4. Complete ER Diagram Symbol Revision**

|Symbol|Meaning|
|---|---|
|Rectangle|Strong Entity|
|Double Rectangle|Weak Entity|
|Oval|Attribute|
|Underlined Attribute|Key Attribute|
|Double Oval|Multi-Valued Attribute|
|Dashed Oval|Derived Attribute|
|Diamond|Relationship|
|Double Diamond|Identifying Relationship|
|Single Line|Partial Participation|
|Double Line|Total Participation|
