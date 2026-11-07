
## 3.1 What is network topology?

**Definition**: Network topology is the arrangement of devices (nodes) and communication links in a computer network, describing how they are connected and, depending on the topology, how data flows between them.

In simple words, topology describes the structure of a network.

For example, imagine five computers in a college lab. They could all connect to one central switch, connect along a shared communication line, or form a ring. Each arrangement represents a different topology.

### Why is network topology important?

- Performance: The arrangement can affect data flow, congestion and communication efficiency.
    
- Reliability: It influences how failures affect the network.
    
- Scalability: It affects how easily new devices can be added.
    
- Cost: Different arrangements require different amounts of cabling and equipment.
    
- Maintenance: It affects how easily faults can be identified and fixed.


## 3.2 Types of network topology

Network topology is commonly studied in two forms.

1. Physical topology: The actual physical arrangement of devices, cables and connections.

2. Logical topology: The way data flows between devices, regardless of the exact physical arrangement.

For example, a network's physical wiring and its logical communication pattern do not always have to be the same.

Keep this distinction in mind: physical topology describes how things are connected; logical topology describes how communication occurs.


## 3.3 Bus topology

![Network Topologies (Bus, Star, Ring) Revision Notes — OCR GCSE | MasteryMind](https://images.openai.com/static-rsc-4/fzsGDasWVw4JeZBdVmH7g_az2UV8p-BydKzy4rsBwhMaTBnNDvdknZNe5inMp4yZecAPuQIw_oAz7gHiZ0xpzqHZalPP6mDxYdV3jimZ8rJPciZEIKrIOuCwzCOgRcndXoy-ezoQdmVDDk6WnT6GbRmJapMPCRXWpYnNMzYQZuI?purpose=inline)

Conceptual diagram: devices share a common backbone cable.

**Definition**: Bus topology is a network arrangement in which multiple devices connect to a single shared communication backbone.

### How does it work?

1. All devices share the same main communication cable.
    
2. When a device transmits data, the signal travels along the shared medium.
    
3. Connected devices can receive the signal, but the intended destination processes the data.
    
4. Terminators at the ends of a traditional bus prevent signals from reflecting back along the cable.

### Advantages

- Requires relatively little cabling.
    
- Simple and inexpensive for small installations.
    
- Does not require a central switch.

### Disadvantages

- A backbone cable failure can disrupt the entire network.
    
- Performance can deteriorate as traffic and contention increase.
    
- Fault detection can be difficult.
    
- Adding devices and maintaining the shared cable can become inconvenient.

Example: Traditional coaxial Ethernet networks used a physical bus arrangement.

Important: Modern Ethernet LANs typically use switches rather than a shared coaxial bus.


## 3.4 Star topology

![Star Topology Diagram: Structure, Functionality & Benefits](https://images.openai.com/static-rsc-4/K9K-q6Ar-3GcpQg9zdzKREI8H9pXYRgdIL64l3pe_GEcROABpYvcyge7lU-bq_ZnflHzjDtlv43kis8wgdT-GG5FN1n3thnSiedfwMGH0lC8eKys8g5cfQV_ULPmhZidYgOt29VR1RanK_W0SggGxwjDTGyhdKnHVQEXw0B4QAI?purpose=inline)

Conceptual diagram: every end device connects to a central device.

**Definition**: Star topology is a network arrangement in which each device connects through a separate link to a central device, such as a switch or hub.

### How does it work?

1. Each computer has its own link to the central device.
    
2. A switch forwards received frames toward the appropriate destination port.
    
3. A hub, in contrast, repeats incoming signals to its other ports.
    
4. Devices communicate through the central device.

### Advantages

- Easy to install, expand and troubleshoot.
    
- Failure of one device's individual link usually affects only that device.
    
- A switch can forward traffic efficiently to the intended port.
    
- Individual links are easier to isolate when diagnosing faults.

### Disadvantages

- Failure of the central switch or hub can disrupt the connected network.
    
- Requires separate links from each device to the centre.
    
- Cabling and central equipment add cost.

Example: Most wired Ethernet networks in offices and computer labs use a star arrangement, often with multiple switches connected together.

Interview point: A star network's central switch is a potential single point of failure unless redundancy is provided.


## 3.8 Hybrid topology

![PPT - Understanding Computer Networks: Fundamental Concepts, Classifications, and Topologies PowerPoint Presentation - ID:2280570](https://images.openai.com/static-rsc-4/Q3reRJWdjrJ7VJNud0Pbn00BcHhAAnSpQuMtGimJ5ElbwOfgqsO80snnDLrE6LFo58Q5jnmrwz_ZbgtD8EDaXdJzjEYH4jChihXOY2X-kIAktBtXvX7oAIvDm1uzaU0QlAR0H-a94gefJN6m-PNkIdqfdoi5Y2hCPFTsUJ4geSQ?purpose=inline)

Conceptual diagram: two or more topology arrangements combined into one network.

**Definition**: Hybrid topology is a network arrangement that combines two or more different topology types.

### How does it work?

A network may use a star topology inside individual offices and connect those stars through a hierarchical or backbone arrangement.

### Advantages

- Flexible network design.
    
- Can be tailored to different departments or buildings.
    
- Can support expansion and fault isolation.

### Disadvantages

- Design and maintenance can be more complicated.
    
- Costs may be higher.
    
- Performance and reliability depend on the selected components and overall design.

Example: An organisation uses star topology in each building and connects the building networks through a redundant backbone.


## 3.5 Ring topology

![Topologia inel](https://images.openai.com/static-rsc-4/FgVhoBuEltlikqscPUPR7VnwBqo70PWD15_w5REfzo1wA6u_YQD5lpa34jly2OJvRMAbs6hXv1bbuK88oaaRoMUb0ugdbGzX15DIZM_yEydAg7wW9wA_COL5VLjNtZ7uFQdUpg4JGqALYlY6iP7yHe0MdVmkI3jFnUAsuS0YbUo?purpose=inline)

Conceptual diagram: each device connects to neighbouring devices in a loop.

**Definition**: Ring topology is a network arrangement in which each device connects to two neighbouring devices, forming a closed loop.

### How does it work?

In a traditional unidirectional ring:

1. A device sends data to its neighbouring device.
    
2. Each intermediate device forwards the data around the ring.
    
3. The data reaches its intended destination.
    
4. Depending on the protocol, data may continue until it returns to the sender or is removed from the ring.

Some ring systems use token passing, where a token controls which device may transmit.

### Advantages

- Communication can be orderly in systems that control access to the ring.
    
- Token-based systems can avoid collisions between competing transmissions.
    
- Every device has a defined place in the ring.

### Disadvantages

- A failed link or device can disrupt a basic ring.
    
- Adding or removing devices may interrupt communication.
    
- Troubleshooting and expansion can be more complex than in a simple star network.

Example: Token Ring is a historical example of ring networking. Some specialised fibre-optic networks also use ring arrangements, often with protection mechanisms.

Important: Not every ring uses token passing. Token passing is one access-control method, not the definition of a ring topology.


## 3.6 Mesh topology

![Chapter 1  Introduction (Data Communication by Forouzan) | PPT](https://images.openai.com/static-rsc-4/EvH7PrpOzRjUWz2_gVA5txGkGgw8YnaJ1jlDOAI0Lfu1NUsER7rn--ScMem0LGi_ITpMhqiyJb5YU_fJBSNoFOTuwUvIpEO0pzJticknCos85heuoTmpKpsyC56hGDcXf2EVq70PVoH9GhHpOjpyea7kHSb874H3p-UPI8QHgVI?purpose=inline)

Conceptual diagram: in a full mesh, every device connects directly to every other device.

**Definition**: Mesh topology is a network arrangement in which devices are interconnected through multiple links. In a full mesh, every device has a direct link to every other device.

There are two main types.

**A. Full mesh**

Every device connects directly to every other device.

For \(n\) devices, the number of direct links required is:
	n(n-1)/2

For example, with 5 devices:

		5(5-1)/2 = 10links

**B. Partial mesh**

Only selected devices have multiple direct connections. Not every pair of devices has a direct link.

### Advantages

- Multiple paths can provide resilience against link failures.
    
- A full mesh offers a direct connection between every pair of devices.
    
- Traffic may be rerouted through alternative paths when routing and network design support it.

### Disadvantages

- Full mesh requires many links as the number of devices grows.
    
- Installation and maintenance can be expensive.
    
- Routing and configuration can become complex.

Examples: Mesh arrangements are used in some wireless mesh networks, backbone networks and highly interconnected network designs.

Interview point: Mesh provides redundancy, but redundancy does not happen automatically at the application level; the network must have suitable routing or failover mechanisms.


## 3.7 Tree topology

![What Is a Network Topology Diagram? | Gliffy](https://images.openai.com/static-rsc-4/yR6D0fooyaUoPG8GvVnJDwsioQx3gHR7Jh2lJrXq1u_nTsZtIUf-nEwQkRFsSXOeBCPLdAns-eowch8Wd2FaZrjWyMWVjymubE_OZy_7UAuC-5QUNN79ZlwPzPv5JQetz5S2L8T6M4CjaCbjGtjF5goYMtlHJaLQvshd4aV0awg?purpose=inline)

Conceptual diagram: devices are organised into hierarchical levels.

**Definition**: Tree topology is a hierarchical network arrangement in which devices or network segments are organised into parent-child levels.

### How does it work?

1. A root or core device connects to lower-level devices.
    
2. Distribution devices connect to access devices or end systems.
    
3. Data travels between branches through the appropriate network devices.
    
4. Additional branches can be added to expand the network.

### Advantages

- Suitable for large, structured networks.
    
- Supports organised expansion.
    
- Makes it possible to divide networks into manageable segments.

### Disadvantages

- Failure of an important upstream device or link can affect an entire branch.
    
- More cabling and equipment may be needed.
    
- Planning and troubleshooting can be complex.

Example: A university network with a core switch connected to building-level switches, which then connect to laboratory computers.

Interview point: A tree topology is hierarchical. It can be viewed as an extension of star-like arrangements connected through higher-level devices.


## 3.9 Comparison of network topologies

|Topology|Structure|Main advantage|Main disadvantage|
|---|---|---|---|
|Bus|Shared backbone|Less cabling in a traditional setup|Backbone failure can disrupt the network|
|Star|Central device|Easy troubleshooting and expansion|Central device can be a single point of failure|
|Ring|Closed loop|Orderly communication in certain designs|Basic ring can be disrupted by a failed link|
|Full mesh|Every pair directly connected|High path redundancy|Many links and high cost|
|Partial mesh|Selected multiple links|Balance between redundancy and cost|Not every pair has a direct path|
|Tree|Hierarchical branches|Scalable organisation|Upstream failures can affect branches|
|Hybrid|Combination of topologies|Flexible design|Greater design complexity|


### Common interview comparisons

**Star vs bus:** Star uses a central device and separate links; bus shares a backbone.

**Star vs mesh**: Star is simpler and cheaper to deploy, while mesh can provide multiple paths at the cost of additional links and complexity.

**Ring vs mesh:** Ring follows a loop, while mesh provides multiple interconnections, depending on whether it is full or partial.

**Tree vs hybrid:** Tree describes a hierarchical arrangement; hybrid describes a combination of topology types. A hybrid network can itself contain a tree.