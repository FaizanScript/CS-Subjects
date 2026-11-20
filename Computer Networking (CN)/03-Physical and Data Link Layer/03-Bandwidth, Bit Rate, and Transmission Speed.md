
These three concepts are related, but they are not interchangeable. Understanding the differences is important for networking, backend engineering, and system design.


## **1. Bandwidth**

**Definition:** In computer networking, bandwidth refers to the maximum data-carrying capacity of a communication link per unit of time.

It is commonly measured in bits per second (bps).

Examples:

- 10 Mbps
    
- 100 Mbps
    
- 1 Gbps
    
- 10 Gbps

Think of bandwidth as the capacity of a road.

- A wider road can accommodate more vehicles at once.
    
- A higher-bandwidth connection can carry more data per second.

**Example:**

Suppose:

- Connection A has 10 Mbps bandwidth.
    
- Connection B has 100 Mbps bandwidth.

Connection B has 10 times the nominal data-carrying capacity of Connection A.

**Important:** Bandwidth is the capacity of a link, not a guarantee that applications will actually achieve that speed.

Also, in physics and signal processing, bandwidth can mean a range of frequencies measured in hertz (Hz). In networking discussions, it commonly refers to data-carrying capacity measured in bits per second.


## **2. Bit Rate**

**Definition:** Bit rate is the number of bits transmitted or processed per second.

It is measured in bits per second (bps).

Common units:

|Unit|Value|
|---|---|
|1 Kbps|1,000 bps|
|1 Mbps|1,000,000 bps|
|1 Gbps|1,000,000,000 bps|
|1 Tbps|1,000,000,000,000 bps|

**Example:**

If a connection transmits 5 million bits in one second:

	Bit Rate = 5,000,000/1 = 5,000,000 bps

Therefore, its bit rate is 5 Mbps.


## **3. Baud Rate**

This is where students often confuse bit rate and signal rate.

**Definition:** Baud rate is the number of signal symbols transmitted per second.

It is measured in baud (symbols per second).

A symbol is a distinct signal state that represents one or more bits.

Consider two examples:

**Example A: One bit per symbol**

Illustrative binary signalling

# **0**                **1**          **1**          **0**
Symbol 1      Symbol 2      Symbol 3    Symbol 4

- 1 symbol = 1 bit
    
- 1,000 symbols/second = 1,000 bits/second


**Example B: Two bits per symbol**

Illustrative four-level signalling

## **00**            **01**             **10**         **11**
Symbol 1      Symbol 2      Symbol 3     Symbol 4

- 1 symbol = 2 bits
    
- 1,000 symbols/second = 2,000 bits/second

The relationship is:

	Bit rate = band Rate x Bits per symbol

For example:

- Baud rate = 1,000 baud
    
- Bits per symbol = 2

	= 1000 x 2 = 2000 bps

**Key distinction:**

|Bit Rate|Baud Rate|
|---|---|
|Bits transmitted per second|Symbols transmitted per second|
|Measured in bps|Measured in baud|
|Depends on bits per symbol|Counts signal symbols|

Baud rate and bit rate are equal only when each symbol represents exactly one bit.


## **4. Transmission Speed vs Throughput**

A connection may have a maximum capacity of 100 Mbps, but that doesn't mean an application will always transfer data at 100 Mbps.

Let's distinguish the terms.

|Term|Meaning|
|---|---|
|Bandwidth|Maximum data-carrying capacity|
|Bit rate|Number of bits transmitted per second|
|Throughput|Actual achieved data transfer rate|
|Goodput|Useful application data delivered per second|

**Example:**

Suppose your Internet connection has a capacity of 100 Mbps.

**During a download:**

- Maximum connection capacity: 100 Mbps
    
- Actual measured throughput: 80 Mbps
    
- Useful application data rate: 75 Mbps

**Why are these values different?**

Because real-world communication involves:

- Protocol headers and trailers.
    
- Network congestion.
    
- Retransmissions.
    
- Latency and receiver limitations.
    
- Server and application limitations.

Remember:

	Goodput <= throughput

Throughput is generally bounded by the available bottleneck capacity when measured consistently.


## **5. Important numerical conversion: Mbps vs MB/s**

This is especially useful when downloading files.

**Mbps** = Megabits per second.

**MB/s** = Megabytes per second.

Since:

	1 Byte = 8 Bits

Therefore:

	 MB/s = Mbps / 8

Example:

Your Internet connection provides 80 Mbps.

			80 / 8 = 10 MB/s

So your theoretical maximum download rate is 10 MB/s, before accounting for protocol overhead and other limitations.

**Quick conversion reference**

|Internet speed|Theoretical file transfer rate|
|---|---|
|40 Mbps|5 MB/s|
|80 Mbps|10 MB/s|
|100 Mbps|12.5 MB/s|
|200 Mbps|25 MB/s|
|500 Mbps|62.5 MB/s|
|1 Gbps|125 MB/s|

## **Quick revision**

|Concept|Remember|
|---|---|
|Bandwidth|Maximum data-carrying capacity|
|Bit rate|Bits per second|
|Baud rate|Symbols per second|
|Throughput|Actual achieved transfer rate|
|Goodput|Useful application data rate|