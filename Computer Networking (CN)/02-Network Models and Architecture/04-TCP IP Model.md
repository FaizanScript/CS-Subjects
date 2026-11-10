

### 1. What is the TCP/IP Model?

**Definition**: The TCP/IP model is a conceptual framework that describes how devices communicate over interconnected networks using a set of networking protocols.

TCP/IP stands for:

- **TCP: Transmission Control Protocol**
    
- **IP: Internet Protocol**

It is also called the Internet Protocol Suite because it contains many protocols, not just TCP and IP.

Unlike the OSI model, which is primarily a reference model for understanding network communication, TCP/IP is closely associated with the actual protocols and architecture used by the Internet.


### 2. Why was the TCP/IP Model developed?

Imagine different computers running different operating systems and connected through different types of networks.

How can all these systems communicate?

They need a common set of communication protocols.

TCP/IP provides that common foundation.

Its major goals include:

1. Interconnecting different types of networks.
    
2. Enabling communication between different devices and systems.
    
3. Providing logical addressing through IP.
    
4. Supporting end-to-end communication through transport protocols such as TCP and UDP.
    
5. Supporting different application-level services.

Key idea: TCP/IP provides the protocol architecture that makes communication across the Internet possible.


## 3. Four Layers of the TCP/IP Model

The commonly taught TCP/IP model contains four layers.

|Layer|Main responsibility|Example protocols|
|---|---|---|
|4. Application|Application-level communication|HTTP, HTTPS, DNS, SMTP, SSH|
|3. Transport|Process-to-process communication|TCP, UDP|
|2. Internet|Logical addressing and routing|IP, ICMP|
|1. Network Access|Local network communication and physical transmission|Ethernet, Wi-Fi|

Let's understand each layer individually.


## 4. Layer 4 — Application Layer

**Definition**: The Application Layer provides protocols and services that applications use to communicate over a network.

It combines the responsibilities of three OSI layers:

- Application Layer (Layer 7)
    
- Presentation Layer (Layer 6)
    
- Session Layer (Layer 5)

In other words, TCP/IP does not define separate Presentation and Session layers.

Major responsibilities:

- Providing application-level communication protocols.
    
- Defining how applications exchange data.
    
- Handling application-specific data formats.
    
- Supporting application-level sessions and interactions.

Common protocols:

|Protocol|Purpose|
|---|---|
|HTTP|Web communication|
|HTTPS|Secure web communication using TLS|
|DNS|Domain name resolution|
|FTP|File transfer|
|SMTP|Sending emails|
|IMAP|Accessing and synchronizing emails|
|SSH|Secure remote access|
|DHCP|Automatic IP configuration|

Practical example:

When you open:

```
https://example.com
```

Your browser uses HTTP to communicate with the web server. HTTPS protects that HTTP communication using TLS.

Both HTTP and DNS belong to the TCP/IP Application Layer.

Important: HTTPS is not a completely separate application protocol from HTTP. It is HTTP secured with TLS.


## 5. Layer 3 — Transport Layer

**Definition**: The Transport Layer provides communication between processes running on different devices.

It handles the delivery of application data between the correct processes.

Major responsibilities:

1. Process-to-process communication.
    
2. Port addressing.
    
3. Segmentation and reassembly.
    
4. Multiplexing and demultiplexing.
    
5. Reliability and retransmission when supported.
    
6. Flow and congestion control when supported.

Major protocols:

|TCP|UDP|
|---|---|
|Transmission Control Protocol|User Datagram Protocol|
|Connection-oriented|Connectionless|
|Reliable and ordered delivery|No delivery or ordering guarantee|
|Uses acknowledgements and retransmissions|No built-in acknowledgements or retransmissions|
|Higher protocol overhead|Lower protocol overhead|

Example:

Suppose your browser communicates with a web server.

```
Source IP:      192.168.1.10
Destination IP: 142.250.183.14

Source Port:      52000
Destination Port: 443
```

The IP addresses help identify the communicating hosts, while the port numbers identify the transport endpoints.

This is why the Transport Layer is essential for running multiple network applications simultaneously.

We'll study TCP and UDP in much greater detail in Phase 5.


## 6. Layer 2 — Internet Layer

**Definition**: The Internet Layer is responsible for logical addressing and moving packets between different interconnected networks.

This layer is equivalent to the OSI Network Layer in the simplified mapping.

Major responsibilities:

- Logical addressing using IP addresses.
    
- Routing between networks.
    
- Packet forwarding.
    
- Internetworking.
    
- Fragmentation in IPv4, where applicable.

Important protocols:

|Protocol|Purpose|
|---|---|
|IPv4|Provides 32-bit logical addressing|
|IPv6|Provides 128-bit logical addressing|
|ICMP|Network error reporting and diagnostics|
|IPsec|Security services at the IP layer|

Example:

Your laptop is connected to your home Wi-Fi network, but the website you're accessing is hosted on a server in another country.

The Internet Layer helps packets travel across multiple networks using IP addressing and routing.

Important distinction: IP provides best-effort packet delivery. It does not guarantee that packets will arrive, arrive in order, or arrive without duplication. TCP can provide reliability on top of IP.


## 7. Layer 1 — Network Access Layer

**Definition**: The Network Access Layer handles communication over the local network and the transmission of data through the underlying network medium.

It combines the responsibilities of two OSI layers:

- Data Link Layer (Layer 2)
    
- Physical Layer (Layer 1)

Major responsibilities:

- Framing.
    
- MAC addressing.
    
- Local network delivery.
    
- Error detection at the link level.
    
- Media access control.
    
- Physical transmission of bits.

Common technologies:

|Technology|Purpose|
|---|---|
|Ethernet|Wired local network communication|
|Wi-Fi|Wireless local network communication|
|PPP|Point-to-point communication|

Example:

Your laptop sends data to your Wi-Fi router.

The Network Access Layer handles the local wireless transmission between your laptop and the access point/router.

It is concerned with delivering data over that local link, not deciding the complete route across the Internet.


## 8. OSI vs TCP/IP — Layer Mapping

This is the most important part of today's topic.

The OSI model has seven layers, while the commonly taught TCP/IP model has four.

|OSI Model|TCP/IP Model|
|---|---|
|Application (7)|Application|
|Presentation (6)|Application|
|Session (5)|Application|
|Transport (4)|Transport|
|Network (3)|Internet|
|Data Link (2)|Network Access|
|Physical (1)|Network Access|

Understand these three combinations:

1. OSI Application + Presentation + Session → TCP/IP Application.
    
2. OSI Transport → TCP/IP Transport.
    
3. OSI Data Link + Physical → TCP/IP Network Access.

The mapping is conceptual, not an exact one-to-one implementation mapping.


## 9. OSI Model vs TCP/IP Model

| Basis                | OSI Model                                 | TCP/IP Model                                      |
| -------------------- | ----------------------------------------- | ------------------------------------------------- |
| Full form            | Open Systems Interconnection              | Transmission Control Protocol / Internet Protocol |
| Developer            | ISO                                       | DARPA/DoD research origins                        |
| Number of layers     | 7                                         | Commonly 4                                        |
| Nature               | Reference model                           | Protocol suite and architecture                   |
| Development          | Model first, protocols defined separately | Protocols and architecture developed together     |
| Usage                | Learning, design, troubleshooting         | Actual Internet communication                     |
| Session/Presentation | Separate layers                           | Included in Application Layer                     |
| Physical/Data Link   | Separate layers                           | Combined as Network Access                        |


### 10. Important points to remember

- TCP/IP is not just TCP and IP; it is a complete suite of protocols.
    
- TCP/IP has four layers in the commonly taught model.
    
- Some textbooks use a five-layer model by separating the Network Access Layer into Data Link and Physical.
    
- OSI is a seven-layer reference model.
    
- TCP/IP is the architecture underlying Internet communication.
    
- TCP and UDP belong to the Transport Layer.
    
- IP belongs to the Internet Layer.
    
- HTTP, DNS, and SMTP belong to the Application Layer.