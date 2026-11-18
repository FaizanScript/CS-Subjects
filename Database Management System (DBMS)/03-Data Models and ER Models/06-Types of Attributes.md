
In the previous topic, we learned that an **attribute is a property or characteristic that describes an entity**.

For example, consider a `STUDENT` entity:

- student_id
    
- name
    
- age
    
- email
    
- phone_number
    
- date_of_birth

However, not all attributes behave in the same way.

Some attributes contain a single value, some contain multiple values, some can be divided into smaller attributes, and some can be calculated from other attributes.

Let's understand each type.


## **1. Simple Attribute (Atomic Attribute)**

A **Simple Attribute** is an attribute that cannot be meaningfully divided into smaller sub-attributes.

Examples:

- age
    
- salary
    
- gender
    
- student_id

For example:

|student_id|age|
|---|---|
|101|20|
|102|21|

Here, `age` and `student_id` are simple attributes.

**Important:** Simple attributes are also called atomic attributes because they represent indivisible values in the context of the database design.


## **2. Composite Attribute**

A **Composite Attribute** is an attribute that can be divided into multiple smaller sub-attributes, each having its own meaning.

Example: `name`

A person's full name can be divided into:

- first_name
    
- middle_name
    
- last_name

Similarly, an `address` can be divided into:

- house_number
    
- street
    
- city
    
- state
    
- pin_code

**Composite Attribute: Name**

			    NAME
			    
First Name         Middle Name           Last Name

**Practical example:**

Instead of storing the entire address as one field:

```
address = "Hazaribagh, Jharkhand, India"
```

We might store it as separate fields:

```
city = "Hazaribagh"
state = "Jharkhand"
country = "India"
```

This makes it easier to search, filter and organize data based on individual components.

**Remember:** A composite attribute is one logical attribute that consists of multiple meaningful sub-attributes.


## **3. Single-Valued Attribute**

A **Single-Valued Attribute** is an attribute that holds only one value for a particular entity at a given time.

Examples:

- date_of_birth
    
- student_id
    
- blood_group

Consider:

|student_id|date_of_birth|
|---|---|
|101|27-06-2006|
|102|15-03-2005|

Each student has one `student_id` and one `date_of_birth`.

Therefore, these are single-valued attributes.


## **4. Multi-Valued Attribute**

A **Multi-Valued Attribute** is an attribute that can have multiple values for a single entity.

Example: A student can have multiple phone numbers.

|student_id|phone_numbers|
|---|---|
|101|9876543210, 9123456780|
|102|9988776655|

Here, `phone_numbers` is a multi-valued attribute because one student can have multiple phone numbers.

Other examples:

- Skills of an employee
    
- Email addresses of a person
    
- Languages spoken by a person
    
- Degrees obtained by a person


### **How is it represented in an ER Diagram?**

A multi-valued attribute is represented using a **double oval**.

**Double oval = Multi-valued attribute**


### **How do we store it in a relational database?**

In a relational database, we generally don't store multiple phone numbers in one column. Instead, we create a separate table.

**Students table**

|student_id|name|
|---|---|
|101|Faizan|
|102|Aman|

**Student****_****Phones table**

|student_id|phone_number|
|---|---|
|101|9876543210|
|101|9123456780|
|102|9988776655|

This design allows each student to have multiple phone numbers while keeping each phone number in its own row.

This is also connected to the concept of normalization, which we'll study later.


## **5. Derived Attribute**

A **Derived Attribute** is an attribute whose value can be calculated or obtained from another attribute or set of attributes.

Example: `age`

We can calculate a person's age using their `date_of_birth`.

Suppose:

```
date_of_birth = 27-06-2006
age = calculated using date_of_birth
```

Here, `age` is a derived attribute because it can be calculated from the date of birth.

Other examples:

|Derived Attribute|Derived From|
|---|---|
|Age|Date of birth|
|Total price|Quantity × Unit price|
|Remaining balance|Total amount − Paid amount|
|Years of service|Joining date|


### **Representation in an ER Diagram**

A derived attribute is represented using a **dashed oval**.

**Dashed oval = Derived attribute**

**Important:** Derived attributes do not always need to be physically stored in the database. They can be calculated when required.

For example, in PostgreSQL, you can calculate age using a date of birth:

```
SELECT
    AGE(CURRENT_DATE, date_of_birth)
FROM Students;
```

This calculates the age interval from the stored date of birth.


## **6. Stored Attribute**

A **Stored Attribute** is an attribute whose value is directly stored in the database rather than calculated from another attribute whenever it is needed.

Example:

`date_of_birth` is stored, and `age` can be derived from it.

**Stored vs Derived**

   **Date of Birth**                                       **Age**
    Stored                                          Derived

Date of birth is stored; age is calculated from it.

**One important distinction:** Stored and derived attributes are related concepts, but an attribute can be stored even if it could theoretically be calculated from other information. The design decision depends on requirements, performance and data consistency.


## **7. Key Attribute**

A **Key Attribute** is an attribute whose value uniquely identifies an entity within an entity set.

Example: `student_id`

|student_id|name|
|---|---|
|101|Faizan|
|102|Aman|
|103|Ravi|

Here, `student_id` uniquely identifies each student.

**In an ER diagram, a key attribute is represented by underlining its name.**

**Underlined attribute = Key attribute**

We'll connect this to primary keys and candidate keys in our upcoming relational database topics.


## **8. Quick comparison of all attribute types**

| Attribute Type | Meaning                            | Example       |
| -------------- | ---------------------------------- | ------------- |
| Simple         | Cannot be meaningfully divided     | Age           |
| Composite      | Can be divided into sub-attributes | Name          |
| Single-valued  | One value per entity               | Date of birth |
| Multi-valued   | Multiple values per entity         | Phone numbers |
| Derived        | Calculated from other attributes   | Age           |
| Stored         | Directly stored in DB              | Date of birth |
| Key            | Uniquely identifies an entity      | Student ID    |

**Important conceptual distinction:**

These categories describe different properties of attributes. They are not mutually exclusive.

For example:

- `student_id` can be simple, single-valued and a key attribute.
    
- `date_of_birth` can be simple, single-valued and stored.
    
- `age` can be simple, single-valued and derived.
    
- `phone_numbers` can be multi-valued.

Don't memorize the table without understanding these distinctions.