

## **1. What is a Domain?**

**A domain is the set of permitted atomic values that an attribute can contain.**

In simpler terms, a domain defines what kind of values a column is allowed to store.

Consider a Student table:

|Attribute|Domain|
|---|---|
|student_id|Positive integers|
|name|Character strings|
|age|Integers between 0 and 120|
|email|Valid email addresses, according to application rules|
|date_of_birth|Valid date values|

For example, the `age` attribute should not contain:

- `"Faizan"` — wrong value type.
    
- `-10` — outside the permitted range.
    
- `500` — outside the permitted range.

A domain helps maintain data validity and consistency.


## **2. What Defines a Domain?**

A domain can be understood through several properties:

1. **Data Type:** What kind of value is allowed?
    
2. **Permitted Values:** Which specific values are acceptable?
    
3. **Range:** What are the minimum and maximum values?
    
4. **Format:** What format should the value follow?
    
5. **NULL Rules:** Is the value allowed to be missing?

Let's understand each through SQL.


## **3. Data Types**

A data type restricts the kind of value a column can store.

Common PostgreSQL data types:

|Data Type|Purpose|Example|
|---|---|---|
|INT|Whole numbers|101|
|BIGINT|Large whole numbers|9876543210|
|DECIMAL(p,s)|Exact decimal numbers|45000.50|
|VARCHAR(n)|Variable-length strings|'Faizan'|
|CHAR(n)|Fixed-length strings|'A'|
|TEXT|Text of variable length|'Hello World'|
|DATE|Calendar date|'2006-06-27'|
|TIME|Time of day|'10:30:00'|
|TIMESTAMP|Date and time|'2026-11-23 10:30:00'|
|BOOLEAN|True or false|TRUE|

Example:

```
CREATE TABLE Student (
    student_id INT,
    name VARCHAR(100),
    age INT,
    date_of_birth DATE
);
```

Here:

- `student_id` accepts integers.
    
- `name` accepts character strings up to the specified length.
    
- `age` accepts integers.
    
- `date_of_birth` accepts date values.

**Important:** A data type restricts the kind of value, but it doesn't necessarily enforce all business rules.

For example, `INT` allows negative numbers. If age must be non-negative, we need another constraint.


## **4. CHECK Constraint**

**A CHECK constraint ensures that values inserted or updated in a column or row satisfy a specified Boolean condition.**

In simple terms, it lets us define additional validation rules.

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT CHECK (age >= 0)
);
```

Now:

|Value|Allowed?|Reason|
|---|---|---|
|18|Yes|Non-negative|
|0|Yes|Meets condition|
|-5|No|Violates condition|
|25|Yes|Meets condition|

The database checks the condition whenever a row is inserted or updated.

### **Multiple CHECK conditions**

Suppose students must be between 0 and 120 years old.

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT CHECK (age >= 0 AND age <= 120)
);
```

We can also write:

```
age INT CHECK (age BETWEEN 0 AND 120)
```

Both express the same range restriction.

### **CHECK on multiple columns**

A CHECK constraint can involve multiple columns.

Suppose an order has:

- `quantity`
    
- `unit_price`
    
- `discount`

We want the discount to be non-negative and not exceed the total price.

```
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    quantity INT,
    unit_price DECIMAL(10,2),
    discount DECIMAL(10,2),

    CHECK (quantity > 0),
    CHECK (unit_price >= 0),
    CHECK (discount >= 0),
    CHECK (discount <= quantity * unit_price)
);
```

Notice that the last CHECK involves multiple columns.

This is useful for enforcing business rules directly inside the database.


## **5. Restricting Values Using IN**

Sometimes a column should accept only a predefined set of values.

For example, suppose a student's status can only be:

- active
    
- inactive
    
- graduated

We can enforce this using `IN`.

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),

    status VARCHAR(20)
        CHECK (status IN ('active', 'inactive', 'graduated'))
);
```

Now the following values are valid:

|Status|Allowed?|
|---|---|
|active|Yes|
|inactive|Yes|
|graduated|Yes|
|suspended|No|

This is useful when an attribute has a fixed set of permitted values.


## **6. NOT NULL Constraint**

**NOT NULL ensures that a column must always contain a non-NULL value.**

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

Here:

- `student_id` cannot be NULL because it's a primary key.
    
- `name` cannot be NULL.
    
- `email` cannot be NULL.

However, an empty string `''` is not the same as NULL.

For example:

```
INSERT INTO Student
VALUES (101, '', 'faizan@example.com');
```

This satisfies the NOT NULL constraint on `name` because an empty string is still a non-NULL value.

If empty strings must also be prohibited, we need an additional CHECK constraint.

```
name VARCHAR(100) NOT NULL CHECK (length(trim(name)) > 0)
```


## **7. DEFAULT Constraint**

**DEFAULT specifies a value that the database uses when an INSERT statement does not provide a value for that column.**

Example:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    status VARCHAR(20) DEFAULT 'active'
);
```

Now:

```
INSERT INTO Student (student_id, name)
VALUES (101, 'Faizan');
```

The database automatically assigns:

|student_id|name|status|
|---|---|---|
|101|Faizan|active|

Important distinctions:

- DEFAULT does not automatically mean NOT NULL.
    
- If you explicitly insert NULL, DEFAULT is generally not applied.
    
- NOT NULL and DEFAULT can be used together.

For example:

```
status VARCHAR(20) NOT NULL DEFAULT 'active'
```

This means:

- If no status is provided, use `active`.
    
- If NULL is explicitly provided, reject the insertion.


## **8. SQL DOMAIN — A Separate Database Feature**

There is another concept worth learning: SQL provides a `CREATE DOMAIN` statement.

A SQL DOMAIN creates a reusable, named set of restrictions based on an existing data type.

Instead of repeatedly writing the same CHECK condition in multiple tables, we can define it once.

Example:

```
CREATE DOMAIN positive_age AS INT
CHECK (VALUE >= 0 AND VALUE <= 120);
```

Now we can reuse this domain:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    age positive_age
);

CREATE TABLE Teacher (
    teacher_id INT PRIMARY KEY,
    name VARCHAR(100),
    age positive_age
);
```

Both tables use the same domain definition.

If we later change the domain's constraint, it can affect every column using that domain.

**Remember the distinction:**

- Domain in relational theory: the permitted set of values for an attribute.
    
- `CREATE DOMAIN` in SQL: a named, reusable database object that defines a data type and associated constraints.


## **9. Important: CHECK Constraints and NULL**

This is a subtle but important concept.

Consider:

```
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    age INT CHECK (age >= 18)
);
```

Would this allow age to be NULL?

Yes.

Why?

Because SQL uses three-valued logic:

- TRUE
    
- FALSE
    
- UNKNOWN

When `age` is NULL:

```
NULL >= 18
```

The result is UNKNOWN.

In PostgreSQL, a CHECK constraint rejects a row only when its condition evaluates to FALSE. TRUE and UNKNOWN pass.

Therefore, the following insertion is permitted:

```
INSERT INTO Student (student_id, age)
VALUES (101, NULL);
```

If age is mandatory, use:

```
age INT NOT NULL CHECK (age >= 18)
```

This distinction is extremely important when designing database schemas.


## **10. Quick Comparison**

|Constraint|Purpose|
|---|---|
|Data Type|Restricts the kind of value|
|CHECK|Enforces a specified condition|
|NOT NULL|Prohibits NULL|
|DEFAULT|Supplies a value when omitted|
|UNIQUE|Prevents duplicate non-NULL values under the DBMS's NULL rules|
|PRIMARY KEY|Enforces uniqueness and NOT NULL|
|FOREIGN KEY|Maintains valid references|
