
## 1. Bandwidth

**Definition**: Bandwidth is the maximum data-carrying capacity of a network connection per unit of time.

In simple terms, it tells you how much data a network connection can potentially carry per second.

Common units:

- bps — bits per second
    
- Kbps — kilobits per second
    
- Mbps — megabits per second
    
- Gbps — gigabits per second

Example:

Suppose your Internet connection has a bandwidth of 100 Mbps.

This means the connection has a theoretical capacity of 100 megabits per second.

Think of bandwidth as the width of a water pipe:

- Wider pipe = more water can flow at once.
    
- Narrower pipe = less water can flow at once.

Similarly, higher bandwidth means a network can potentially carry more data per second.

**Important**: Bandwidth is not the same as actual transfer speed. Actual performance depends on other factors, including congestion, latency, packet loss, protocol overhead, and the capabilities of the endpoints.


## 2. Throughput

**Definition**: Throughput is the actual amount of data successfully transferred across a network per unit of time.

Unlike bandwidth, which describes capacity, throughput describes the actual achieved transfer rate.

Example:

Suppose:

- Your network bandwidth is 100 Mbps.
    
- You're downloading a file.
    
- Your actual measured transfer rate is 65 Mbps.
    

Then:

- Bandwidth = 100 Mbps
    
- Throughput = 65 Mbps

Why might throughput be lower than bandwidth?

- Network congestion.
    
- Packet loss and retransmissions.
    
- Protocol overhead.
    
- Server limitations.
    
- Hardware limitations.
    
- Poor network conditions.

**Key point:** Throughput is generally less than or equal to the available bandwidth of the bottleneck link, when measured over the same direction and time interval.


## 3. Goodput

**Definition**: Goodput is the amount of useful application-level data successfully delivered to the destination per unit of time, excluding protocol overhead and retransmitted data.

Let's understand the difference.

Suppose you download a file over a network.

The network transfers:

- 100 MB of total data.
    
- 10 MB is protocol overhead.
    
- 5 MB is retransmitted data.

The useful new application data delivered is 85 MB.

So:

- Total transferred data = 100 MB
    
- Useful application data = 85 MB

Goodput focuses on the useful data, not all the traffic travelling across the network.

|Metric|What it measures|
|---|---|
|Bandwidth|Maximum capacity|
|Throughput|Actual data transfer rate|
|Goodput|Useful application data transfer rate|

**Remember**: Goodput is generally less than or equal to throughput because throughput may include protocol overhead and retransmissions.


## 4. Latency

**Definition**: Latency is the time taken for data to travel from its source to its destination.

It is commonly measured in milliseconds (ms).

In simple terms, latency tells you how long data takes to travel through a network.

Example:

Suppose you send a request from your computer to a server in another country.

If the request takes 150 milliseconds to reach the server, the one-way network latency is approximately 150 ms.

### Factors affecting latency

1. **Distance**: Greater physical distance generally increases propagation delay.
    
2. **Transmission delay**: Time required to put all bits of a packet onto a link.
    
3. **Processing delay**: Time routers and other devices take to process packets.
    
4. **Queuing dela**y: Time packets spend waiting in network-device queues.

A useful simplified model:

**Total Delay = Transmission + Propagation + Processing + Queuing**

### Why does latency matter?

Consider two applications:

- File download: Higher bandwidth can help transfer large files faster.
    
- Online gaming: Low latency is crucial because players need quick responses.

A network can have high bandwidth but still have high latency.

For example, a 1 Gbps connection to a distant server can still experience 200 ms latency.

**Important**: High bandwidth does not automatically mean low latency.


## 5. Round-Trip Time (RTT)

**Definition**: Round-trip time is the total time taken for a signal or packet to travel from a source to a destination and for a response to return to the source.

Example:

Suppose:

- Request travels from your computer to a server in 50 ms.
    
- Response travels back in 50 ms.

Then:

RTT = 50 + 50 = 100 ms

RTT = 100 ms.

### Latency vs RTT

|Latency|RTT|
|---|---|
|Delay in one direction, when specified as one-way latency|Time for a round trip|
|Source to destination|Source to destination and back|
|Example: 50 ms|Example: 100 ms|

Important: RTT is not always exactly twice the one-way latency. The forward and return paths can have different delays.

### Why RTT matters in web development

Suppose your frontend makes five sequential API requests, each requiring a separate network round trip.

If the network RTT is 100 ms, the network round-trip component alone could contribute roughly:

5 times 100 = 500 ms

This is a simplified example, excluding server processing and other delays.

This is why reducing unnecessary sequential requests can improve application performance.


## 6. Jitter

**Definition**: Jitter is the variation in packet delay over time.

In simple terms, it measures how inconsistent network delivery times are.

Example:

Suppose packets are arriving with these delays:

- Packet 1: 20 ms
    
- Packet 2: 22 ms
    
- Packet 3: 21 ms
    
- Packet 4: 80 ms
    
- Packet 5: 25 ms

The delivery delay is inconsistent. This variation is jitter.

### Why does jitter matter?

Jitter is especially important for:

- Video calls.
    
- Voice calls.
    
- Live streaming.
    
- Online gaming.

For example, during a video call, inconsistent packet arrival times can cause choppy audio or video.

Remember:
- Latency = How long data takes to travel.
    
- Jitter = How much that delay varies.


## 7. Packet Loss

**Definition**: Packet loss occurs when one or more packets fail to reach their intended destination.

Example:

Suppose a device sends 100 packets, but only 95 reach the destination.

Packet Loss = Lost Packets / Total Packets Sent  x 100

= 5 / 100 x 100 = 5%

Packet loss = 5%.

### Causes of packet loss

- Network congestion.
    
- Faulty cables or hardware.
    
- Weak wireless signals.
    
- Network configuration problems.
    
- Overloaded routers or switches.

### Effects of packet loss

- Slower downloads due to retransmissions.
    
- Interrupted voice or video calls.
    
- Poor gaming experience.
    
- Unstable network communication.

Important distinction:

TCP can detect missing data and retransmit it, helping provide reliable delivery. UDP does not automatically retransmit lost packets.

However, even with TCP, packet loss can reduce performance because retransmissions and congestion-control mechanisms take time.


## 8. Network Congestion

**Definition**: Network congestion occurs when the amount of data being transmitted exceeds the available capacity of network resources.

In simple terms, too many devices or applications are trying to use the same network resources at the same time.

Example:

Suppose a network link supports 100 Mbps.

At a particular moment, multiple users collectively attempt to transmit 150 Mbps of traffic.

The link cannot carry all that traffic simultaneously at the requested rate.

As a result:

- Packets may wait in queues.
    
- Latency may increase.
    
- Packets may be dropped.
    
- Throughput may become limited.

### How can congestion be managed?

- Increasing available network capacity.
    
- Traffic prioritization and Quality of Service (QoS).
    
- Congestion-control algorithms.
    
- Load balancing.
    
- Traffic shaping and rate limiting.

We'll revisit congestion control when we study TCP.


## 9. Comparing All Network Performance Metrics

This is the most important revision table from today's topic.

|Metric|What does it tell us?|Common unit|
|---|---|---|
|Bandwidth|Maximum data-carrying capacity|Mbps, Gbps|
|Throughput|Actual achieved transfer rate|Mbps, Gbps|
|Goodput|Useful application data rate|Mbps, Gbps|
|Latency|One-way communication delay|ms|
|RTT|Round-trip communication time|ms|
|Jitter|Variation in packet delay|ms|
|Packet Loss|Percentage of packets lost|%|
|Congestion|Excessive demand on network resources|No single unit|


## 10. Practical Example: Diagnosing a Slow Application

Imagine you've built a full-stack application using React, Node.js, Express, and MongoDB.

A user reports that the application feels slow.

Here is how these metrics help you investigate:

|Problem|Possible explanation|
|---|---|
|Large file downloads take too long|Insufficient bandwidth or low throughput|
|API takes a long time to respond|High network latency, server processing delay, or database delay|
|Video calls keep freezing|High jitter or packet loss|
|Requests become slow during peak hours|Network or server congestion|
|Download speed is lower than expected|Congestion, packet loss, or endpoint limitations|

**A critical distinction**: Not every slow API is a networking problem. Your backend code, database queries, external APIs, and server resources can also cause delays.

A good developer learns to identify the actual bottleneck instead of randomly optimizing code.


## 11. Quick Revision

Remember these distinctions:

- Bandwidth = Capacity.
    
- Throughput = Actual transfer rate.
    
- Goodput = Useful application data rate.
    
- Latency = One-way delay.
    
- RTT = Round-trip delay.
    
- Jitter = Variation in delay.
    
- Packet loss = Data packets that fail to arrive.
    
- Congestion = Demand exceeding available network capacity.

		Three important relationships

		 Goodput <= Throughput 

	 Throughput <= Available bottleneck capacity

		RTT  = Forward delay + Return delay

These relationships are simplified; measurement definitions and network conditions matter.