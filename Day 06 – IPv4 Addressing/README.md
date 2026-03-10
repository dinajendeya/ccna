
# Day 06 – IPv4 Addressing

## The Story Behind IPv4: How It All Started
Before computers could "talk" to each other, they needed a universal way to identify themselves.  
Imagine a world where **every device is a house**, and the network is a giant city.  
To deliver any message, you need:

- a **street address** (so the mailman knows where to go)  
- a **consistent system** to assign these addresses  
- a **rulebook** explaining how messages travel  

In the early days of the internet (the 1970s), the U.S. Department of Defense created a project called **ARPANET** — the very early version of the global internet.  
As more computers connected to ARPANET, engineers realized:

➡️ *We need a structured addressing system, or no device will know where to send data.*

This is how **IPv4** was born.

In 1981, IPv4 was officially documented in **RFC 791** — a standard that defines how every device should receive an address.

This single invention allowed computers all over the world to communicate reliably.  
Without IPv4, there would be **no internet**, no Google, no social media, no email — nothing.

---

## 🌍 2. What Is an IPv4 Address?
An **IPv4 address** is a unique identifier assigned to every device on a network.  

It consists of:

- **4 numbers**  
- each between **0–255**  
- separated by dots  
- total size: **32 bits**

Example in binary:


11000000.10101000.00000001.00001010

Why 32 bits?  
Because in the 1970s–1980s, engineers believed:

💡 *“4.3 billion addresses is more than enough — no one will ever need more.”*

Back then, they had no idea smartphones, IoT, laptops, smart TVs, and billions of devices would exist.

---

## 🎯 3. Why Do We Use IPv4?
### ✔ Simple  
It’s easy to read, easy to configure, easy to understand.

### ✔ Works Everywhere  
Routers, switches, operating systems, firewalls — all built their foundation on IPv4.

### ✔ Fueled the Growth of the Internet  
IPv4 was the addressing system during the rise of:

- Google  
- Facebook  
- YouTube  
- Online gaming  
- Cloud services  

### ✔ Supported by NAT  
When IPv4 started running out of addresses, engineers invented:

- **NAT**  
- **PAT**  
- **Private IP ranges**

These solutions **extended the life** of IPv4 (we still use it today because of NAT).

---


### OSI Model – Layer 3 (Network Layer)
- Connects different networks (outside LAN)
- Provides logical addressing (IP addresses)
- Provides path selection (routing)
- Routers operate at Layer 3

### Routing Basics
Switches (Layer 2) extend LANs, but do NOT separate networks.
Routers (Layer 3) create separate networks.

**Example networks:**
- `192.168.1.0/24`
- `192.168.2.0/24`

Each router interface gets a unique IP:
- G0/0: `192.168.1.254/24`
- G0/1: `192.168.2.254/24`

Hosts on same LAN share network portion:
```
192.168.1.100
192.168.1.105
192.168.1.205
```
Routers do NOT forward broadcast traffic.

---

## IPv4 Header
- Main Layer 3 protocol
- 32-bit addresses (4 octets)
- Example:
```
192.168.1.254 → 11000000.10101000.00000001.11111110
```
Each 8 bits = 1 octet.

---

## Decimal → Binary Conversion
Example:
Binary: `10001111`
```
128 + 8 + 4 + 2 + 1 = 143
```

Binary: `01110110`
```
64 + 32 + 16 + 4 + 2 = 118
```

---

## Decimal → Binary Conversion (Subtraction Method)
Example: 221
Result: `11011101`

Example: 127
Result: `01111111`

---

## IPv4 + Prefix
Example:
```
192.168.1.254/24
```
/24 = first 24 bits = network portion

---

## IPv4 Address Classes
| Class | First Bits | Range |
|-------|------------|--------|
| A     | 0xxxxxxx   | 0–126 |
| B     | 10xxxxxx   | 128–191 |
| C     | 110xxxxx   | 192–223 |
| D     | 1110xxxx   | 224–239 (Multicast) |
| E     | 1111xxxx   | 240–255 (Experimental) |

Loopback: `127.0.0.0/8`

---

## Netmasks
- Class A /8 → `255.0.0.0`
- Class B /16 → `255.255.0.0`
- Class C /24 → `255.255.255.0`

---

## Network & Broadcast Addresses
Example Class C:
```
Network:   192.168.1.0
Broadcast: 192.168.1.255
Usable:    1–254
```

---

## Hosts Per Network
Formula:
```
Hosts = 2^N – 2
```
(N = number of host bits)

### Class C /24
`2^8 – 2 = 254`

### Class B /16
`2^16 – 2 = 65,534`

### Class A /8
`2^24 – 2 = 16,777,214`

---

## First & Last Usable IPs
### Class C
```
Network: 192.168.1.0
First:   192.168.1.1
Last:    192.168.1.254
Broadcast:192.168.1.255
```

### Class B
```
Network: 172.16.0.0
First:   172.16.0.1
Last:    172.16.255.254
Broadcast:172.16.255.255
```

### Class A
```
Network: 10.0.0.0
First:   10.0.0.1
Last:    10.255.255.254
Broadcast:10.255.255.255
```

---

## Cisco CLI Summary
### Enter privileged mode
```
R1> enable
```

### Show interfaces
```
R1# show ip interface brief
```

### Enter configuration mode
```
R1# conf t
```

### Configure interface
```
R1(config)# int g0/0
R1(config-if)# ip address 10.255.255.254 255.0.0.0
R1(config-if)# no shutdown
```

### Verify interface
```
R1(config-if)# do show ip interface brief
```

### Add description
```
R1(config)# int g0/0
R1(config-if)# description ## to SW1 ##
```
