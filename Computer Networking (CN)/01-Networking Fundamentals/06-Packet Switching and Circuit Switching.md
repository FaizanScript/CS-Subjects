
## 1. What is Switching in Computer Networks?

Switching is the process of forwarding data from a source to a destination through one or more intermediate network devices.

Imagine you want to send information from Computer A to Computer B, but they aren't directly connected.

Your data may need to travel through multiple intermediate devices before reaching its destination.

Switching determines how the network establishes or uses paths to transfer that data.

There are three major switching techniques:

1. Circuit Switching
    
2. Packet Switching
    
3. Message Switching


## 2. Circuit Switching

**Definition:** Circuit switching is a communication technique in which a dedicated communication path is established between the sender and receiver before data transmission begins.

Once established, the path remains reserved for the duration of the communication session.
### How does circuit switching work?

It generally involves three phases:

**Phase 1:** Connection Establishment

- The sender requests a connection with the receiver.
    
- The network establishes a dedicated path through intermediate switches.
    
- Network resources are reserved along that path.

**Phase 2:** Data Transfer

- Data travels through the established path.
    
- The path remains dedicated to the communicating parties.
    
- Other users cannot use the reserved resources during that session.

**Phase 3:** Connection Termination

- The communication session ends.
    
- The dedicated path is released.
    
- Reserved resources become available to other users.

### Example: Traditional telephone networks

In traditional telephone systems, when you called someone:

1. The telephone network established a connection between you and the other person.
    
2. A communication path was reserved for the duration of the call.
    
3. When the call ended, the network released the connection.

This is the classic example of circuit switching.

### Advantages

- Dedicated communication resources.
    
- Predictable bandwidth once the circuit is established.
    
- Consistent transmission characteristics.
    
- No need to compete for the reserved circuit during the session.

### Disadvantages

- Resources remain reserved even when no data is being transmitted.
    
- Connection establishment introduces initial delay.
    
- Not efficient for bursty data communication.
    
- A failure in the established path can interrupt communication.

**Important concept:** If two people are on a telephone call but remain silent for 20 seconds, the reserved circuit resources are still occupied.

That's one of the biggest limitations of circuit switching.


## 3. Packet Switching

**Definition:** Packet switching is a communication technique in which data is divided into smaller units called packets, which are transmitted independently through a network and reassembled at the destination when necessary.

The Internet primarily uses packet switching.

### How does packet switching work?

Suppose Computer A wants to send a large file to Computer B.

**Step 1: Data Division**

The original data is divided into smaller pieces called packets.

**Step 2: Packet Preparation**

Each packet carries information needed for delivery, such as source and destination addressing information.

**Step 3: Transmission**

Packets are forwarded through intermediate routers.

Depending on the network and routing decisions, packets may take different paths toward the destination.

**Step 4: Reception**

The destination receives the packets.

**Step 5: Reassembly**

The original data is reconstructed using the received packets. The transport protocol or application may handle ordering and recovery if needed.

### Example: Browsing a website

When you open a website, the data exchanged between your device and the server travels across the Internet in packets.

Routers forward these packets toward their destinations.

Unlike traditional circuit switching, the network doesn't reserve one dedicated end-to-end physical communication path exclusively for your website session.

### Advantages

- Efficient sharing of network resources.
    
- No dedicated path needs to be reserved for each communication session.
    
- Multiple users can share the same network links.
    
- Can route around certain network failures if alternative paths are available.
    
- Well suited to bursty Internet traffic.

### Disadvantages

- Packets may experience variable delays.
    
- Packets can be lost or arrive out of order.
    
- Network congestion can affect performance.
    
- Additional mechanisms may be needed for reliable and ordered delivery.

**Important:** Packet switching itself does not guarantee that packets will arrive in order or even arrive at all. TCP provides additional mechanisms for reliable, ordered delivery. UDP does not provide those guarantees.


## 4. Types of Packet Switching

Packet switching can be divided into two major approaches:

###### 1. Datagram approach

###### 2. Virtual Circuit approach

### A. Datagram Packet Switching

**Definition:** In datagram packet switching, each packet is treated independently and can be forwarded using its own routing decision.

**Key characteristics:**

- No dedicated connection is established before sending packets.
    
- Each packet carries destination addressing information.
    
- Different packets belonging to the same communication may take different routes.
    
- Packets may arrive out of order.
    
- The network does not guarantee delivery.

**Example:** IP (Internet Protocol) uses a connectionless datagram approach.

Imagine sending three packets:

- Packet 1 takes Route A.
    
- Packet 2 takes Route B.
    
- Packet 3 takes Route A.

They can reach the destination in a different order from the order in which they were sent.

### B. Virtual Circuit Packet Switching

**Definition:** Virtual circuit packet switching establishes a logical path between sender and receiver before data transmission. Packets belonging to that virtual circuit follow the established logical route.

**Key characteristics:**

- A logical connection is established before data transfer.
    
- Network devices maintain information about the virtual circuit.
    
- Packets follow the established logical path.
    
- Packets generally follow the same route while the virtual circuit remains established.
    
- The logical path is released when communication ends.

**Examples:** Frame Relay and ATM are historical examples of technologies that use virtual circuits.

**Important distinction:** A virtual circuit is a logical path, not a dedicated physical cable or a reserved physical circuit like traditional circuit switching.


## 5. Message Switching

**Definition:** Message switching is a communication technique in which the entire message is transmitted from one intermediate device to another, with each device storing the complete message before forwarding it.

This is commonly called store-and-forward switching.

### How does it work?

1. The sender transmits the complete message to an intermediate device.
    
2. The intermediate device stores the entire message.
    
3. It determines the next destination.
    
4. It forwards the complete message to the next device.
    
5. This process continues until the message reaches its destination.

### Disadvantages

- Requires significant storage at intermediate devices.
    
- Large messages can introduce considerable delays.
    
- Not suitable for applications requiring real-time communication.

**Example:** Message switching is a historical networking technique. Modern Internet packet forwarding does not generally store and forward entire application messages in this manner.


## 6. Circuit Switching vs Packet Switching vs Message Switching

This is an important comparison for university exams and interviews.

|Feature|Circuit Switching|Packet Switching|Message Switching|
|---|---|---|---|
|Data division|Continuous stream|Smaller packets|Complete message|
|Dedicated path|Yes|No|No|
|Resource reservation|Yes|Generally no|Generally no|
|Forwarding|Through established circuit|Packet by packet|Message by message|
|Storage requirement|Low at intermediate switches|Packet buffering|Entire message storage|
|Delay|Setup delay, then predictable transmission|Variable|Can be high|
|Resource efficiency|Lower for idle periods|Generally high|Can be inefficient|
|Example|Traditional telephone network|Internet|Historical store-and-forward systems|

### Datagram vs Virtual Circuit

|Feature|Datagram|Virtual Circuit|
|---|---|---|
|Connection setup|Not required|Required|
|Routing|Each packet can be routed independently|Logical path established first|
|Packet route|May differ|Generally follows the same logical path|
|Ordering|Not guaranteed|Often maintained by the network, depending on technology|
|Example|IP|ATM, Frame Relay|


## 7. Real-World Analogy

Let's use a simple delivery example.

**Circuit Switching — Dedicated taxi**

You book a taxi for your journey. It is reserved for you throughout the trip, even if you spend some time waiting or not moving.

**Packet Switching — Postal parcels**

You divide a large shipment into multiple parcels. Each parcel can travel through different sorting centers and routes before reaching the same destination.

**Message Switching — Complete shipment at each sorting center**

You send the entire shipment to a sorting center. It stores the complete shipment before forwarding it to the next center.

These are simplified analogies, but they help you remember the fundamental differences.

## 8. Important Concepts to Remember

1. The Internet uses packet switching: It allows multiple users to share network resources efficiently.
    
2. Packets don't need to follow the same route: In datagram networks, routing decisions can differ between packets.
    
3. Packet switching doesn't automatically mean reliable delivery: Reliability is provided by additional protocols when required.
    
4. **Circuit switching reserves resources:** The dedicated path remains established throughout the session.
    
5. **Virtual circuit is not circuit switching:** It is a logical connection used in certain packet-switched networks.

**One more distinction:** packet switching and circuit switching describe how network communication resources and paths are used. They are not simply two different types of cables or networking devices.


## 9. Quick Revision

Switching techniques

1. **Circuit Switching**
    
    Dedicated path established before communication; resources reserved until the session ends.
    
2. **Packet Switching**
    
    Data divided into packets; packets are forwarded through shared network resources.
    
3. **Message Switching**
    
    Complete message stored at each intermediate device before forwarding.


**Packet switching approaches**

- **Datagram:** packets routed independently.
    
- **Virtual Circuit:** logical path established before transmission.