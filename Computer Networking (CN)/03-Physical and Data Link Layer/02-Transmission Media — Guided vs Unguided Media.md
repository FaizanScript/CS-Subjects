
A **transmission medium** is the physical path or channel through which data signals travel from a sender to a receiver.

For example:

- When your laptop connects through an Ethernet cable, data travels through a guided medium.
    
- When your phone connects through Wi-Fi, data travels through an unguided medium.

Transmission media are divided into two major categories:

## **Types of Transmission Media**

**Guided Media**                            **Unguided Media**

Signals travel through           Signals travel wirelessly through  a physical medium.                space.


## **1. Guided Transmission Media**

**Definition:** Guided media are transmission media in which signals travel through a physical path, such as copper wires or optical fibre.

There are three important types.

### **A. Twisted-Pair Cable**

A twisted-pair cable consists of two insulated copper wires twisted around each other.

**Why are the wires twisted?**

Twisting helps reduce electromagnetic interference and crosstalk between neighbouring wire pairs.

There are two main types:

- **UTP (Unshielded Twisted Pair):** No additional metallic shielding around the wire pairs.
    
- **STP (Shielded Twisted Pair):** Includes additional shielding to reduce external electromagnetic interference.

**Applications:**

- Ethernet LAN connections.
    
- Office and home networking.
    
- Traditional telephone connections.

**Advantages:**

- Relatively inexpensive.
    
- Easy to install.
    
- Widely available.

**Limitations:**

- More susceptible to interference than fibre.
    
- Limited transmission distance compared with fibre.
    
- Performance depends on cable category and installation quality.

### **B. Coaxial Cable**

A coaxial cable consists of a central copper conductor surrounded by insulation, a metallic shield, and an outer protective jacket.

Its structure provides shielding against external interference.

**Applications:**

- Cable television.
    
- Cable Internet connections.
    
- Radio-frequency signal transmission.

**Advantages:**

- Better shielding than ordinary UTP.
    
- More resistant to external electromagnetic interference.
    
- Suitable for carrying high-frequency signals.

**Limitations:**

- Bulkier than twisted-pair cables.
    
- Can be more difficult to install.
    
- Less common than twisted-pair Ethernet in modern office LANs.

### **C. Fibre-Optic Cable**

Fibre-optic cables transmit information using light signals rather than electrical signals.

They contain thin strands of glass or plastic called optical fibres.

**How does it work?**

1. A transmitter converts digital information into optical signals.
    
2. Light travels through the fibre's core.
    
3. A receiver detects the light and converts it back into electrical/digital information.

**Two important types:**

**Single-Mode Fibre (SMF)**

Uses a very small core and carries light primarily in one propagation mode.

- Suitable for long-distance communication.
    
- Common in telecom networks and ISP backbones.

**Multimode Fibre (MMF)**

Uses a larger core and supports multiple propagation modes.

- Common for shorter-distance links.
    
- Used in data centres and campus networks.

**Advantages:**

- Very high bandwidth potential.
    
- Low signal attenuation over long distances.
    
- Immune to electromagnetic interference.
    
- Difficult to tap without detection compared with ordinary copper cables.

**Limitations:**

- Higher installation and equipment costs in many situations.
    
- Requires specialised equipment and handling.
    
- Physical damage or poor connections can disrupt transmissio


## **2. Unguided Transmission Media**

**Definition:** Unguided media transmit signals through free space using electromagnetic waves, without requiring a physical cable to guide them.

**Three important types:**

### **A. Radio Waves**

Radio waves can travel through the air and, depending on their frequency and environment, may penetrate walls and cover large areas.

**Applications:**

- Wi-Fi.
    
- Bluetooth.
    
- Radio broadcasting.
    
- Cellular communication.

**Advantages:**

- Supports wireless mobility.
    
- Can cover large areas.
    
- Does not require a physical cable between communicating devices.

**Limitations:**

- Can experience interference.
    
- Signal quality can be affected by obstacles and distance.
    
- Wireless communication requires appropriate security measures.

### **B. Microwaves**

Microwaves are electromagnetic waves commonly used for directional wireless communication.

They are often transmitted using antennas such as satellite dishes.

Two common applications:

- **Terrestrial microwave:** Communication between ground-based towers or antennas.
    
- **Satellite microwave:** Communication between ground stations and satellites.

**Applications:**

- Mobile network backhaul.
    
- Satellite communication.
    
- Point-to-point wireless links.

**Advantages:**

- Supports high-capacity wireless links.
    
- Useful for long-distance communication.
    
- Can connect locations where installing cables is difficult.

**Limitations:**

- Many terrestrial microwave links require line-of-sight.
    
- Obstacles can block or weaken signals.
    
- Weather can affect some higher-frequency links.

### **C. Infrared**

Infrared communication uses infrared electromagnetic radiation to transmit information.

**Applications:**

- Television remote controls.
    
- Short-range device communication.
    
- Certain sensors and optical communication systems.

**Advantages:**

- Useful for short-range communication.
    
- Generally does not penetrate walls.
    
- Can reduce interference between separate rooms.

**Limitations:**

- Usually requires a relatively clear path between transmitter and receiver.
    
- Limited range in common applications.
    
- Not suitable for most long-distance networking.


## **3. Guided vs Unguided Media**

|Feature|Guided|Unguided|
|---|---|---|
|Transmission path|Physical medium|Free space|
|Signal type|Electrical or optical|Electromagnetic waves|
|Mobility|Generally limited|Supports mobility|
|Interference|Depends on medium|Often more exposed to environmental interference|
|Installation|Requires physical infrastructure|Requires wireless equipment|
|Examples|Ethernet, coaxial, fibre|Wi-Fi, Bluetooth, satellite|


## **4. Quick comparison of guided media**

| Medium       | Signal     | Main characteristic                                     |
| ------------ | ---------- | ------------------------------------------------------- |
| Twisted pair | Electrical | Affordable and common in LANs                           |
| Coaxial      | Electrical | Shielded copper construction                            |
| Fibre optic  | Optical    | High bandwidth potential and long-distance transmission |


## **5. Important practical distinction**

Imagine your home Internet connection:

1. Your ISP brings fibre-optic cable into your home.
    
2. An ONT converts optical signals into electrical/network signals.
    
3. An Ethernet cable connects the ONT to your router.
    
4. Your router provides Wi-Fi.
    
5. Your laptop receives data through radio signals.

So, a single Internet connection can use **multiple transmission media** along its journey.


## **Quick revision**

- Guided media use a physical transmission path.
    
- Unguided media transmit electromagnetic waves through free space.
    
- Guided media: twisted pair, coaxial cable, fibre optic.
    
- Unguided media: radio waves, microwaves, infrared.
    
- Fibre uses light; copper cables use electrical signals.
    
- Wi-Fi uses radio waves.