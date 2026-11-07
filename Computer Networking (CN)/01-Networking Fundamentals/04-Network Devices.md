
## 4.1 Network Interface Card (NIC)

**Definition**: A Network Interface Card (NIC) is a hardware component that enables a computing device to connect to a network and communicate with other devices.

It is also called a Network Interface Controller.

### How does it work?

1. The NIC provides a network connection through Ethernet or wireless technology.
    
2. It handles the transmission and reception of network frames.
    
3. It provides a MAC address for identification at the data-link layer in typical Ethernet and Wi-Fi interfaces.
    
4. It communicates with the operating system through appropriate drivers.

### Types of NIC

1. **Wired NIC**: Connects a device to a network using a physical cable, commonly Ethernet.

2. **Wireless NIC:** Connects a device to a network wirelessly, commonly using Wi-Fi.

Examples: Your laptop's Ethernet adapter and Wi-Fi adapter.

**Important**: A modern computer can have multiple network interfaces. A NIC is not necessarily a separate physical card; it can be integrated into the motherboard.


## 4.2 Repeater

**Definition**: A repeater is a physical-layer networking device that regenerates or repeats signals to extend the transmission distance of a network.

### Why do we need a repeater?

When a signal travels through a communication medium, it can become weakened or distorted.

This can limit how far data can travel reliably.

A repeater receives the signal, regenerates it, and retransmits it.

### How does it work?

	Device A   ->    Repeater    ->    Device B
	Sender      regenerates signals     Reciver
### Advantages

- Extends the reach of a network.
    
- Helps overcome signal attenuation.
    
- Relatively simple functionality.

### Disadvantages

- Does not filter traffic.
    
- Does not understand IP addresses or MAC addresses to make forwarding decisions.
    
- Cannot solve every type of signal interference or transmission problem.

**OSI layer: Physical Layer (Layer 1).**

Example: A repeater used to extend the reach of a communication signal.


## 4.3 Hub

**Definition**: A hub is a physical-layer networking device that connects multiple devices in a LAN and broadcasts incoming signals to all other connected ports.

### How does it work?

Imagine four computers connected to a hub.

1. Computer A sends data to the hub.
    
2. The hub receives the incoming signal.
    
3. It repeats the signal to all other connected ports.
    
4. All connected devices receive the signal.
    
5. Only the intended recipient processes the relevant data.

### Advantages

- Simple to install and operate.
    
- Connects multiple devices.
    
- Relatively inexpensive historically.

### Disadvantages

- Sends incoming traffic to all other ports.
    
- Creates a shared collision domain in traditional Ethernet.
    
- Offers poor network efficiency compared with switches.
    
- Provides no traffic filtering or intelligent forwarding.

**OSI layer: Physical Layer (Layer 1).**

Important interview concept:

A hub does not maintain a MAC address table. It does not inspect destination MAC addresses to decide which port should receive a frame.


## 4.4 Bridge

**Definition**: A network bridge is a data-link-layer device that connects multiple LAN segments and forwards or filters frames using MAC addresses.

### How does it work?

A traditional learning bridge:

1. Receives an Ethernet frame.
    
2. Examines its source MAC address and learns which segment that address belongs to.
    
3. Examines the destination MAC address.
    
4. Forwards the frame to the appropriate segment if necessary.
    
5. Filters the frame if the destination is already on the same segment.

### Advantages

- Reduces unnecessary traffic between network segments.
    
- Separates collision domains.
    
- Forwards frames based on MAC addresses.

### Disadvantages

- Limited scalability compared with modern switched networks.
    
- More complex networks require mechanisms to prevent switching loops.
    
- A traditional bridge is less flexible than a modern managed switch.

**OSI layer: Data Link Layer (Layer 2).**

Important: A bridge and a switch perform similar functions. A traditional Ethernet switch can be understood as a multiport bridge.


## 4.5 Switch

**Definition**: A network switch is a networking device that connects multiple devices within a LAN and forwards frames based on destination MAC addresses.

**This is one of the most important devices in modern LANs.**

### How does a switch work?

Suppose four computers are connected to a switch.

**Step 1: Frame arrives**

Computer A sends an Ethernet frame to the switch.

**Step 2: Source MAC learning**

The switch examines the source MAC address and records which port that device is connected to.

**Step 3: Destination MAC lookup**

The switch checks its MAC address table to find the destination device's port.

**Step 4: Frame forwarding**

- If the destination MAC is known, the switch forwards the frame through the corresponding port.
    
- If the destination MAC is unknown, it typically floods the frame to other ports in the same VLAN, except the incoming port.
    
- Broadcast frames are also forwarded within the relevant VLAN, subject to network rules.

### **What is MAC Address ?**

A **MAC (Media Access Control) address** is a unique physical hardware identifier assigned to a network interface controller (NIC) for communication on a local area network (LAN). It operates at **Layer 2 (the Data Link Layer)** of the OSI model

**How a MAC Address is Structured**

- **Length:** Typically 48 bits long (expressed as 12 hexadecimal characters, such as `0A:1A:2A:3B:4B:5B`).

- **Manufacturer ID (OUI):** The first 6 digits (24 bits) represent the **Organizationally Unique Identifier**, which identifies the hardware manufacturer.

- **Device ID:** The last 6 digits (24 bits) are a unique serial number assigned by the manufacturer to that specific NIC.

- **Format:** Usually separated by colons (`0A:1A:2A:...`), hyphens (`0A-1A-2A-...`), or periods. 

### What is a MAC address table?

**Definition**: A MAC address table is a table maintained by a switch that maps learned MAC addresses to their associated switch ports and VLANs.

Example:

|MAC address|Switch port|
|---|---|
|AA:AA:AA:AA:AA:01|Port 1|
|BB:BB:BB:BB:BB:02|Port 2|
|CC:CC:CC:CC:CC:03|Port 3|

This table helps the switch forward frames to the appropriate destination instead of broadcasting every frame.

### Advantages

- Efficient forwarding of Ethernet frames.
    
- Each switch port generally creates a separate collision domain.
    
- Supports full-duplex communication in modern Ethernet.
    
- Can support VLANs and network management features.


### Disadvantages

- More expensive than a basic hub.
    
- Incorrect configuration can cause network problems.
    
- Switching loops can create serious issues without appropriate prevention mechanisms.


**OSI layer: Primarily Data Link Layer (Layer 2).**

Advanced note: Layer 3 switches can also perform IP routing. We'll cover routing later.


## 4.6 Router

**Definition**: A router is a networking device that connects different IP networks and forwards packets between them using destination IP addresses and routing information.

A router is fundamental to Internet communication.

### How does it work?

Imagine your home network communicating with a web server on the Internet.

1. Your computer generates an IP packet.
    
2. It determines that the destination is outside its local network.
    
3. It sends the packet to its default gateway, usually your home router.
    
4. The router examines the destination IP address.
    
5. It checks its routing table to determine the next hop.
    
6. It forwards the packet toward the destination network.

### What is a routing table?

**Definition**: A routing table is a collection of routing entries that tells a router where to forward packets based on their destination IP addresses.

### Advantages

- Connects different IP networks.
    
- Determines appropriate forwarding paths.
    
- Separates broadcast domains by default in typical router configurations.
    
- Can support NAT, firewalling and other network services.

### Disadvantages

- More complex to configure than basic Layer 2 devices.
    
- Routing decisions and processing require resources.
    
- Incorrect routing configuration can disrupt connectivity.

**OSI layer: Network Layer (Layer 3).**

Important distinction:

- Switch: Primarily forwards Ethernet frames using MAC addresses.
    
- Router: Forwards IP packets using destination IP addresses and routing information.


## 4.7 Modem

**Definition**: A modem (modulator-demodulator) is a communication device that adapts data signals for transmission over a particular communication medium and recovers the data at the receiving end.

### Why do we need a modem?

Different communication technologies use different signalling methods.

A modem enables communication over the transmission system used by an Internet Service Provider (ISP).

### How does it work?

- Modulation: Converts information into a suitable signal for transmission over a communication medium.
    
- Demodulation: Recovers the transmitted information from the received signal.

### Types of modems

- DSL modem.
    
- Cable modem.
    
- Cellular modem.

Important distinction: A traditional DSL or cable modem is not the same as a Wi-Fi router.

A modem provides connectivity over the ISP's access technology, while a router connects and manages IP networks. Modern home devices often combine modem, router, switch and wireless access point functions.

For fibre broadband, the corresponding device is commonly called an ONT (Optical Network Terminal). It is not simply an ordinary DSL-style modem.


## 4.8 Wireless Access Point (WAP)

**Definition**: A Wireless Access Point (WAP) is a networking device that allows wireless devices to connect to a wired LAN using Wi-Fi.

### How does it work?

1. The access point advertises a wireless network (SSID).
    
2. A smartphone or laptop discovers the network.
    
3. The device connects using the required authentication and security settings.
    
4. The access point bridges wireless traffic into the LAN.

### Advantages

- Provides wireless connectivity.
    
- Allows multiple devices to connect without individual Ethernet cables.
    
- Makes network expansion easier.

### Disadvantages

- Wireless signals can experience interference.
    
- Coverage is limited by distance and physical obstacles.
    
- Security configuration is important.

**OSI layer: Commonly operates at Layers 1 and 2, with Wi-Fi bridging at Layer 2.**

Access Point vs Router:

An access point provides wireless access to a network. A router connects different IP networks.

A typical home Wi-Fi router combines both functions.


## 4.9 Gateway

**Definition**: A gateway is a device or software system that connects different networks or systems and can translate between different communication protocols or architectures.

### How does it work?

A gateway provides an entry and exit point between networks or systems.

Depending on its purpose, it may:

- Forward traffic between networks.
    
- Translate protocols or data formats.
    
- Apply security policies.
    
- Connect systems using different communication technologies.

### Example

Imagine an organisation connecting an older industrial control system to a modern IP-based monitoring application. A protocol gateway may translate between the two communication systems.

Important interview distinction:

A default gateway is the next-hop device a host uses to reach destinations outside its local IP subnet.

In a typical home network, your router acts as the default gateway.

The word gateway has broader meanings than just a router, but in basic IP networking, the terms are often used together.


## 4.10 Firewall

**Definition**: A firewall is a network security system that monitors and controls incoming and outgoing network traffic according to predefined security rules.
### How does it work?

1. Network traffic passes through or is inspected by the firewall.
    
2. The firewall evaluates the traffic against its security policies.
    
3. It allows, blocks or otherwise handles traffic according to those policies.

### Types of firewalls

1. Packet-filtering firewall: Examines packet information such as IP addresses, ports and protocols.

2. Stateful firewall: Tracks connection state and makes decisions using connection information.

3. Application-layer firewall: Inspects application-level traffic, such as HTTP requests, depending on its capabilities.

**Advantages**

- Helps prevent unauthorised network access.
    
- Controls network traffic.
    
- Can reduce exposure to certain network attacks.

**Limitations**

- Cannot guarantee complete security.
    
- Incorrect rules can block legitimate traffic or allow harmful traffic.
    
- Must be combined with other security measures.

**OSI layer: Depends on the firewall type. Firewalls can operate across multiple layers.**


# 4.11 Comparison of all network devices

This is an important revision table. Pay particular attention to the layer and the primary function.

| Device       | Primary function                            | OSI layer             |
| ------------ | ------------------------------------------- | --------------------- |
| NIC          | Connects a host to a network                | L1/L2                 |
| Repeater     | Regenerates signals                         | L1                    |
| Hub          | Repeats signals to all ports                | L1                    |
| Bridge       | Connects LAN segments using MAC addresses   | L2                    |
| Switch       | Forwards frames using MAC addresses         | L2 primarily          |
| Router       | Routes packets using IP addresses           | L3                    |
| Modem        | Adapts signals for communication media      | Depends on technology |
| Access Point | Provides wireless LAN access                | L1/L2                 |
| Gateway      | Connects or translates between systems      | Depends on function   |
| Firewall     | Controls traffic based on security policies | Multiple layers       |


## 4.12 Important interview comparisons

### Hub vs Switch

|Hub|Switch|
|---|---|
|Physical Layer|Primarily Data Link Layer|
|Repeats signals to other ports|Forwards frames based on MAC addresses|
|Does not maintain MAC table|Maintains MAC address table|
|Shared collision domain|Separate collision domain per port|
|Less efficient|More efficient|

### Switch vs Router

|Switch|Router|
|---|---|
|Primarily connects devices within a LAN|Connects different IP networks|
|Uses MAC addresses for Layer 2 forwarding|Uses IP addresses for Layer 3 forwarding|
|Primarily operates at Layer 2|Operates at Layer 3|
|Forwards Ethernet frames|Forwards IP packets|

### Modem vs Router

|Modem|Router|
|---|---|
|Adapts signals for the ISP's access technology|Connects and routes between IP networks|
|Provides access-side connectivity|Provides network-to-network connectivity|
|Does not necessarily provide Wi-Fi|May provide Wi-Fi if it includes an access point|

### Bridge vs Switch

A traditional bridge usually has fewer ports and connects LAN segments. A switch is essentially a multiport bridge with more advanced forwarding and management capabilities.

### Router vs Gateway

A router forwards packets between IP networks. A gateway is a broader term for a system connecting networks or translating between different systems. A router can serve as a default gateway.