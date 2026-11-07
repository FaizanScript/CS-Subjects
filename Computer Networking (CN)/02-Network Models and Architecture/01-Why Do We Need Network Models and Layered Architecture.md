
**Objective**: Understand why computer networks use layered architecture, what problems it solves, how layers communicate, and why the OSI and TCP/IP models were introduced.

Before studying the OSI model, you need to understand the problem it was designed to solve. Otherwise, you'll just memorize seven layer names without understanding why they exist.



## 1. The Problem: How Do Different Devices Communicate?

Imagine you have a laptop running Windows, and you want to communicate with a server running Linux.

Your laptop and the server may have:

- Different operating systems.
    
- Different hardware.
    
- Different network interfaces.
    
- Different applications.
    
- Different communication technologies.

Yet, they need to exchange data successfully.

Consider what must happen when you open a website:

1. Your browser needs to prepare an HTTP request.
    
2. The data needs to be transported between devices.
    
3. The destination must be identified.
    
4. The data must travel through one or more networks.
    
5. The receiver must understand the incoming data.
    
6. The response must travel back to your browser.

These are different responsibilities.

If every device and application had to independently implement all networking responsibilities in one massive system, networking would become extremely complicated.

We need a structured approach.

That's where network models and layered architecture come in.


## 2. What Is a Network Model?

**Definition**: A network model is a conceptual framework that organizes network communication into different layers, with each layer responsible for specific networking functions.

In simple terms, it divides the complex process of network communication into smaller, manageable parts.

Each layer has:

- Specific responsibilities.
    
- Defined functions.
    
- Rules for interacting with other layers.
    
- Interfaces through which it uses services from adjacent layers.

Two important network models are:

1. **OSI Model** — Open Systems Interconnection model.
    
2. **TCP/IP Model** — Transmission Control Protocol/Internet Protocol model.

We'll study both in detail in upcoming topics.


## 3. What Is Layered Architecture?

**Definition**: Layered architecture is a networking design approach in which communication responsibilities are divided into multiple layers, with each layer performing a specific set of functions.

Think of sending a physical parcel to someone.

The process involves different responsibilities:

1. Packaging the item.
    
2. Labelling the parcel with the destination.
    
3. Transporting it.
    
4. Sorting it at distribution centers.
    
5. Delivering it to the receiver.
    
6. Unpacking it.

Each stage has a different responsibility, but all stages work together to complete the delivery.

Networking follows a similar idea.

Instead of one component handling everything, different layers handle different parts of communication.



**Important**: Layers are logical divisions of responsibilities. They don't necessarily represent separate physical devices or independent programs.


## 4. Why Do We Need Layered Architecture?

There are several important reasons.

### 1. Reduces Complexity

Network communication involves many different tasks.

Layering divides those tasks into smaller sections, making the overall system easier to understand and manage.

For example:

- One layer handles application communication.
    
- Another handles transport.
    
- Another handles logical addressing and routing.
    
- Another handles local network delivery.

Instead of understanding the entire networking system at once, you can study each responsibility separately.

### 2. Modularity

Each layer has its own responsibilities.

This makes the networking system modular.

A change in one layer does not necessarily require changes in every other layer, provided the interfaces and expected services remain compatible.

Example:

Suppose your computer switches from a wired Ethernet connection to Wi-Fi.

Your browser doesn't need to be rewritten.

HTTP can continue working because the lower networking layers provide the necessary communication services.

### 3. Standardization

Network models help define common responsibilities and interfaces.

This allows different manufacturers and developers to build compatible networking technologies.

For example, a Windows laptop can communicate with a Linux server because both systems implement compatible networking protocols.

### 4. Interoperability

**Definition**: Interoperability is the ability of different devices, systems, and technologies to communicate and work together.

Without common networking standards and protocols, devices from different manufacturers might not understand each other's communication.

Layered architecture helps organize the standards and protocols required for interoperability.

### 5. Easier Troubleshooting

Because networking responsibilities are separated, you can investigate problems layer by layer.

Example:

Suppose you cannot access a website.

You can investigate:

- Is the network cable or Wi-Fi connection working?
    
- Does your device have a valid IP configuration?
    
- Is DNS resolving the domain name?
    
- Can you establish a transport connection?
    
- Is the web server responding?

Instead of randomly checking everything, you can narrow down the problem.

### 6. Flexibility and Scalability

Layered architecture makes it easier to introduce new technologies and improve existing ones without redesigning the entire networking system.

For example, newer Wi-Fi standards can improve wireless communication while applications continue using familiar protocols such as HTTP.


## 5. How Do Layers Work Together?

This is one of the most important concepts in network architecture.

Let's understand it through a practical example.

Suppose you open a website from your browser.

Your browser wants to send an HTTP request to a web server.

The request passes through different layers before travelling across the network.
### At the sender's side

Each layer performs its own responsibility.

1. **The application layer prepares the application data.**
    
2. **The transport layer provides end-to-end transport services.**
    
3. **The network layer handles logical addressing and routing.**
    
4. **The data link layer prepares data for transmission over the local link.**
    
5. **The physical layer transmits the bits through the communication medium.**

### At the receiver's side

The reverse process happens.

1. **The physical layer receives bits.**
    
2. **The data link layer processes the incoming frame.**
    
3. **The network layer processes the IP packet.**
    
4. **The transport layer processes the transport segment or datagram.**
    
5. **The application layer receives the application data.**

The receiving device processes the information using the corresponding networking layers.

We'll study this process in detail when we cover encapsulation and decapsulation.


## 6. Protocols and Layers: What Is the Relationship?

You already learned about network protocols in Phase 1.

Now connect that knowledge with layered architecture.

A layer defines responsibilities, while protocols define the rules used to perform communication-related tasks.

For example:

|Layer / Responsibility|Example protocols or technologies|
|---|---|
|Application|HTTP, DNS, SMTP|
|Transport|TCP, UDP|
|Network|IP, ICMP|
|Data Link|Ethernet, Wi-Fi|
|Physical|Ethernet physical specifications, Wi-Fi radio specifications|

These are simplified groupings. The exact placement of some technologies depends on the model and how their functions are classified.

Important distinction:

- **Network layer:** A conceptual division of networking responsibilities.
    
- **Network protocol:** A set of communication rules.
    
- **Network model:** A framework that organizes those responsibilities into layers.

Don't confuse these three terms.


## 7. What Is Peer-to-Peer Communication Between Layers?

In layered architecture, corresponding layers on two different devices are often described as communicating with each other logically.

For example:

- The application layer on your computer logically communicates with the application layer on the server.
    
- The transport layer on your computer logically communicates with the transport layer on the server.
    
- The network layer on your computer logically communicates with the network layer on the server.

However, the data doesn't physically jump directly between corresponding layers.

It travels down through the sender's networking stack, across the network, and up through the receiver's networking stack.

Remember: Peer-to-peer communication is a logical relationship between corresponding layers, not necessarily a direct physical connection.


## 8. Advantages and Limitations of Layered Architecture

### Advantages

- Reduces networking complexity.
    
- Makes development and maintenance easier.
    
- Encourages standardization.
    
- Supports interoperability.
    
- Makes troubleshooting more systematic.
    
- Allows individual technologies to evolve independently within defined interfaces.

### Limitations

- Each layer can introduce processing overhead.
    
- Headers and control information consume additional bandwidth.
    
- Strict separation can sometimes be inefficient.
    
- Some networking functions overlap across layers.
    
- Real-world protocols don't always fit perfectly into a theoretical model.

Important: Layered architecture is a design approach, not a rule that every real-world networking technology must follow perfectly.


## 9. Quick Revision

| Term                       | Meaning                                                   |
| -------------------------- | --------------------------------------------------------- |
| Network model              | Framework organizing network communication into layers    |
| Layered architecture       | Dividing networking responsibilities into separate layers |
| Modularity                 | Separating a system into manageable components            |
| Standardization            | Establishing common technical specifications              |
| Interoperability           | Different systems working together                        |
| Peer-to-peer communication | Logical communication between corresponding layers        |
| Protocol                   | Rules governing communication                             |

### The central idea

Why layered architecture?

Complex network communication is divided into smaller responsibilities.

Each layer performs specific functions, uses services from lower layers, and provides services to higher layers.

Together, the layers enable communication between different devices and applications.