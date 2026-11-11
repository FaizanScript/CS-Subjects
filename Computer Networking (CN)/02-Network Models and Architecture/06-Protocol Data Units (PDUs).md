

## 1. What is a Protocol Data Unit (PDU)?

**Definition**: A Protocol Data Unit (PDU) is the unit of data exchanged or processed by a particular layer of a network protocol stack.

In simpler words, each networking layer handles data in a particular form, and that form is called its PDU.

For example:

- Transport Layer handles segments (TCP) or datagrams (UDP).
    
- Internet Layer handles IP packets.
    
- Data Link Layer handles frames.
    
- Physical Layer transmits bits.

Why do we need PDUs?

Each layer has different responsibilities. Its PDU provides the structure needed to perform those responsibilities.

For example:

- TCP needs sequence numbers and port numbers.
    
- IP needs source and destination IP addresses.
    
- Ethernet needs source and destination MAC addresses.

These pieces of information are carried in the respective protocol headers.


## 2. Types of PDUs in networking

Let's understand the common PDU names from top to bottom.

| Layer              | PDU                            | Example protocol               |
| ------------------ | ------------------------------ | ------------------------------ |
| Application        | Data / Message                 | HTTP, DNS, SMTP                |
| Transport          | Segment (TCP) / Datagram (UDP) | TCP, UDP                       |
| Internet / Network | Packet / Datagram              | IP                             |
| Data Link          | Frame                          | Ethernet, Wi-Fi                |
| Physical           | Bits                           | Ethernet physical transmission |

Let's examine each one.

### A. Data / Message — Application Layer

At the Application Layer, the information being exchanged is commonly called data or a message.

Example:

```
GET /products HTTP/1.1
Host: example.com
```

This is an HTTP request.

The application protocol defines how this information is structured and interpreted.

Remember: There isn't one universal PDU name for every application protocol. Data and message are common general terms.

### B. Segment — Transport Layer (TCP)

When TCP receives application data, it encapsulates it with a TCP header.

The resulting PDU is called a TCP Segment.

A TCP segment contains:

- TCP header.
    
- TCP payload.

The header includes information such as source port, destination port, sequence number, acknowledgment number, and flags.

Important: TCP is a byte-stream protocol. It does not preserve application message boundaries. TCP segments are transport-level chunks of that byte stream.

### C. Datagram — Transport Layer (UDP)

UDP adds a UDP header to application data.

The resulting PDU is called a UDP Datagram.

A UDP header is much simpler than a TCP header. It contains four fields:

1. Source Port.
    
2. Destination Port.
    
3. Length.
    
4. Checksum.

Unlike TCP, UDP does not provide built-in reliable delivery, ordering, or retransmission.

Remember the distinction:

- TCP → Segment.
    
- UDP → Datagram.

### D. Packet — Internet Layer

The Internet Layer adds an IP header to the transport-layer PDU.

The resulting PDU is called an IP Packet.

An IP packet generally contains:

- IP header.
    
- Payload (often a TCP segment or UDP datagram).

The IP header contains information such as:

- Source IP address.
    
- Destination IP address.
    
- TTL (IPv4).
    
- Protocol field (IPv4), identifying the encapsulated transport protocol.

Remember: IP is responsible for logical addressing and forwarding packets across networks.

### E. Frame — Data Link Layer

The Data Link Layer encapsulates the network-layer packet inside a frame.

An Ethernet frame generally contains:

- Ethernet header.
    
- Payload.
    
- Frame Check Sequence (FCS) trailer.

Its header includes source and destination MAC addresses.

Remember: Frames are used for communication over a particular local link.

### F. Bits — Physical Layer

At the Physical Layer, the frame is transmitted as a sequence of bits.

```
10101010 11001100 10110101 00110110
```

These bits are represented using physical signals:

- Electrical signals over copper cables.
    
- Light pulses over fibre-optic cables.
    
- Radio waves over wireless networks.

Remember: The Physical Layer handles bit transmission, not the interpretation of IP addresses or TCP ports.


## 3. Complete PDU hierarchy

This is the most important table to remember from today's topic.

## Encapsulation — Sender

		Application Data
	HTTP message / application data

	   TCP or UDP header added

		Segment / Datagram
		   Transport Layer

		   IP header added

			IP Packet
		   Internet Layer

	Link header and trailer added

			   Frame
		   Data Link Layer

		Transmitted as signals

			   Bits
		Physical Layer

The reverse process happens at the receiver through decapsulation.


## 4. PDU vs SDU — An important distinction

You may encounter another term while studying networking: Service Data Unit (SDU).

Let's distinguish them.

- **SDU: Data received by a layer from the layer above it, which the receiving layer needs to process or carry.**
    
- **PDU: The resulting data unit after that layer adds its own protocol control information.**

For example:

When the Transport Layer receives application data:

```
Application Data = Transport Layer SDU

TCP Header + Application Data = TCP Segment (PDU)
```

This distinction helps explain encapsulation more precisely.

## 5. Common terminology traps

|Statement|Correct?|
|---|---|
|TCP's PDU is called a segment.|Yes|
|UDP's PDU is called a datagram.|Yes|
|IP's PDU is called a packet.|Yes|
|Ethernet's PDU is called a frame.|Yes|
|A frame and packet mean exactly the same thing.|No|
|TCP guarantees that application messages arrive as separate messages.|No|
|The Physical Layer transmits bits.|Yes|

## 6. Quick revision

|Layer|PDU|Addressing / identification|
|---|---|---|
|Application|Data / Message|Application-specific information|
|Transport|Segment / Datagram|Port numbers|
|Internet|Packet|IP addresses|
|Data Link|Frame|MAC addresses (Ethernet/Wi-Fi)|
|Physical|Bits|Physical signal representation|

One-line memory trick:

Data → Segment/Datagram → Packet → Frame → Bits