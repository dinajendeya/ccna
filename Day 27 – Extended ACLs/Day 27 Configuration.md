# Day 27 – Extended ACLs – Configuration.md
*Practical configuration for Extended Access Control Lists (ACLs): service/port-based filtering, placement near source, and verification. This file covers **Extended ACLs only**. Standard ACLs are in Day 26.*

---
# 📘 1) Scenario Overview
Company: **BlueWave University**  
Edge Device: Cisco IOS Router (ISR/29xx/43xx/Packet Tracer)  

Networks & Policy:
- **Students VLAN** → 192.168.50.0/24 (Gi0/0)
- **Servers VLAN** → 10.1.1.0/24 (Gi0/1)
- **Internet/Uplink** → Serial0/0/0

Security goals (examples):
1. **Block SSH (TCP/22)** from Students to Servers, but **allow HTTP/HTTPS**.
2. **Deny ICMP echo** (ping) from Students to Servers.
3. **Allow DNS** from Students to *any* resolver (UDP/53) but deny other UDP to servers.
4. **Permit Admin subnet 10.10.10.0/24** full access to Servers.

**Placement rule:** Extended ACLs are placed **close to the source**. We will apply them **inbound** on the Students SVI/Layer‑3 interface (Gi0/0).

---
# 🛠 2) Named Extended ACL – STUDENTS_TO_SERVERS
Create a single, readable ACL with remarks and ordered statements.

```bash
configure terminal
ip access-list extended STUDENTS_TO_SERVERS
 remark === Allow essential web to servers ===
 permit tcp 192.168.50.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 80
 permit tcp 192.168.50.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 443
 remark === Allow DNS to anywhere (clients query resolvers) ===
 permit udp 192.168.50.0 0.0.0.255 any eq 53
 remark === Block SSH to servers ===
 deny tcp 192.168.50.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 22
 remark === Block ICMP echo to servers (allow other types by default rules) ===
 deny icmp 192.168.50.0 0.0.0.255 10.1.1.0 0.0.0.255 echo
 remark === Permit admin subnet full access to servers (overrides student policy) ===
 permit ip 10.10.10.0 0.0.0.255 10.1.1.0 0.0.0.255
 remark === Default: allow students to the Internet, but not to other UDP/TCP on servers ===
 permit ip 192.168.50.0 0.0.0.255 any
exit
```

**Why final `permit ip ... any`?**  
We want students to still reach the Internet via the uplink; only server‑targeted restrictions apply. If you instead want a strict policy, end with `deny ip 192.168.50.0 0.0.0.255 any` and add explicit permits above.

Apply the ACL **inbound** on the Students interface (closest to the source):
```bash
interface gig0/0
 description Students VLAN SVI
 ip access-group STUDENTS_TO_SERVERS in
end
```

---
# 🛠 3) Numbered Extended ACL Examples (Quick Patterns)

## A) Block Telnet (23) to Anywhere, but Allow Everything Else
```bash
configure terminal
access-list 110 deny tcp 192.168.50.0 0.0.0.255 any eq 23
access-list 110 permit ip 192.168.50.0 0.0.0.255 any
interface gig0/0
 ip access-group 110 in
end
```

## B) Allow HTTPS to a Single Server Only; Deny All Other Traffic to That Server
```bash
configure terminal
access-list 120 permit tcp any host 10.1.1.50 eq 443
access-list 120 deny ip any host 10.1.1.50
access-list 120 permit ip any any
interface gig0/0
 ip access-group 120 in
end
```

## C) Block ICMP from a Host; Permit the Rest
```bash
configure terminal
access-list 130 deny icmp host 192.168.50.77 any
access-list 130 permit ip any any
interface gig0/0
 ip access-group 130 in
end
```

---
# 🛠 4) Protect the Router Itself (VTY/Management) with an Extended ACL
Allow only Admin subnet to reach the router’s management services (SSH/HTTPS), block others.
```bash
configure terminal
ip access-list extended ROUTER_MGMT
 permit tcp 10.10.10.0 0.0.0.255 host 10.1.1.1 eq 22
 permit tcp 10.10.10.0 0.0.0.255 host 10.1.1.1 eq 443
 deny   ip any host 10.1.1.1
exit
line vty 0 4
 access-class ROUTER_MGMT in
 transport input ssh
 login local
end
```
*(Replace `10.1.1.1` with the router interface IP you manage.)*

---
# 🧪 5) Verification & Troubleshooting

### Show ACL contents and hit counters
```bash
show access-lists
show ip access-lists STUDENTS_TO_SERVERS
```

### Show ACL applied on interfaces
```bash
show ip interface gig0/0
```
Look for lines like:
```
Inbound access list is STUDENTS_TO_SERVERS
```

### Packet‑level testing (lab)
```bash
ping 10.1.1.10
ping 8.8.8.8
```

### Debug (use cautiously)
```bash
debug ip packet detail
undebug all
```

---
# 🔍 6) Best Practices & Notes
- **Order matters** → most specific `deny`/`permit` entries first.
- Remember the **implicit `deny ip any any`** at the end of numbered ACLs; named ACLs also have it if you don’t end with a permit.
- Apply **Extended ACLs near the source** to save bandwidth.
- Use **remarks** for documentation.
- For return traffic from the Internet, pair with stateful devices (firewall/CBAC/Zone‑Based Firewall) or explicit permits as required.

---
# ✅ 7) Summary
You now have:
- A comprehensive named Extended ACL enforcing student‑to‑server policy
- Quick numbered ACL patterns for common tasks
- Router management protection using Extended ACLs
- Full verification steps

This completes **Day 27 – Extended ACLs – Configuration.md**.
