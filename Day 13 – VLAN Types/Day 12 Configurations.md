
# Day 13 – VLAN Types – configuration.md  
Configuration for Default, Data, Management, and Native VLANs  

---

# 📘 1. Scenario Overview

Company: **AUC University**  
Network Device: **Cisco Catalyst 2960 Switch**  
Goal: Implement the following VLAN types:

- **Default VLAN (1)** → Will NOT be used for users (security best practice)
- **Data VLANs** → VLAN 10 (Teachers), VLAN 20 (Students)
- **Management VLAN** → VLAN 99 (for remote SSH/HTTPS/SNMP)
- **Native VLAN** → VLAN 999 (for trunk links, avoiding VLAN 1)

We will:

- Create all VLAN types  
- Assign ports to data VLANs  
- Configure a Management SVI  
- Assign an IP for remote access  
- Configure the Native VLAN on a trunk  
- Verify each component  
- Provide expected results  

---

# 🧩 2. VLAN Creation (Default, Data, Management, Native)

Why we do this:

- **Default VLAN**: exists automatically → no need to configure  
- **Data VLANs**: separate departments and reduce broadcast traffic  
- **Management VLAN**: allows secure remote switch administration  
- **Native VLAN**: receives untagged frames on trunk links  

---

# 🛠 3. Create the VLANs

```bash
configure terminal

vlan 10
 name Teachers_VLAN

vlan 20
 name Students_VLAN

vlan 99
 name Management_VLAN

vlan 999
 name Native_VLAN

end
```

***

## 🔍 4. Expected Result

Running:

```bash
show vlan brief
```

You should see:
```bash

    VLAN Name               Status    Ports
    ---- ------------------ --------- ------------------------
    1    default            active    Fa0/...
    10   Teachers_VLAN      active
    20   Students_VLAN      active
    99   Management_VLAN    active
    999  Native_VLAN        active
```

### Explanation:

*   All VLANs are created
*   Default VLAN (1) is untouched
*   No ports assigned yet

***

## 🧱 5. Assign Access Ports to Data VLANs

Scenario:

| Port  | User Type | VLAN |
| ----- | --------- | ---- |
| Fa0/1 | Teacher 1 | 10   |
| Fa0/2 | Teacher 2 | 10   |
| Fa0/3 | Student 1 | 20   |
| Fa0/4 | Student 2 | 20   |

```bash
configure terminal

interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 10

interface range fa0/3 - 4
 switchport mode access
 switchport access vlan 20

end
```

***

## 📊 6. Expected Result

```bash
show vlan brief
```

Should display:

```bash
    10  Teachers_VLAN      active   Fa0/1, Fa0/2
    20  Students_VLAN      active   Fa0/3, Fa0/4
    99  Management_VLAN    active
    999 Native_VLAN        active
```

Explanation:

*   Teachers and students are separated
*   VLAN 1 is untouched
*   Management and Native VLANs ready for use

***

## 🔐 7. Configure the Management VLAN (VLAN 99)

Why:

*   Allows remote SSH, HTTPS, SNMP access
*   Keeps management traffic isolated from users
*   Industry best practice

```bash
configure terminal

interface vlan 99
 ip address 192.168.99.10 255.255.255.0
 no shutdown

ip default-gateway 192.168.99.1

end
```

***

# 🧭 8. Expected Result

```bash
show ip interface brief
```

You should see:

```bash

    Vlan99     192.168.99.10   YES manual up     up
```

Explanation:

*   The switch now has a reachable IP
*   Remote configuration is now possible

***

## 🟧 9. Configure Native VLAN on the Trunk Port

Scenario:

Switch connects to another switch on port **Gig0/1**.  
We must:

*   Configure a trunk
*   Change the Native VLAN to **999**
*   Avoid VLAN 1 (security best practice)

```bash
configure terminal

interface gig0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,99,999

end
```

***

# 📡 10. Expected Result

```bash
show interfaces gig0/1 switchport
```

Output snippet:

```bash
    Administrative Mode: trunk
    Operational Mode: trunk
    Trunking Native Mode VLAN: 999
    Administrative Native VLAN tagging: disabled
    Trunking VLANs Allowed: 10,20,99,999
```

Verification that:

*   Native VLAN is not 1
*   Only required VLANs are allowed
*   The trunk is stable and clean

***

## 🧪 11. Additional Verification Commands

### Check device VLAN membership

```bash
show vlan brief
```

### Check trunk status

```bash
show interfaces trunk
```

### Validate management reachability

From another device:

```bash
ping 192.168.99.10
```

### Check what VLAN a port belongs to

```bash
show interfaces fa0/3 switchport
```

***

# ⚠ Common Mistakes (Avoided by Our Config)

| Mistake                     | Why It’s Bad           | How We Fixed It                   |
| --------------------------- | ---------------------- | --------------------------------- |
| Using VLAN 1                | Major security risk    | We use VLAN 999 as Native         |
| Using management on VLAN 1  | Exposes switch         | Management = VLAN 99              |
| Allowing all VLANs on trunk | Unnecessary & insecure | Only allowed required VLANs       |
| Not setting access mode     | Ports become dynamic   | Explicit `switchport mode access` |

***

# 🏁 Final Summary

This `configuration.md` includes:

*   VLAN creation (Data, Management, Native)
*   Assigning ports to data VLANs
*   Creating a Management SVI for remote access
*   Configuring a secure Native VLAN on a trunk
*   Real-world university network scenario
*   Verification steps and expected results

* Future days we will disscus:

- Inter‑VLAN Routing → **Day 14**
- DTP / VTP → **Day 15**

All of those will have their own configuration.md.

