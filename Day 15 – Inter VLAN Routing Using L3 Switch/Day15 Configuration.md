# Day 15 – Inter‑VLAN Routing Using a Layer 3 Switch – configuration.md
*Configuration for SVI-based Layer 3 Routing with a real scenario, explanations, expected results.* 

## 1. Scenario Overview
Company: BlueWave University
Device: Cisco Catalyst Layer 3 Switch (e.g., 3560, 3850)

Goal: Allow routing between:
- VLAN 10 – Teachers – 192.168.10.0/24 (Gateway: 192.168.10.1)
- VLAN 20 – Students – 192.168.20.0/24 (Gateway: 192.168.20.1)
- VLAN 30 – Admin – 192.168.30.0/24 (Gateway: 192.168.30.1)

SVIs will be used instead of a router, and routing will occur inside the switch.

---
## 2. Create VLANs
```
configure terminal

vlan 10
 name Teachers

vlan 20
 name Students

vlan 30
 name Admin

end
```
---
## 3. Assign Ports to VLANs (Access Ports)
```
configure terminal

interface range fa0/1 - 10
 switchport mode access
 switchport access vlan 10

interface range fa0/11 - 20
 switchport mode access
 switchport access vlan 20

interface range fa0/21 - 24
 switchport mode access
 switchport access vlan 30

end
```
---
## 4. Enable Layer 3 Routing
```
configure terminal
ip routing
end
```
---
## 5. Create SVIs (Layer 3 Interfaces for VLANs)
```
configure terminal

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown

end
```
---
## 6. Configure Uplink as Trunk
```
configure terminal
interface gig0/1
 switchport mode trunk
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 10,20,30
end
```
---
## 7. Verification
### Check SVIs
```
show ip interface brief
```
Expected:
```
Vlan10   192.168.10.1   YES up   up
Vlan20   192.168.20.1   YES up   up
Vlan30   192.168.30.1   YES up   up
```
### Check Routing Table
```
show ip route
```
Expected:
```
C 192.168.10.0/24 is directly connected, Vlan10
C 192.168.20.0/24 is directly connected, Vlan20
C 192.168.30.0/24 is directly connected, Vlan30
```
### Test Connectivity
From VLAN 10 device:
```
ping 192.168.20.50
```
Should reply successfully.

---
## 8. Summary
This file includes:
- VLAN creation
- Access port assignment
- SVI creation
- Enabling IP routing
- Trunk configuration
- Verification steps and expected results

No overlap with Day 14 (Router-on-a-Stick).
