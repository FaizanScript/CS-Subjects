

### 1. What is DBMS Architecture?

DBMS Architecture describes how users, applications and database systems are organized and how they communicate with one another to access and manage data.

In simple terms, it explains where the application and database are located and how they interact.

There are three commonly discussed architectures:

1. 1-Tier Architecture
    
2. 2-Tier Architecture
    
3. 3-Tier Architecture

Before studying them, understand one important distinction:

Three-Schema Architecture ≠ 1-Tier, 2-Tier and 3-Tier Architecture.

|Three-Schema Architecture|Tier Architecture|
|---|---|
|Explains levels of database abstraction.|Explains how application components are organized.|
|External, Conceptual, Internal.|1-Tier, 2-Tier, 3-Tier.|
|Focuses on database structure.|Focuses on system communication and deployment.|


### 2. 1-Tier Architecture

**Definition**: In 1-Tier Architecture, the user, application and database are present on the same system, and the user interacts directly with the database system.

How it works:

1. The user works directly with the application or database interface.
    
2. The application communicates with the local DBMS.
    
3. The DBMS processes database operations.
    
4. Results are returned to the user.

Example:

Suppose you install SQLite and a database management application on your laptop.

- User: You.
    
- Application: Database management tool.
    
- DBMS: SQLite.
    
- Database: Local database file.

Everything operates on the same machine.

**Characteristics:**

- All major components are on one machine.
    
- No separate application server is required.
    
- Simple to develop and use.
    
- Suitable for local development, testing and standalone applications.
    
- Not ideal for large multi-user applications.


### 3. 2-Tier Architecture

**Definition**: In 2-Tier Architecture, the client application communicates directly with the database server.

It follows a Client–Server model.

There are two tiers:

**Tier 1 — Client**

- User interface.
    
- Application logic (at least some of it).
    
- Sends database queries to the database server.

**Tier 2 — Database Server**

- DBMS.
    
- Database.
    
- Processes queries and returns results.

How it works:

1. The user interacts with the client application.
    
2. The client sends a database request directly to the database server.
    
3. The DBMS processes the request.
    
4. The database server returns the result to the client.

Example:

Imagine a desktop college management application installed on computers in a college laboratory.

- Client: College management software.
    
- Database server: MySQL server.
    
- Communication: Client directly connects to MySQL.

**Characteristics:**

- Client directly communicates with the database server.
    
- Usually uses a database driver or connector.
    
- Supports multiple clients.
    
- Easier to build than a 3-tier system.
    
- Exposing database access directly to clients creates security and scalability concerns.


### 4. 3-Tier Architecture

Widely used in modern web applications

**Definition**: In 3-Tier Architecture, the application is divided into three separate layers: Presentation Layer, Application Layer and Database Layer.

Let's understand each one.

**Tier 1 — Presentation Layer (Frontend)**

This is the user-facing part of the application.

**Responsibilities:**

- Displays information to users.
    
- Collects user input.
    
- Sends requests to the application server.
    
- Displays responses.

Examples: HTML, CSS, JavaScript, React.

**Tier 2 — Application Layer (Backend)**

This layer contains the application's business logic and acts as an intermediary between the frontend and database.

**Responsibilities:**

- Processes client requests.
    
- Validates user input.
    
- Handles authentication and authorization.
    
- Executes business logic.
    
- Communicates with the database.
    
- Sends responses to the frontend.

Examples: Node.js, Express.js, Java Spring Boot.

**Tier 3 — Database Layer**

This layer stores and manages application data.

**Responsibilities:**

- Stores data.
    
- Retrieves and updates data.
    
- Manages database operations.
    
- Maintains data integrity and security.

Examples: PostgreSQL, MySQL, MongoDB.


### 5. Practical Example: Login System

Let's understand the complete architecture using a login system.

				Tier 1
			Presentation Layer
   User enters email and password in a React application.

			   HTTP request

				Tier 2
			Application Layer
		    Node.js + Express.js
   Validates credentials and checks the user's account.

			   Database query

				Tier 3
			  Database Layer
			   PostgreSQL
   Retrieves the user's stored account information.

	Query result → Backend response → Frontend

The response travels back through the application layer to the presentation layer.

**The actual process:**

1. User enters email and password in the React frontend.
    
2. React sends an HTTP request to the Express backend.
    
3. Express validates the input and queries PostgreSQL.
    
4. PostgreSQL returns the relevant account data.
    
5. Express verifies the password securely and generates an appropriate response.
    
6. React displays the result to the user.

Notice something important: The frontend does not directly communicate with the database. The backend handles database communication.

This separation helps with security, maintainability and scalability.


### 6. Comparison: 1-Tier vs 2-Tier vs 3-Tier

|Feature|1-Tier|2-Tier|3-Tier|
|---|---|---|---|
|Components|Same system|Client + DB server|Frontend + Backend + DB|
|Communication|Local|Client ↔ DB|Frontend ↔ Backend ↔ DB|
|Complexity|Low|Medium|Higher|
|Scalability|Limited|Moderate|Better|
|Security separation|Limited|Less separation|Better separation|
|Common use|Local apps|Desktop client-server apps|Web applications|


### 7. Important Interview Points

1. Which architecture is commonly used in modern web applications? 3-Tier Architecture.
    
2. Does the frontend directly access the database in 3-Tier Architecture? No, it communicates through the application layer.
    
3. What is the middle tier? Application or business logic layer.
    
4. What architecture follows the client-server model? 2-Tier Architecture is a direct client-server arrangement; 3-Tier also commonly uses client-server communication.
    
5. Is 3-Tier always better? No. It introduces additional complexity and isn't necessary for every application.


## Quick Revision

- 1-Tier: User, application and database operate on the same system.
    
- 2-Tier: Client communicates directly with the database server.
    
- 3-Tier: Frontend communicates with backend, and backend communicates with database.
    
- Presentation Layer: User interface.
    
- Application Layer: Business logic.
    
- Database Layer: Data storage and management.