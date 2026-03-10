
# Day 11 – Routing Fundamentals & Static Routing

## What is Routing?
Routing is the decision‑making process routers use to determine the best path for IP packets to reach their destination across interconnected networks. Routers depend on a **routing table**, which contains all known network routes and the next-hop information required to forward packets.

Routers perform three essential actions:
1. **Forward to next-hop** when the destination is reachable through another router.
2. **Send directly** when the destination network is directly connected.
3. **Deliver locally** when the destination IP matches one of the router’s own interfaces.

---

## Routing Table Learning Methods
Routers populate their routing tables using one of two mechanisms:

### **1. Dynamic Routing**
- Routers exchange routing information automatically using routing protocols.
- Example: **OSPF**.
- Routers adapt to network changes without manual intervention.

### **2. Static Routing**
- Routes are manually configured by a network engineer.
- Offers full control, but does not adapt automatically to topology changes.

---

## WAN Overview
A **WAN (Wide Area Network)** spans a large geographic area, connecting multiple LANs. Routers interconnect LANs across WAN links to enable end‑to‑end communication.

---

# Day 11 (Part 2) – Static Routing

## Review
- **Switches** → forward traffic *within* a LAN.
- **Routers** → forward traffic *between* LANs.
- **WANs** → interconnect LANs across long distances.

---

## Static Routes
Static routes define explicit paths for packets. A static route provides the router with one of the following:
- A **next-hop IP** to use when forwarding traffic.
- An **exit interface** for directly connected forwarding.
- Knowledge of a network reachable through manual configuration.
- **Local** means it's the ip address on one of the router interfaces usually with 32 prefix.
- **connected** means this network is directly connected with one of the routers interfaces.

Static routing is predictable, secure, and low-overhead, but requires manual updates when the network changes.

---

## Basic Static Route Configuration

### Configure a static route using **next-hop IP**:
```ios
R1(config)# ip route <DESTINATION-NETWORK> <SUBNET-MASK> <NEXT-HOP-IP>
```

### Configure a static route using **exit interface**:
```ios
R1(config)# ip route <DESTINATION-NETWORK> <SUBNET-MASK> <EXIT-INTERFACE>
```

---

## Example Static Route Using Next-Hop
```ios
R1(config)# ip route 192.168.4.0 255.255.255.0 10.1.1.2
```
This tells R1:
“To reach **192.168.4.0/24**, send packets to the next-hop **10.1.1.2**.”

---

## Example Static Route Using Exit Interface
```ios
R1(config)# ip route 192.168.4.0 255.255.255.0 s0/0/0
```
Useful on point‑to‑point links.

---

## Default Route
A default route is used when **no specific route** to a destination exists.

### Configure a default route:
```ios
R1(config)# ip route 0.0.0.0 0.0.0.0 <NEXT-HOP-IP>
```
Routers rely on this route to handle unknown destinations — typically used to forward traffic toward an upstream ISP or a core router.

---

