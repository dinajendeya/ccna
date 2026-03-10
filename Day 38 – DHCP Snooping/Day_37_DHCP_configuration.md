# Day 37 – DHCP – configuration.md
*Practical DHCP (IPv4/IPv6) configurations with explanations, real scenarios, verification, and expected behavior.*

---
# 📘 1. Scenario Overview
Company: **BlueWave University**  
Devices: Cisco **Layer 3 switch** (SVIs as gateways), a central **DHCP server** *or* the switch itself acting as DHCP server; multiple VLANs.

**Goals**
- Provide automatic IPv4 addressing for multiple VLANs using one centralized server.
- Use **DHCP relay** on SVIs when the server is on another subnet.
- Demonstrate **scope options**, **exclusions**, **reservations**, and **voice (Option 150)**.
- Show quick **IPv6** variants (stateful + stateless).
- Verify operations and typical outputs.

**Topology (simplified)**
```
[Clients VLAN10]--(Access)-->[L3 Switch: SVI VLAN10/VLAN20/VLAN30]--(Routed)-->[Data Center]
                                                                                                                               +-->[Wireless Controller / APs]

Central DHCP server: 10.10.10.10 (in Data Center)
Gateway addresses: VLAN10=192.168.10.1, VLAN20=192.168.20.1, VLAN30(Voice)=192.168.30.1
```

---
# 🛠 2. Option A – L3 Switch as the **DHCP Server** (Per‑VLAN Pools)
Use this when the switch should hand out addresses directly.

> Create exclusions first (gateways/servers), then pools with options.

```bash
configure terminal
!
! Exclude gateway and infrastructure addresses
ip dhcp excluded-address 192.168.10.1 192.168.10.50
ip dhcp excluded-address 192.168.20.1 192.168.20.50
ip dhcp excluded-address 192.168.30.1 192.168.30.20
!
! VLAN 10 – Staff
ip dhcp pool VLAN10_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 1.1.1.1
 domain-name bluewave.local
 lease 1 0 0          ! 1 day
!
! VLAN 20 – Students (shorter lease for churn)
ip dhcp pool VLAN20_POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.4.4 1.0.0.1
 domain-name bluewave.local
 lease 0 8 0          ! 8 hours
!
! VLAN 30 – Voice (Cisco IP phones need Option 150)
ip dhcp pool VOICE_VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 option 150 ip 10.1.1.10        ! TFTP/CallManager address
 dns-server 8.8.8.8
 domain-name voice.bluewave.local
 lease 3 0 0          ! 3 days
end
```


---
# 🛠 3. Wireless & Guest VLAN Tweaks (Conceptual + Minimal Config)
For guest WLANs, leases should be **short** to recycle addresses. Ensure SSID → VLAN mapping lands in the right scope.

```bash
configure terminal
ip dhcp pool GUEST_VLAN50
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.1
 dns-server 9.9.9.9 149.112.112.112
 lease 0 2 0    ! 2 hours
end
```

---
# 🛠 4. Quick IPv6 Variants (On the Same L3 Switch)
Enable IPv6 routing once, then choose a model.

```bash
configure terminal
ipv6 unicast-routing
end
```

**Stateless (SLAAC + DHCPv6 for DNS only)**
```bash
configure terminal
interface Vlan10
 ipv6 address 2001:db8:10:10::1/64
 ipv6 nd other-config-flag         ! Clients use SLAAC for address, DHCPv6 for "other info"
 ipv6 dhcp server V6_DNS_ONLY
!
ipv6 dhcp pool V6_DNS_ONLY
 dns-server 2001:4860:4860::8888
 domain-name bluewave.local
end
```

---
# 🧪 5. Verification Commands & Expected Outputs

### ✔ Server/Pool Status (On IOS DHCP server)
```bash
show ip dhcp pool
```
**Expected**: Utilization per pool, network, available/leased addresses.

```bash
show ip dhcp binding
```
**Expected**: MAC/Client-ID ↔ IP mappings with lease expiry.

```bash
show ip dhcp server statistics
```
**Expected**: Packet counters for Discover/Offer/Request/Ack, declines, releases.


### ✔ Client-Side Hints
- Windows: `ipconfig /all`, `ipconfig /renew`
- Linux/macOS: `dhclient -v`, `ifconfig`/`ip a`
**Expected**: Correct IP, mask, gateway, DNS. Ping gateway and DNS to confirm.

### ✔ IPv6
```bash
show ipv6 dhcp binding
show ipv6 interface vlan 10
```
**Expected**: Clients have addresses per chosen model, correct DNS/domain.

### ✔ Debug (Use carefully, not in production hours)
```bash
debug ip dhcp server events
undebug all
```
**Expected**: Real-time log of requests, offers, acks.

---
# 🔍 6. Expected Behavior in This Lab
- Clients in VLAN10/20/30 receive correct addresses and options.
- Guest VLAN reuses addresses quickly due to short leases.
- IPv6 clients behave according to SLAAC/stateless/stateful selections.

---
# ⚠ Common DHCP Misconfigurations (and Fixes)
- **Wrong scope gateway/DNS** → Clients can’t route or resolve. *Fix:* Correct options in scope/pool.
- **No exclusions** → DHCP leases gateway/server IPs by accident. *Fix:* Add `ip dhcp excluded-address`.
- **Firewall blocks UDP 67/68** between relay and server. *Fix:* Allow DHCP.

---
# 🏁 Final Summary
This configuration file shows:
- Local IOS **DHCP server** pools with exclusions, reservations, and options.
- **IPv6** stateless/stateful and relay examples.
- Comprehensive **verification** steps and expected outcomes.
