
# Day 10 – Life of a Packet

The **Life of a Packet** describes how data travels from one host to another across multiple networks. As a packet moves, some headers change (like MAC addresses), while others remain constant (like IP addresses). Understanding these transformations is critical for troubleshooting, routing, and interpreting packet captures.

---

## Unique MAC Addresses
Every network interface (NIC) has a **unique MAC address** burned into the hardware. These 48‑bit addresses operate at **Layer 2 (Data Link Layer)** and are used for local network delivery—from one device to another within the same LAN segment.

Routers, switches, PCs, servers, and wireless devices all have interfaces with unique MACs.

---

## Header Ordering Differences
A packet contains multiple encapsulated headers. Two important details:

### TCP Header
- **Source IP** comes first
- **Destination IP** follows

Order inside the IPv4 header:
```
[ Source IP ] → [ Destination IP ]
```

### Ethernet Header
- **Destination MAC** comes first
- **Source MAC** follows

Order inside the Ethernet frame:
```
[ Destination MAC ] → [ Source MAC ] → [ EtherType ]
```
This ordering helps switches quickly identify where to forward a frame.

---

## Encapsulation from Host A to Host B
When a host sends data, it is wrapped (encapsulated) with several protocol headers:

### 1. Application Data
Raw data from the application (HTTP, DNS, SSH, etc.)

### 2. Transport Layer Header (TCP/UDP)
Includes:
- Source port
- Destination port
- Sequence/ACK numbers (in TCP)
- Checksum

### 3. Network Layer Header (IPv4)
Contains:
- **Source IP** (sender)
- **Destination IP** (final target)
- TTL
- Protocol (TCP/UDP)

These IP addresses **never change** during transit.

### 4. Data Link Layer Header (Ethernet)
Contains:
- **Destination MAC** (next‑hop device)
- **Source MAC** (current sender)

**These CAN change** at every router.

---

## ARP: Resolving MAC Addresses
Before a host can send a frame, it must know the **MAC address of the next-hop** (default gateway if remote, destination host if local).

If unknown, the host uses the **Address Resolution Protocol (ARP)**:
- Sends an ARP broadcast asking, “Who has IP X.X.X.X?”
- The device with that IP replies with its MAC
- The host stores it in its ARP cache

---

## What Happens at the First Router?
When the packet reaches the router:

### Router actions:
1. **Strips off** the incoming Ethernet header (MAC addresses)
2. **Reads** the IPv4 header to find the destination IP
3. **Looks up** the best route in the routing table
4. **Creates a new Ethernet header** for the outgoing interface

### Which fields change?
- New **Source MAC** = router’s outgoing interface MAC
- New **Destination MAC** = next-hop router (or final host on last hop)

### Which fields stay the same?
- **Source IP**
- **Destination IP**
- **Transport layer headers** (TCP/UDP)
- **Application data**

Routers do NOT modify Layer 3 source/destination IPs.

---

## Hop-by-Hop Forwarding
At each router along the path:
- The **IP header remains the same**, except TTL decreases by 1
- The **Ethernet header is rebuilt** with fresh MAC addresses
- The packet is forwarded to the next-hop device

This continues until the packet reaches the destination LAN.

---

## Final Delivery to the Destination Host
When the packet reaches the last router and enters the destination LAN:
- The router sets the **Destination MAC** to the end host’s MAC
- The router sets the **Source MAC** to its own LAN interface
- The IP header still contains the original source and destination IPs

When the destination host receives it:
- It removes the Ethernet header
- Passes the packet up to IP
- Hands the segment to TCP/UDP
- Application receives the data

---

## Key Point to Remember
**IP addresses DO NOT change during transmission.**
They represent the communication endpoints.

**MAC addresses change at every hop.**
They represent the link‑local path between each router.

This ensures:
- Global delivery (via IP)
- Local delivery (via MAC)

The combination of these layers working together is what enables a packet to travel across multiple switches, routers, LANs, and WANs to reach the correct device.

---
