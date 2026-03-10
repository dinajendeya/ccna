

## Day 02 – Interfaces and Cables

### Switch Ports
- Switches typically provide many access ports (commonly 24) using RJ‑45 connectors for Ethernet.

### What Is Ethernet and Why Standards Matter
- Ethernet is a collection of network vendor-nutral protocols and standards that ensure interoperable communication and hardware compatibility.

### Data Rate Units
- Speeds are measured in bits per second (bps).
- Bit = 0 or 1; Byte = 8 bits.
- Kb = 1,000 bits; Mb = 1,000,000 bits; Gb = 1,000,000,000 bits; Tb = 1,000,000,000,000 bits.

### Ethernet Governance
- Ethernet standards are defined under IEEE 802.3 (established in 1983).

### Copper Ethernet Standards (Twisted Pair)
- 10 Mbps — Ethernet — 802.3i — 10BASE‑T — 100 m max.
- 100 Mbps — Fast Ethernet — 802.3u — 100BASE‑T — 100 m max.
- 1 Gbps — Gigabit Ethernet — 802.3ab — 1000BASE‑T — 100 m max.
- 10 Gbps — 10 Gigabit Ethernet — 802.3an — 10GBASE‑T — 100 m max.

### Copper Cable Notes
- BASE = Baseband signaling; T = Twisted Pair.
- UTP (Unshielded Twisted Pair) uses twisting to reduce electromagnetic interference (EMI).
- Typical UTP has 8 wires (4 pairs).
- 10/100BASE‑T uses 2 pairs; 1000BASE‑T and 10GBASE‑T use all 4 pairs.

### RJ‑45 Pin Usage (10/100BASE‑T)
- PCs: transmit on pins 1–2, receive on pins 3–6.
- Switches: receive on pins 1–2, transmit on pins 3–6.
- Complementary mapping enables full‑duplex operation.

### Straight‑Through vs Crossover
- Router/PC to Switch: use straight‑through.
- Like‑to‑like (PC–PC, switch–switch, router–router): use crossover that swaps pairs 1↔3 and 2↔6.

### Device TX/RX Summary (10/100)
- Router / Firewall / PC: TX 1–2, RX 3–6.
- Switch: TX 3–6, RX 1–2.

### Auto MDI‑X and Higher‑Speed Copper
- Auto MDI‑X automatically detects neighbor transmit pins and adjusts receive pins.
- 1000BASE‑T and 10GBASE‑T use all 4 pairs; each pair is bidirectional for higher throughput.

### Fiber‑Optic Basics
- High‑speed fiber is standardized under IEEE 802.3ae.
- SFP (Small Form‑Factor Pluggable) transceivers provide fiber connectivity; fiber typically uses separate strands for transmit and receive.

### Fiber Types
- Single‑Mode Fiber (SMF): narrow core, laser transmitter, longest distances, higher cost.
- Multimode Fiber (MMF): wider core, LED transmitter, shorter than SMF but longer than UTP, lower cost than SMF.

### Common Fiber Standards
- 1000BASE‑LX (802.3z): 1 Gbps; MMF/SMF; up to 550 m (MMF) or 5 km (SMF).
- 10GBASE‑SR (802.3ae): 10 Gbps; MMF; up to 400 m.
- 10GBASE‑LR (802.3ae): 10 Gbps; SMF; up to 10 km.
- 10GBASE‑ER (802.3ae): 10 Gbps; SMF; up to 30 km.

### UTP vs Fiber — Quick Comparison
- UTP: lower cost, up to ~100 m, susceptible to EMI, RJ‑45 ports are cheaper, emits faint signals externally (potential security risk).
- Fiber: higher cost, much longer distances, immune to EMI, SFP ports cost more (SMF optics > MMF optics), no external signal emission (reduced security risk).
