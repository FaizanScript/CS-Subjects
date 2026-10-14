## 1.1 What is a Computer Network?

**Definition**: A computer network is a collection of two or more computing devices connected through communication links that exchange data and share resources using agreed-upon protocols.

In simple words, a computer network allows devices to communicate with each other.

**Examples:**
- Your laptop communicating with a web server.
    
- Computers in a college lab sharing files and a printer.
    
- Your smartphone communicating with your Wi-Fi router.
    
- A browser requesting data from a backend server.

## How It Works

- **Nodes:** End devices such as computers, servers, smartphones, or printers that send and receive data.

- **Links:** Physical cables (like Ethernet or optical fiber) or wireless signals (like Wi-Fi) that connect the nodes.

- **Protocols:** Set rules (such as TCP/IP) that dictate how data packets are formatted, addressed, transmitted, and received.

- **Network Devices:** Hardware like **switches** (which manage internal traffic within a local network) and **routers** (which direct data between different networks like the internet).

![[Pasted image 20261015021300.png]]

### Why do we need computer networks?

**The main purposes are:**

| Purpose                | Explanation                                    | Example                            |
| ---------------------- | ---------------------------------------------- | ---------------------------------- |
| Resource sharing       | Share hardware or software resources           | Multiple computers use one printer |
| Data sharing           | Exchange information between devices           | Send a file to another computer    |
| Communication          | Enable communication between users and systems | Email, messaging, video calls      |
| Remote access          | Access systems and services over a network     | Connect to a remote server         |
| Centralized management | Manage data and services centrally             | A company stores files on a server |
| Scalability            | Connect more devices and services as needed    | A website serves users worldwide   |

**Key takeaway:** Networks make communication and resource sharing possible across connected devices, whether those devices are in the same room or on different continents.


## 1.2 Basic components of a computer network

A network needs several components to communicate effectively.

### Client
(Requests data)

### Network
(Communication links and devices)

### Server
(Provides data or services)

Simplified client-server illustration. Real networks may contain multiple routers, switches, wireless links and servers.

### The important components are:

- **Sender:** The device that sends data.
    
- **Receiver:** The device that receives data.
    
- **Message:** The information being transmitted, such as text, images, audio or video.
    
- **Transmission medium:** The path through which signals travel, such as copper cable, optical fiber or radio waves.
    
- **Network interface:** Hardware or a virtual interface that enables a device to connect to a network.
    
- **Network devices**: Devices such as switches and routers that connect devices and forward traffic.
    
- **Protocols**: Rules that govern communication between devices.

A device can be a sender in one communication and a receiver in another.


## 1.3 What is a host?

**Definition**: A host is a device connected to a network that can send or receive data and, in the usual Internet Protocol context, is assigned network-layer addressing.

**Examples include:**
- A laptop.
- A smartphone.
- A web server.
- A virtual machine running an application.

A router is also a networked device, but its primary role is forwarding traffic between networks rather than acting as an ordinary end host.


## 1.4 What is a client and what is a server?

**Client**: A device or software application that requests a service or resource.

**Server**: A device or software application that provides a service or resource in response to requests.

**Example**: When you open a website, your browser acts as the client and requests a page from a web server.

Remember that client and server describe roles, not necessarily different kinds of physical computers. One computer can act as both a client and a server for different services.


## 1.5 Internet and WWW

### Q - What is Internet?

**Definition**: The Internet is a global network of interconnected networks that communicate using the Internet Protocol (IP) suite.

In simple words, the Internet connects millions of private, public, academic, business and government networks worldwide.

For example, when your browser requests a website hosted in another country, data travels across multiple interconnected networks to reach the destination.

### Q - What is WWW?

**Definition**: The **World Wide Web (WWW)** is a global system of interconnected web pages, documents, and multimedia content that you can access through the internet using a web browser

### Internet vs World Wide Web (WWW)

These terms are not interchangeable.

|Internet|World Wide Web|
|---|---|
|A global network infrastructure.|A system of interlinked web resources accessed over the Internet.|
|Supports many services and protocols.|Primarily uses HTTP/HTTPS for web communication.|
|Includes networking infrastructure and interconnected networks.|Includes websites, web pages, browsers and web servers.|
|The underlying network.|A service that runs on the network.|

Other Internet services include email, voice communication, file transfer and online gaming.

**Interview question:** Is the World Wide Web the same as the Internet?

**Answer:** No. The Internet is the underlying global network, whereas the World Wide Web is a service that operates over it.


## 1.6 Network architecture: Client-server vs Peer-to-peer

These are two common ways of organizing communication between devices.

### A. Client-server architecture

	Client1    Client 2    Client 3

		    Central server

**Definition**: A network architecture in which clients request services or resources from servers.

**Examples**: Web applications, online banking, and centralized file storage.

**Advantages**: Centralized management, easier access control, and convenient data management.

**Disadvantages**: Server failures can disrupt dependent clients, and the server may become a performance bottleneck.

### B. Peer-to-peer (P2P) architecture

### Peer A

### Peer B

### Peer C

**Definition**: A network architecture in which participating devices communicate and share resources directly, with each peer potentially acting as both a requester and a provider.

**Examples**: BitTorrent-style file sharing and some decentralized communication systems.

**Advantages**: Resources can be distributed among peers, potentially reducing dependence on a central server.

**Disadvantages**: Management, availability, security and coordination can be more difficult.