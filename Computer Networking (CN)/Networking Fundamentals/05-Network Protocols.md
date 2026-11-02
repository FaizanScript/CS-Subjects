
## 1. What is a Network Protocol?

A network protocol is a set of rules and standards that define how devices communicate and exchange data over a network.

These rules specify:

- How data should be formatted.
    
- How devices identify each other.
    
- How data should be transmitted and received.
    
- How errors should be handled.
    
- How communication should begin and end.

Example:

Imagine you want to send a message to someone who speaks only English, but you communicate only in Hindi. You both need a common language to understand each other.

Network protocols serve a similar purpose: they establish common communication rules so different devices and systems can communicate.

Technical example: When you open `https://google.com`, your browser and Google's server use several protocols, including DNS, IP, TCP or QUIC, TLS, and HTTP.


## 2. Why Do We Need Network Protocols?

Without protocols, devices wouldn't have a common way to communicate.

Consider two computers trying to exchange data:

- How would the sender identify the receiver?
    
- How would the receiver know where a message starts and ends?
    
- How would it interpret the received data?
    
- What happens if data gets corrupted or lost?
    
- How would both devices coordinate communication?

Protocols solve these problems.

Main purposes:

1. **Data formatting:** Define how information is structured.
    
2. **Addressing:** Identify devices and destinations.
    
3. **Data transmission:** Define how data travels across networks.
    
4. **Error handling:** Detect or recover from communication errors.
    
5. **Flow control:** Prevent a sender from overwhelming a receiver.
    
6. **Connection management:** Establish, maintain, or terminate communication where required.
    
7. **Security:** Support authentication, encryption, and data integrity.

Not every protocol handles all these responsibilities. Different protocols have different jobs.


## 3. Three Key Elements of a Protocol

A protocol's rules can be understood through three fundamental elements:

### A. Syntax

**Defines the format and structure of data.**

It specifies:

- What fields are included.
    
- The order of those fields.
    
- How many bits or bytes each field occupies.

Example: An IP packet contains fields such as source IP address, destination IP address, and other control information.

### B. Semantics

**Defines the meaning of each field and what action should be taken.**

Example: In a TCP header, the SYN flag indicates that a device wants to initiate a TCP connection.

### C. Timing

**Defines when data should be transmitted and at what rate, including coordination and synchronization requirements.**

Example: A sender may need to wait for an acknowledgment before transmitting more data under a particular flow-control mechanism.

|Element|Main question|
|---|---|
|Syntax|What is the format of the data?|
|Semantics|What does the data mean?|
|Timing|When and how fast should data be transmitted?|

**Remember**: Syntax, semantics, and timing are the three commonly taught characteristics of network protocols.


## 4. Protocols vs. Standards

These two terms are related, but they are not the same.

**Network protocol:** A set of rules governing communication between devices.

**Network standard:** A formally documented specification that defines how a technology or protocol should work so different implementations can interoperate.

For example:

- HTTP is an application-layer protocol.
    
- HTTP specifications are published as standards documents, primarily through the IETF.

### Important standardization organizations

|Organization|Main responsibility|
|---|---|
|ISO|Develops international standards, including the OSI reference model.|
|IEEE|Develops networking standards such as Ethernet (802.3) and Wi-Fi (802.11).|
|IETF|Develops Internet standards and protocols such as TCP, IP, DNS, and HTTP.|
|ITU-T|Develops telecommunications standards.|
|W3C|Develops web standards and specifications, such as HTML and CSS.|

You don't need to memorize every organization immediately. Remember the major ones and what they do.


## 5. Protocols Are Organized Into Layers

Network communication involves several responsibilities. Instead of making one protocol handle everything, networking uses multiple protocols that work together.

These protocols are organized into layers.

For example, when you visit a website:

- HTTP handles the web request and response.
    
- TLS provides security for HTTPS.
    
- TCP provides reliable transport for HTTP/1.1 and HTTP/2.
    
- IP handles addressing and routing between networks.
    
- Ethernet or Wi-Fi carries data across the local network.

This is an example of a protocol stack: a collection of protocols working together to enable communication.

We'll study the OSI and TCP/IP models in Phase 2. For now, understand that protocols operate at different layers and cooperate with one another.


## 6. Important Network Protocols

Let's categorize the protocols you'll encounter frequently.

### A. Application Layer Protocols

These protocols provide network services directly to applications:-

### 1. HTTP — Hypertext Transfer Protocol

Used for communication between web browsers and web servers. It defines how clients send requests and how servers return responses.

Default port: 80 (commonly over TCP for HTTP/1.1 and HTTP/2).

### 2. HTTPS — Hypertext Transfer Protocol Secure

HTTP secured using TLS. It provides encryption, authentication of the server, and data integrity.

Default port: 443. HTTP/3 uses QUIC over UDP.

### 3. DNS — Domain Name System

Translates human-readable domain names, such as `google.com`, into IP addresses so devices can locate servers.

Default port: 53 (usually UDP; TCP is also used).

### 4. DHCP — Dynamic Host Configuration Protocol

Automatically provides network configuration to devices, such as an IP address, subnet mask, default gateway, and DNS server.

UDP ports: 67 (server), 68 (client).

### 5. FTP — File Transfer Protocol

Used to transfer files between a client and a server.

TCP ports: 20 and 21 in traditional active-mode FTP. FTP is not encrypted by default.

### 6. SMTP — Simple Mail Transfer Protocol

Used to send emails from clients to mail servers and between mail servers.

Common TCP ports: 25 (server-to-server), 587 (message submission), 465 (implicit TLS submission).

### 7. IMAP and POP3

Both are used to retrieve emails.

- IMAP: Keeps email synchronized with the mail server across devices.
    
- POP3: Typically downloads messages to a client, with server retention depending on configuration.

IMAP: TCP 143 / 993 (TLS). POP3: TCP 110 / 995 (TLS).

### 8. SSH — Secure Shell

Provides secure remote access to another computer. Commonly used by developers and system administrators to manage remote servers.

Default TCP port: 22.

For your development journey: HTTP, HTTPS, DNS, DHCP, and SSH deserve particular attention. You'll encounter them frequently in web development and deployment.


### B. Transport Layer Protocols

These protocols handle communication between applications running on different devices.

### 1. TCP — Transmission Control Protocol

TCP is a connection-oriented transport protocol that provides reliable, ordered data delivery.

Key characteristics:

- Establishes a connection before normal data transfer.
    
- Uses acknowledgments and retransmissions.
    
- Maintains data order.
    
- Provides flow control and congestion control.

Example: Commonly used by HTTP/1.1 and HTTP/2.

### 2. UDP — User Datagram Protocol

UDP is a connectionless transport protocol that sends datagrams without guaranteeing delivery, ordering, or retransmission.

Key characteristics:

- No connection establishment like TCP.
    
- Lower protocol overhead.
    
- No built-in reliability or ordering guarantee.
    
- Useful when low latency matters.

Example: Real-time gaming, voice/video communication, and DNS queries commonly use UDP.

|TCP|UDP|
|---|---|
|Connection-oriented|Connectionless|
|Reliable delivery|No delivery guarantee|
|Ordered delivery|No ordering guarantee|
|More protocol overhead|Less protocol overhead|
|Flow and congestion control|No built-in TCP-style flow/congestion control|

We'll study TCP and UDP in depth in Phase 5.


### C. Internet / Network Layer Protocols

These protocols help devices communicate across different networks.

### 1. IP — Internet Protocol

IP is responsible for logical addressing and routing packets from a source to a destination across networks.

Two major versions:

- IPv4 — Uses 32-bit addresses.
    
- IPv6 — Uses 128-bit addresses.

Example IPv4 address: `192.168.1.10`

### 2. ICMP — Internet Control Message Protocol

Used to send network-related error reports and diagnostic messages.

Example: The `ping` command commonly uses ICMP Echo Request and Echo Reply messages to test reachability.

### 3. ARP — Address Resolution Protocol

Used in IPv4 local networks to discover the MAC address associated with an IPv4 address.

Example: A computer knows the destination's local IP address but needs its MAC address to send an Ethernet frame.

We'll study ARP in the Data Link section and revisit its relationship with IP later.


### D. Data Link Layer Protocols / Technologies

These technologies define how devices communicate over a local network link.

### 1. Ethernet (IEEE 802.3)

A widely used technology for wired local area networks (LANs). It defines frame formats and how devices communicate over Ethernet links.

### 2. Wi-Fi (IEEE 802.11)

A family of wireless LAN standards that allows devices to communicate over radio waves.

|Ethernet|Wi-Fi|
|---|---|
|Wired communication|Wireless communication|
|Uses Ethernet cables|Uses radio waves|
|Commonly used in desktops and servers|Commonly used in laptops and mobile devices|
|IEEE 802.3|IEEE 802.11|


## 7. Important Protocol Port Numbers

A port number helps the transport layer deliver incoming data to the correct application or service on a device.

For now, learn these common defaults:

|Protocol / Service|Default port|
|---|---|
|HTTP|80|
|HTTPS|443|
|DNS|53|
|DHCP|UDP 67, 68|
|FTP|TCP 20, 21|
|SSH|22|
|SMTP|25, 587, 465|
|IMAP|143, 993|
|POP3|110, 995|

Important: A port number is not the same as an IP address.

- IP address identifies a network interface or destination host.
    
- Port number identifies a service or application endpoint on that host.

Also, these are default or commonly assigned ports. Services can sometimes be configured to use different ports.


## 8. Real-World Example: What Happens When You Open a Website?

Suppose you enter `https://www.example.com` in your browser.

Here's a simplified sequence:

1. **DNS:** Your device resolves `www.example.com` to an IP address, if it doesn't already have a usable cached result.
    
2. **IP:** Your device uses IP addressing to send packets toward the server.
    
3. **Transport:** It establishes a TCP connection for HTTP/1.1 or HTTP/2, or uses QUIC over UDP for HTTP/3.
    
4. **TLS:** For HTTPS, a secure connection is established using TLS (with QUIC integrating TLS 1.3).
    
5. **HTTP:** Your browser sends an HTTP request.
    
6. **Server response:** The server processes the request and sends an HTTP response.
    
7. **Browser:** Your browser processes the response and displays the webpage.

This is why understanding protocols matters: a seemingly simple action like opening a website involves multiple protocols cooperating.

## 9. Quick Revision

|Protocol|Main responsibility|
|---|---|
|HTTP|Web communication|
|HTTPS|Secure web communication|
|DNS|Domain name resolution|
|DHCP|Automatic network configuration|
|FTP|File transfer|
|SMTP|Sending emails|
|IMAP|Synchronizing and retrieving emails|
|POP3|Retrieving emails|
|SSH|Secure remote access|
|TCP|Reliable, ordered transport|
|UDP|Connectionless datagram transport|
|IP|Logical addressing and routing|
|ICMP|Network diagnostics and error reporting|
|ARP|IPv4-to-MAC address resolution on a local link|
|Ethernet|Wired LAN communication|
|Wi-Fi|Wireless LAN communication|