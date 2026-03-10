
# Day 14 – Inter‑VLAN Routing (Using a Router) – configuration.md  
*Router-on-a-Stick configuration with a real scenario, explanations, expected results*
---

# 📘 1. Scenario Overview

Company: **IUG University**  
Network Devices:  
- **Cisco Layer 2 Switch (Catalyst 2960)**  
- **Cisco Router (e.g., 2911, 1841, ISR)**  

Goal:  
Enable communication between 3 VLANs using a **single physical router interface**:

| VLAN | Purpose | Subnet | Default Gateway |
|------|----------|---------|------------------|
| 10 | Teachers | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Students | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Admin | 192.168.30.0/24 | 192.168.30.1 |

Switch uses trunk port **Gig0/1** to connect to the router’s **Gig0/0** port.  
Router performs all routing between VLANs via **sub‑interfaces**.

---

# 🔧 2. Why Router-on-a-Stick?

We use this method when:

- The switch is *Layer 2 only* (cannot perform routing itself)
- We want VLANs to communicate
- Network is small or low‑traffic
- Lab/CCNA practice requires it

This method DOES NOT scale well for high‑traffic networks —  
that’s why **Day 15 uses a Layer 3 switch instead**.

---

# 🛠 3. Switch Configuration (VLANs + Trunk)

Even though VLAN setup was shown in previous days,  
for completeness, we show only what's needed **to support routing**:

```bash
configure terminal

vlan 10
 name Teachers

vlan 20
 name Students

vlan 30
 name Admin

interface gig0/1
 switchport mode trunk
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 10,20,30

end
```

### 🔍 Why this config?

*   The router needs all VLANs tagged → trunk mode
*   Router-on-a-stick **requires 802.1Q tags** to map frames to sub‑interfaces
*   Only the required VLANs are allowed for security

***

# 🚀 4. Router Configuration (Sub‑Interfaces)

This is the **core** of Router-on-a-Stick.

The router takes ONE physical interface and splits it into **three virtual interfaces**, one per VLAN.

```bash
configure terminal

interface gig0/0
 no shutdown

! VLAN 10 – Teachers
interface gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

! VLAN 20 – Students
interface gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

! VLAN 30 – Admin
interface gig0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

end
```

***

# 🧠 5. Explanation of Each Command

### `interface gig0/0.10`

Creates a **sub‑interface** for VLAN 10.

### `encapsulation dot1Q 10`

Says:  
➡ “Handle frames tagged with VLAN 10.”

### `ip address 192.168.10.1 255.255.255.0`

This is the **default gateway** for all VLAN 10 devices.

***

# 🧪 6. Host Configuration (Example PCs)

PC in VLAN 10:

    IP Address: 192.168.10.50
    Subnet Mask: 255.255.255.0
    Default Gateway: 192.168.10.1

PC in VLAN 20:

    IP Address: 192.168.20.60
    Subnet Mask: 255.255.255.0
    Default Gateway: 192.168.20.1

This ensures all inter‑VLAN traffic reaches the router.

***

# 🧭 7. Verification Commands

### On the Switch

```bash
show vlan brief
show interfaces trunk
```

Expected output:
```bash

    VLANs allowed on trunk: 10,20,30
    Trunking mode: on
```

***

### On the Router

#### Check sub‑interfaces:

```bash
show ip interface brief
```

Expected:
```bash

    Gig0/0.10  192.168.10.1  YES up  up
    Gig0/0.20  192.168.20.1  YES up  up
    Gig0/0.30  192.168.30.1  YES up  up
```

## Check routing table:

```bash
show ip route
```

Expected:
```bash

    C 192.168.10.0/24 is directly connected, Gig0/0.10
    C 192.168.20.0/24 is directly connected, Gig0/0.20
    C 192.168.30.0/24 is directly connected, Gig0/0.30
```

This confirms the router is routing between VLANs.

***

# 🖧 8. Expected Connectivity Test

From a Teacher PC (VLAN 10):

```bash
ping 192.168.20.60
```

Expected:

    Reply from 192.168.20.60: bytes=32 time=1 ms TTL=64

This confirms Inter‑VLAN Routing is working.

***

# ⚠ Common Misconfigurations (and how we avoid them)

| Problem                        | Why It Happens                  | How This File Prevents It              |
| ------------------------------ | ------------------------------- | -------------------------------------- |
| Missing `encapsulation dot1Q`  | Router won’t map VLANs          | Every sub‑interface includes it        |
| Forgotten trunk mode           | Router never receives VLAN tags | Switch config sets trunk explicitly    |
| Wrong default gateway on hosts | PCs can’t route                 | Correct gateways shown in host section |
| Allowing all VLANs             | Security risk                   | Only 10,20,30 allowed                  |

***

# 🏁 Final Summary

This configuration file includes:

*   VLAN trunking for router link
*   Router-on-a-stick with sub‑interfaces
*   Proper IP addressing
*   Verification & expected results
*   Real-world university scenario
*   next day we will disscuss inter-vlan-routing using L3 Switch