

## 1. What is Encapsulation?

**Definition**: Encapsulation is the process of adding protocol-specific information to data as it moves down the networking layers at the sender's side.

This additional information is usually added in the form of headers, and sometimes trailers.

In simple words:

When data travels from the Application Layer toward the Physical Layer, each relevant layer adds its own control information to help deliver that data correctly.

### Why is encapsulation necessary?

Different layers have different responsibilities.

For example:

- Application Layer needs to identify what kind of application data is being exchanged.
    
- Transport Layer needs source and destination port numbers.
    
- Internet Layer needs source and destination IP addresses.
    
- Data Link Layer needs source and destination MAC addresses for local-link delivery.

Each layer adds the information necessary for its own job.


## 2. What is Decapsulation?

**Definition**: Decapsulation is the reverse process in which the receiving device removes and processes the protocol-specific headers and trailers as data moves up the networking layers.

In simple words:

The receiver examines the additional information added by the sender and removes it layer by layer until the original application data is recovered.


## 3. How does Encapsulation work?

Let's understand it step by step.

Imagine your browser sends this HTTP request to a web server:

```
GET /products HTTP/1.1
Host: example.com
```

We'll follow the data as it moves down the TCP/IP stack.

### Step 1 — Application Layer

The browser creates application data.

```
HTTP Request
```

At this point, the data contains the information required by the application protocol.

Example:

```
GET /products HTTP/1.1
Host: example.com
```

The Application Layer passes this data to the Transport Layer.


### Step 2 — Transport Layer

The Transport Layer adds a TCP or UDP header, depending on the transport protocol being used.

For a traditional HTTP/1.1 or HTTP/2 connection using TCP, the TCP header contains information such as:

- Source port.
    
- Destination port.
    
- Sequence number.
    
- Acknowledgment number.
    
- Control flags.
    
- Window size.

Example:

```
[TCP Header]
Source Port: 52000
Destination Port: 443

[HTTP Data]
GET /products HTTP/1.1
Host: example.com
```

Now the resulting data unit is called a TCP Segment.

==🔵PDU: Segment==

Important: TCP adds a header, not a trailer, in its standard format. UDP also adds a header, but it does not provide TCP's reliability features.

### Step 3 — Internet Layer

The Internet Layer adds an IP header.

This header contains information such as:

- Source IP address.
    
- Destination IP address.
    
- Time To Live (TTL) in IPv4.
    
- Protocol information identifying the encapsulated transport protocol.

Example:

```
[IP Header]
Source IP: 192.168.1.10
Destination IP: 93.184.216.34

[TCP Header]
Source Port: 52000
Destination Port: 443

[HTTP Data]
GET /products HTTP/1.1
Host: example.com
```

The resulting data unit is called an IP Packet.

==🔵PDU: Packet==

### Step 4 — Network Access Layer

The Network Access Layer adds link-layer information.

For an Ethernet frame, this includes:

- Source MAC address.
    
- Destination MAC address.
    
- Type/Length field.
    
- Frame Check Sequence (FCS) trailer.

Example:

```
[Ethernet Header]
Source MAC: AA:BB:CC:11:22:33
Destination MAC: DD:EE:FF:44:55:66

[IP Header]
Source IP: 192.168.1.10
Destination IP: 93.184.216.34

[TCP Header]
Source Port: 52000
Destination Port: 443

[HTTP Data]
GET /products HTTP/1.1
Host: example.com

[Ethernet Trailer: FCS]
```

The resulting data unit is called an Ethernet Frame.

==🔵PDU: Frame==

Note: The source and destination MAC addresses shown are illustrative. The destination MAC would normally be the MAC address of the next hop on the local link, such as your default gateway, rather than the remote web server's MAC address.

### Step 5 — Physical Layer

Finally, the frame is transmitted as a sequence of bits over the network medium.

```
10101010 11001100 10110101 ...
```

The actual transmission may use electrical signals, light pulses, or radio waves.


## 4. Complete Encapsulation Structure

This is the structure you should remember:

| Layer          | What gets added                  | Resulting PDU      |
| -------------- | -------------------------------- | ------------------ |
| Application    | Application protocol information | Data               |
| Transport      | TCP/UDP header                   | Segment / Datagram |
| Internet       | IP header                        | Packet             |
| Network Access | Link header and usually trailer  | Frame              |
| Physical       | Converts/transmits as signals    | Bits               |

The overall structure can be represented as:

```
Application Data

[TCP Header + Application Data]

[IP Header + TCP Header + Application Data]

[Ethernet Header + IP Header + TCP Header
 + Application Data + Ethernet Trailer]

Bits transmitted over the medium
```

**One important detail:** A PDU (Protocol Data Unit) is the data unit handled at a particular networking layer. The names segment, packet, frame, and bits describe the data at different stages of encapsulation.


## 5. How Decapsulation works

Now imagine the server receives the transmitted frame.

The receiving device processes the data in the reverse direction.

### Step 1 — Physical Layer

The receiving network interface receives signals and reconstructs the transmitted bits.

### Step 2 — Data Link Layer

The receiver:

1. Processes the Ethernet frame.
    
2. Checks the destination MAC address.
    
3. Checks the FCS for transmission errors.
    
4. Removes the Ethernet header and trailer.
    
5. Passes the enclosed IP packet upward.

### Step 3 — Internet Layer

The receiver:

1. Processes the IP header.
    
2. Checks the destination IP address.
    
3. Identifies the encapsulated transport protocol.
    
4. Removes the IP header.
    
5. Passes the TCP segment or UDP datagram upward.

### Step 4 — Transport Layer

If TCP is being used, the receiver:

1. Processes the TCP header.
    
2. Uses the destination port to identify the receiving socket.
    
3. Handles sequence numbers, acknowledgments, and reassembly as necessary.
    
4. Passes the resulting application data to the appropriate application.

### Step 5 — Application Layer

The web server receives the HTTP request and processes it.

The original application data is now available to the application.


## 6. What happens when data passes through a router?

This is where many beginners make mistakes.

A router does not simply forward the original Ethernet frame unchanged across every network.

Let's understand why.

Imagine:

Your laptop sends an IP packet to a remote server through your home router.

### At your laptop

The laptop creates:

```
Ethernet Frame
    └── IP Packet
          └── TCP Segment
                └── HTTP Data
```

The Ethernet frame is addressed to the router's local-link MAC address.

### At the router

The router:

1. Receives the frame.
    
2. Processes and removes the incoming link-layer header and trailer.
    
3. Examines the destination IP address.
    
4. Checks its routing table to select the next hop and outgoing interface.
    
5. Decreases the IPv4 TTL by at least one.
    
6. Updates the IPv4 header checksum.
    
7. Encapsulates the IP packet in a new link-layer frame for the outgoing link.
    
8. Forwards it.

The key point:

- The original Ethernet frame is replaced.
    
- The IP packet is generally forwarded onward, with certain IP header fields updated.
    
- The TCP segment and application data generally remain unchanged during ordinary routing.

This process repeats at subsequent routers.

Important: The outgoing link might use Ethernet, Wi-Fi, or another link-layer technology. The frame format can change at each hop.


## 7. Headers vs Trailers

You should understand the difference.

|Header|Trailer|
|---|---|
|Added before the payload|Added after the payload|
|Contains control information|Often contains error-detection information|
|Used by protocols such as TCP and IP|Used by technologies such as Ethernet|

Example of an Ethernet frame:

```
[Header] [Payload] [Trailer]
```

The Ethernet header contains MAC addresses and other fields.

The FCS trailer helps detect corruption in the received frame.

Not every networking protocol uses both a header and a trailer.

## 8. Important concepts and common mistakes

|Concept|What you should remember|
|---|---|
|Encapsulation|Adding protocol information as data moves down the stack|
|Decapsulation|Processing and removing protocol information as data moves up|
|Header|Information added before the payload|
|Trailer|Information added after the payload|
|Segment|Transport-layer PDU for TCP|
|Datagram|Commonly used for UDP and IP PDUs|
|Packet|Internet/network-layer PDU|
|Frame|Data Link-layer PDU|
|Bits|Physical-layer transmission unit|

A small terminology warning: UDP's PDU is commonly called a UDP datagram, whereas IP's PDU is an IP packet. TCP's PDU is a TCP segment.

## Quick revision

**Sender — Encapsulation**

1. Application Data
    
2. TCP Header added → Segment
    
3. IP Header added → Packet
    
4. Link Header and Trailer added → Frame
    
5. Transmitted as Bits

**Receiver — Decapsulation**

1. Receives Bits
    
2. Processes Frame
    
3. Processes Packet
    
4. Processes Segment
    
5. Delivers Application Data
