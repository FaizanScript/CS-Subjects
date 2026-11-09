

### 1. The seven layers — quick reference

|Layer|Name|Main responsibility|
|---|---|---|
|7|Application|Network services for applications|
|6|Presentation|Data format and representation|
|5|Session|Session management|
|4|Transport|Process-to-process delivery|
|3|Network|Logical addressing and routing|
|2|Data Link|Local network delivery|
|1|Physical|Transmission of raw bits|


### 2. Layer 7 — Application Layer

**Definition**: The Application Layer provides network services and protocols that applications use to communicate over a network.

It is the layer closest to the user-facing applications.

Examples of protocols:

- HTTP — Web communication.
    
- DNS — Domain name resolution.
    
- SMTP — Sending emails.
    
- FTP — File transfer.
    
- SSH — Secure remote access.

Practical example:

When you open a website:

1. Your browser needs to communicate with a web server.
    
2. It uses HTTP to structure the request.
    
3. The request might look conceptually like this:

```
GET /products HTTP/1.1
Host: example.com
```

The browser uses HTTP at the Application Layer to communicate with the server.

**Important**: The browser itself is not the Application Layer. HTTP and other application-level protocols are what we associate with this layer.


### 3. Layer 6 — Presentation Layer

**Definition**: The Presentation Layer is responsible for ensuring that data is represented in a format the receiving system can understand.

Its major responsibilities are:

|Responsibility|Explanation|Example|
|---|---|---|
|Translation|Converts data between formats|UTF-8, ASCII|
|Encryption|Converts readable data into encrypted data|Encryption|
|Compression|Reduces data size|gzip|

Example:

Imagine your application sends the text:

```
Hello World
```

Different systems need a common way to represent that text as bytes. Character encoding standards such as UTF-8 provide that representation.

Similarly, compression can reduce the amount of data transferred, and encryption protects information from unauthorized reading.

Real-world clarification: TLS performs encryption for HTTPS, but it doesn't map perfectly to one OSI layer. Calling it a Presentation Layer function is a useful conceptual simplification, not a strict implementation fact.


### 4. Layer 5 — Session Layer

**Definition**: The Session Layer is responsible for establishing, managing, synchronizing, and terminating communication sessions between applications.

Think of a session as an ongoing communication interaction.

Its responsibilities include:

- Session establishment — starting a communication session.
    
- Session maintenance — managing the ongoing interaction.
    
- Session termination — ending the session.
    
- Synchronization — providing checkpoints during certain communication processes.

Example:

Imagine two systems are involved in a long data-transfer operation. If the transfer is interrupted, synchronization checkpoints can help the process resume from an earlier point rather than starting over.

Real-world clarification: Modern TCP/IP networking generally implements session-related functionality through application protocols and libraries rather than a separate Session Layer.


### 5. Layer 4 — Transport Layer

This is one of the most important layers for backend development.

**Definition**: The Transport Layer provides communication between processes running on different hosts.

Its responsibilities include:

1. Segmentation and reassembly.
    
2. Port addressing.
    
3. Multiplexing and demultiplexing.
    
4. Reliable delivery when supported.
    
5. Flow control.
    
6. Error recovery when supported.

Important protocols:

|TCP|UDP|
|---|---|
|Connection-oriented|Connectionless|
|Reliable, ordered byte stream|No delivery or ordering guarantee|
|Retransmits lost data|No built-in retransmission|
|More protocol overhead|Lower protocol overhead|

Example:

Suppose your computer is communicating with a web server.

- Your browser might use local port `52000`.
    
- The web server might use port `443`.
    
- TCP uses these port numbers to identify the communicating processes.

Remember:

- IP address identifies a host/interface for network delivery.
    
- Port number identifies a transport endpoint on that host.
    
- TCP or UDP carries application data between those endpoints.

We'll study TCP, UDP, ports, and the three-way handshake in detail in Phase 5.


### 6. Layer 3 — Network Layer

**Definition**: The Network Layer handles logical addressing and the forwarding/routing of packets between different networks.

Its major responsibilities include:

- Logical addressing using IP addresses.
    
- Routing — selecting paths through networks.
    
- Packet forwarding — sending packets toward their next hop.
    
- Internetworking — connecting different networks.

Example:

Suppose your computer has this IP address:

```
192.168.1.10
```

And the server you're contacting has this IP address:

```
142.250.183.14
```

These devices may be on completely different networks.

Routers examine the destination IP address and use their routing tables to decide where to forward packets.

Important distinction:

- IP address: identifies the network-layer destination.
    
- MAC address: identifies a link-layer destination on the local network.

A router may receive a frame addressed to its own MAC address, remove that frame, examine the IP packet, and forward it inside a new frame on the next link.


### 7. Layer 2 — Data Link Layer

**Definition**: The Data Link Layer provides communication between devices on the same local link or network segment.

Its responsibilities include:

- Framing — organizing bits into frames.
    
- Physical addressing — using MAC addresses in technologies such as Ethernet.
    
- Error detection — detecting transmission errors.
    
- Media access control — coordinating access to shared transmission media.

Example:

Suppose your laptop wants to send data to your home router.

Your laptop needs to deliver an Ethernet or Wi-Fi frame to the router's local-link interface.

A switch uses MAC addresses to forward Ethernet frames within a LAN.

Important: A switch doesn't normally use IP addresses to make ordinary Layer 2 forwarding decisions. It learns MAC addresses and maintains a MAC address table.

### 8. Layer 1 — Physical Layer

**Definition**: The Physical Layer transmits raw bits over a physical or wireless transmission medium.

Its responsibilities include:

- Signal transmission.
    
- Electrical, optical, or radio characteristics.
    
- Cables and connectors.
    
- Data transmission rates.
    
- Bit representation and timing.

Examples:

- Ethernet cables.
    
- Fibre-optic cables.
    
- Radio waves used by Wi-Fi.
    
- Electrical and optical signals.

At this layer, we're concerned with transmitting bits, not interpreting IP addresses or HTTP requests.


## 9. How all seven layers work together

Let's connect everything using one practical example: opening `https://example.com`.

At a high level:

1. Application: Browser creates an HTTP request.
    
2. Presentation: Data representation, encryption, and compression functions are handled as applicable.
    
3. Session: Session-related communication functions are managed.
    
4. Transport: TCP or UDP-based transport carries the data using port numbers.
    
5. Network: IP provides logical addressing and packet forwarding.
    
6. Data Link: A frame is created for transmission over the local link.
    
7. Physical: Bits are transmitted as electrical, optical, or radio signals.

At the receiving end, the process moves upward through the layers.

This downward process is called encapsulation, and the reverse process is called decapsulation. We'll study both properly in a later topic.

## 10. Important distinctions for interviews

|Question|Correct distinction|
|---|---|
|Does a switch use IP addresses for normal Layer 2 forwarding?|No, it uses MAC addresses.|
|Does a router use MAC addresses to route between networks?|It routes using IP addresses; MAC addresses are used for local-link delivery.|
|Is HTTP the browser itself?|No, HTTP is an application-layer protocol.|
|Does TCP belong to the Network Layer?|No, TCP belongs to the Transport Layer.|
|Is TLS strictly a Presentation Layer protocol?|No, its mapping to OSI is not exact.|
|Are all seven OSI layers separate in real-world implementations?|No.|

## Quick revision

- Application: What network service does the application need?
    
- Presentation: How should the data be represented?
    
- Session: How is the communication session managed?
    
- Transport: Which process should receive the data, and what transport service is needed?
    
- Network: Which host/network should receive the packet, and where should it be forwarded?
    
- Data Link: How should the data reach the next device on this local link?
    
- Physical: How are the bits transmitted?