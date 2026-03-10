
# Day 7 – IPv4 Header

The **IPv4 header** is the Layer 3 structure that enables packets to move between different networks. Routers read the header to perform **routing**, ensuring data can cross local networks, regional ISPs, or the global Internet. The IPv4 header encapsulates a **TCP** or **UDP** segment before forwarding.

---

## IPv4 Header Fields and Sizes

| Field | Size (bits) |
|-------|-------------|
| Version | 4 |
| IHL (Internet Header Length) | 4 |
| DSCP | 6 |
| ECN | 2 |
| Total Length | 16 |
| Identification | 16 |
| Flags | 3 |
| Fragment Offset | 13 |
| Time To Live (TTL) | 8 |
| Protocol | 8 |
| Header Checksum | 16 |
| Source IP Address | 32 |
| Destination IP Address | 32 |
| Options | Up to 320 |

---

## Version
- *4 bits*
- Indicates the IP version used.
- For IPv4, the binary value is **0100**.

---

## Internet Header Length (IHL)
- *4 bits*
- Specifies the IPv4 header size in **4‑byte units**.
- Minimum = **5** (20 bytes) → no options
- Maximum = **15** (60 bytes) → full options
- Ensures routers know where the header ends and the payload begins.

---

## DSCP (Differentiated Services Code Point)
- *6 bits*
- Supports QoS by marking packets for priority handling (voice, video, etc.).

---

## ECN (Explicit Congestion Notification)
- *2 bits*
- Allows endpoints to detect congestion **without packet loss**—if both hosts and the network support it.

---

## Total Length
- *16 bits*
- Total size of the IPv4 packet (header + encapsulated L4 data).
- Values range from **20 bytes** to **65,535 bytes**.

---

## Identification
- *16 bits*
- Labels fragments belonging to the same original packet.
- Required when a packet exceeds the link’s **MTU** (commonly 1500 bytes).
- Reassembly happens at the destination.

---

## Flags
- *3 bits*
- Control and describe fragmentation.
  - **Bit 0:** Reserved (always 0)
  - **Bit 1 – DF:** Do not fragment
  - **Bit 2 – MF:** More fragments follow (1) or final fragment (0)

---

## Fragment Offset
- *13 bits*
- Indicates the fragment’s position relative to the original packet.
- Allows correct reassembly even if fragments arrive out of order.

---

## Time To Live (TTL)
- *8 bits*
- Prevents perpetual looping.
- Decremented by each router; packet discarded at 0.
- Common default: **64**.

---

## Protocol
- *8 bits*
- Identifies the payload’s Layer 4 protocol.
- Common values:
  - **1:** ICMP
  - **6:** TCP
  - **17:** UDP
  - **89:** OSPF

---

## Header Checksum
- *16 bits*
- Validates the IPv4 header only.
- Routers recalculate this on each hop.
- Payload integrity is checked by TCP/UDP.

---

## Source and Destination IP Addresses
- *32 bits each*
- Identify sender and receiver.

---

## Options
- *0–320 bits*
- Rarely used; indicated when **IHL > 5**.


**For more explanation SEE [IPv4 Header](https://www.gatevidyalay.com/ipv4-ipv4-header-ipv4-header-format/)**

