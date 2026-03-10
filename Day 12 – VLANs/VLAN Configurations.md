

# Day 12 – VLANs – configuration.md  
*All VLAN-related configurations.  
future days (VLAN Types, Inter‑VLAN Routing, DTP, VTP will come in their own files).*

---

## 📘 1. Scenario Overview

Company: **TechVille Solutions**  
Switch: **Cisco Catalyst 2960**  
Goal: Create 3 VLANs for different departments:

- **VLAN 10 – IT Department**
- **VLAN 20 – HR Department**
- **VLAN 30 – Students Department**

Then:

- Assign ports to the correct VLANs  
- Verify VLAN creation and membership  
- Ensure proper switch behavior for access ports  
- (NO trunk configuration here — it belongs to future days)

---

## 🛠 2. VLAN Creation (Why, Where, and When)

We create VLANs when:

- We need network separation inside a switch  
- Departments or security zones must be isolated  
- We want to reduce broadcast traffic and improve performance  
- We need logical segmentation regardless of physical location  

We apply these configs **on a managed switch**, typically in a campus LAN.

---

## 🧩 3. VLAN Creation Commands  

```bash
configure terminal

vlan 10
 name IT_Department

vlan 20
 name HR_Department

vlan 30
 name Students_Department

end
```


## 🔍 4. Expected Result

Running:

```bash
show vlan brief


Should show:

    VLAN Name                             Status    Ports
    ---- -------------------------------- --------- ----------
    1    default                          active    Fa0/...
    10   IT_Department                    active
    20   HR_Department                    active
    30   Students_Department              active
end
```

**Explanation:**

*   The VLANs are now created but have **no ports** assigned yet.
*   They are active and ready for use.

***

## 🧱 5. Assigning Access Ports to VLANs

Each end device (PC, laptop, printer) connects to an **access port**, not a trunk port.

Our scenario says:

| Port  | User      | VLAN |
| ----- | --------- | ---- |
| Fa0/1 | IT User 1 | 10   |
| Fa0/2 | IT User 2 | 10   |
| Fa0/3 | HR User 1 | 20   |
| Fa0/4 | HR User 2 | 20   |
| Fa0/5 | Student 1 | 30   |
| Fa0/6 | Student 2 | 30   |

# Configuration:

```bash
configure terminal

interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 10

interface range fa0/3 - 4
 switchport mode access
 switchport access vlan 20

interface range fa0/5 - 6
 switchport mode access
 switchport access vlan 30

end
```

***

## 📊 6. Expected Result

```bash
show vlan brief
```

should now display:
```bash

    VLAN Name                Status   Ports
    ---- ------------------- -------- -----------------
    10   IT_Department       active   Fa0/1, Fa0/2
    20   HR_Department       active   Fa0/3, Fa0/4
    30   Students_Department active   Fa0/5, Fa0/6
end
```

**Explanation:**

*   Ports now belong to correct VLANs
*   HR and IT traffic are isolated
*   Students cannot see IT or HR data
*   Broadcasts stay inside their VLAN

***

# 🧪 7. Testing & Verification Commands

### Verify VLAN membership

```bash
show vlan brief
```

### Verify interface status

```bash
show interfaces status
```

### Verify which VLAN a port belongs to

```bash
show interfaces fa0/3 switchport
```

Expected output (example):
```bash

    Name: Fa0/3
    Administrative Mode: static access
    Access Mode VLAN: 20 (HR_Department)
    Operational Mode: static access
end
```
***

# ⚠ Common Mistakes (And How This Config Avoids Them)

| Mistake                                      | Why It Happens                  | How Our Config Solves It                |
| -------------------------------------------- | ------------------------------- | --------------------------------------- |
| Forgetting to enter `switchport mode access` | Switch ports default to dynamic | We explicitly defined access mode       |
| Assigning VLAN before creating it            | VLAN doesn’t exist yet          | We created VLANs before assignment      |
| Overlapping future topics (trunks/VTP)       | Causes confusion                | This file includes VLAN basics **only** |

***

# 🏁 Final Summary

This `configuration.md` contains:

*   VLAN creation
*   VLAN naming
*   Access port assignment
*   Verification commands
*   A real‑world company scenario
*   Expected results and explanations

There are upcoming days covering:

*   VLAN Types
*   Inter‑VLAN Routing
*   DTP
*   VTP

Those will each have their own configuration.md.

