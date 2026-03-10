
# CCNA Configurations — Day 11 (Basic Static Route Configuration)

## 1) Configure a Static Route Using Next-Hop IP
**Why:** Directs traffic to a specific next-hop router to reach a destination network.  
**Where:** Global configuration mode.

```bash
R1(config)# ip route <DESTINATION-NETWORK> <SUBNET-MASK> <NEXT-HOP-IP>
```

**Result:** Router forwards packets for the destination network to the specified next-hop IP.

---

## 2) Configure a Static Route Using Exit Interface
**Why:** Useful on point‑to‑point links where the router knows exactly which interface to send traffic through.  
**Where:** Global configuration mode.

```bash
R1(config)# ip route <DESTINATION-NETWORK> <SUBNET-MASK> <EXIT-INTERFACE>
```

**Result:** Router sends traffic directly out the specified interface. Particularly beneficial on serial point‑to‑point links.

---

## 3) Example Static Route (Next-Hop Method)
**Why:** Defines a clear path toward a remote network using a next-hop router.  
**Where:** Global configuration.

```bash
R1(config)# ip route 192.168.4.0 255.255.255.0 10.1.1.2
```

**Result:** R1 forwards packets destined for **192.168.4.0/24** to the next-hop **10.1.1.2**.

---

## 4) Example Static Route (Exit Interface Method)
**Why:** Often used on point‑to‑point serial links where the outgoing interface is known.  
**Where:** Global configuration.

```bash
R1(config)# ip route 192.168.4.0 255.255.255.0 g0/0/0
```

**Result:** Traffic to **192.168.4.0/24** is sent out the **gigaeathernet0/0/0** interface.

---

## 5) Configure a Default Route
**Why:** Used when the destination network is unknown—forward traffic to a gateway of last resort.  
**Where:** Global configuration.

```bash
R1(config)# ip route 0.0.0.0 0.0.0.0 <NEXT-HOP-IP>
```

**Result:** Router forwards all packets without a matching route to the specified next-hop, typically an ISP or core router.

---
