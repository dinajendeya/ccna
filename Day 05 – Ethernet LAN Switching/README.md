
## Day 05 – Ethernet LAN Switching  

### Ethernet Frame Overview
**Canonical order (transmit/receive):** `Ethernet Header — PACKET (Layer‑3 data) — Ethernet Trailer`.

- **Ethernet Header fields:** `Preamble · SFD · Destination · Source · Type/Length`
  - **Preamble:** 7 bytes (56 bits), alternating 1s and 0s (`10101010` × 7). Synchronizes receiver clocks. 
  - **SFD (Start Frame Delimiter):** 1 byte (`10101011`). Marks the end of the preamble and start of the rest of the frame. 
  - **Destination:** 6 bytes (Layer 2 destination MAC address). 
  - **Source:** 6 bytes (Layer 2 source MAC address). 
  - **Type/Length:** 2 bytes. Values **≤ 1500** indicate **length** (bytes) of the encapsulated payload; values **≥ 1536** indicate **type** (e.g., IPv4/IPv6). 
    - **IPv4:** `0x0800` (2048 decimal) 
    - **IPv6:** `0x86DD` (34525 decimal)

- **Ethernet Trailer:** `FCS (Frame Check Sequence)` — 4 bytes (32 bits). Used for error detection via **CRC (Cyclic Redundancy Check)** on received data.

> **Size notes (both statements preserved):** One source description states **“Ethernet frame = 26 bytes (header + trailer)”**. Another (Part 2) clarifies **Preamble + SFD are not counted** in the Ethernet header; therefore **Header (14) + Trailer (4) = 18 bytes**. The **minimum frame** (Header + Payload + Trailer) is **64 bytes**, so the **minimum payload** is **46 bytes**; if payload < 46, **padding** bytes (zeros) are added until 46.

---

### MAC Address (48 bits)
- **Length:** 6 bytes (48 bits). Also called **Burned‑In Address (BIA)**; globally unique.
- **Structure:** First **3 bytes** = **OUI** (Organizationally Unique Identifier); last **3 bytes** = device‑unique portion.
- **Format:** 12 hexadecimal characters. Example: `E8:BA:70:11:28:74` (OUI `E8:BA:70`, device ID `11:28:74`).

**Interface Naming Example**
- `F0/1`, `F0/2`, ... where **F = Fast Ethernet (100 Mbps)**.

---

### Switch Forwarding Behavior & MAC Address Table
- Switches **dynamically learn** a **MAC Address Table** using the **source MAC** of incoming frames.
- **Unknown unicast** (destination MAC not in table): the switch **floods** out **all ports except** the incoming port.
- **Known unicast:** forwarded **only** out the specific egress port learned for the destination MAC.
- **Aging:** dynamically learned MAC entries are **removed after 5 minutes** of inactivity.

**Useful Verification/Operations (Privileged EXEC)**
```bash
# Show the ARP table on a host or L3 device
show arp

# Show the switch MAC address table
show mac address-table
# Output columns typically include: VLAN | MAC Address | Type | Ports

# Clear the entire dynamic MAC table
clear mac address-table dynamic

# Clear MAC entries learned on a specific interface
clear mac address-table dynamic interface <INTERFACE>
# e.g.: clear mac address-table dynamic interface f0/1
```

---

### ARP (Address Resolution Protocol)
- Purpose: Discover the **Layer 2 (MAC)** address that corresponds to a known **Layer 3 (IP)** address.
- **Messages:**
  - **ARP Request:** **broadcast** on the local network; contains **Source IP**, **Destination IP**, **Source MAC**, and **Broadcast MAC** (`FFFF.FFFF.FFFF`).
  - **ARP Reply:** **unicast** back to the requester; contains **Source IP**, **Destination IP**, **Source MAC**, **Destination MAC**.

---

### PING (ICMP Echo)
- **Function:** reachability testing and **round‑trip time** measurement.
- **Messages:** **ICMP Echo Request** and **ICMP Echo Reply**.
- **Traffic type:** **Unicast**.
- **Defaults (Cisco IOS):** sends **5** ICMP requests by default; **default payload size ~100 bytes**.
- **Indicators in output:** `.` (period) = **failed**, `!` (exclamation) = **successful** reply.

**Example**
```bash
ping 192.0.2.10
# !!!!!   # (5 successful replies)
# or
# ..!..    # (fail, fail, success, fail, success)
```

---

### Ethernet Frame Sizing & Padding (Part 2 highlights)
- **Preamble + SFD** are generally **not** considered part of the Ethernet header size for calculations.
- **Header (14 bytes) + Trailer (4 bytes) = 18 bytes**.
- **Minimum frame (header + payload + trailer) = 64 bytes** ⇒ **Minimum payload = 46 bytes**.
- **Padding** (zeros) is added if payload is smaller than 46 bytes.

---

### Quick Reference — Field Summary
```text
Preamble: 7 bytes, clock sync
SFD: 1 byte, marks start of frame
Destination: 6 bytes, destination MAC
Source: 6 bytes, source MAC
Type/Length: 2 bytes, ≤1500 length / ≥1536 type (e.g., IPv4 0x0800, IPv6 0x86DD)
FCS: 4 bytes, CRC error detection
```
