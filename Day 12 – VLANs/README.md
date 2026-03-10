# Day 12 – VLANs

> **Note:** We will later discuss related topics such as **VLAN Types, Inter‑VLAN Routing, DTP, VTP**, but **not in this section**.  
> This section focuses **only on VLAN fundamentals**.

---

## 🧠 1. The Story: Why VLANs Were Invented

Imagine a big school building:

- Students from all grades walk in the same hallways  
- They all hear every announcement  
- They all get caught in the same traffic  

Even if 9th grade students have nothing to do with 12th grade students, they are physically mixed together.

This is exactly what early networks looked like:

- Every computer was in **one big LAN**  
- Broadcasts went to **everyone**  
- Anyone could **sniff traffic** from other departments  
- The network was **slow, noisy, and insecure**

Engineers asked:

➡️ *"How can we separate departments logically even if they share the same switch?"*

That question led to the invention of:

💡 **VLANs — Virtual Local Area Networks**

A VLAN divides one physical switch into many **separate logical networks**.

---

## 🧩 2. What Is a VLAN?

**VLAN = Virtual LAN**

A VLAN creates a **logical separation** inside a switch, even if all devices are plugged into the **same physical box**.

It works like building invisible walls inside the network.

### Without VLANs:
- All devices are in the same broadcast domain  
- Anyone can access anyone      
- Broadcast storms slow everything  

### With VLANs:
- Each VLAN is isolated  
- Broadcasts stay inside their VLAN  
- Traffic is separated  
- Security is increased  

---

## 🏫 3. Simple High‑School Analogy

Imagine this classroom is a network switch.

If we create VLANs:

- **VLAN 10 = IT Department**  
- **VLAN 20 = HR Department**  
- **VLAN 30 = Students**  
- **VLAN 40 = Security Cameras**  

Even if all cables plug into the same switch, each VLAN behaves like a **separate building** with:

- separate hallways  
- separate announcements  
- separate security

---

## 📡 4. The Most Important Concept: **Broadcast Domains**

### Without VLANs:
One switch = **One broadcast domain**

When one device sends a broadcast (ARP, DHCP Discover), everyone receives it.

### With VLANs:
Each VLAN = **its own broadcast domain**

This reduces:

- network congestion  
- unnecessary traffic  
- security risks

This alone is why VLANs are essential.

---

## 🧱 5. How VLANs Work on a Switch

A managed switch has a **VLAN table**.

When you assign a port to a VLAN:

- That port becomes a member of that VLAN  
- Devices connected to that port belong to that VLAN  
- They can only communicate directly with devices in the **same VLAN**  
- To talk to other VLANs → you need a **router** (we will discuss Inter‑VLAN Routing later)

---

## 🔢 6. VLAN Numbering & Naming

A VLAN has:

- **VLAN ID** → a number between **1–4094**  
- **VLAN Name** → descriptive label

Examples:

| VLAN ID | Name |
|---------|---------|
| 10 | Staff |
| 20 | Students |
| 30 | HR |
| 40 | Cameras |
| 99 | Management |
| 100 | Guest WiFi |

Default VLAN is **VLAN 1**.

---

## 🏗 7. Types of Ports: Access vs Trunk

> **Note:** We will discuss DTP and VLAN Types later.  
> Here we only explain the basics needed for VLANs.

### ✔ Access Port  
- Belongs to **ONE VLAN only**  
- Used by end devices (PCs, printers, phones)  
- Example:  
switchport mode access
switchport access vlan 10

### ✔ Trunk Port  
- Carries **multiple VLANs** across switches  
- Used between:
- Switch ↔ Switch  
- Switch ↔ Router  
- Switch ↔ Firewall  
- Tags frames using **802.1Q**

Example:

switchport mode trunk
switchport trunk allowed vlan 10,20,30

---

## 🚀 8. How Frames Travel in VLANs (Important for CCNA)

When a frame leaves a switch port:

### Access Port  
- Frame leaves **untagged**  
- Belongs to only one VLAN

### Trunk Port  
- Frame gets a **tag** added (802.1Q)  
- Tag tells the receiving switch:  
  “This frame belongs to VLAN X”

---

## 🔐 09. Why Are VLANs Important?

### ✔ Performance  
Fewer broadcasts = faster network.

### ✔ Security  
HR cannot see IT traffic.  
Students cannot access cameras.

### ✔ Organization  
Makes network design clean and scalable.

### ✔ Flexibility  
You can move users across the building and keep them in the same VLAN.

---

## 🧠 10. VLAN Summary (Memorize This)

- VLAN = Virtual LAN  
- VLANs create **logical** separation inside a switch  
- Each VLAN = separate broadcast domain  
- Access ports → one VLAN  
- Trunk ports → many VLANs  
- VLANs need a router to communicate between them  
- Improves performance, structure, and security  

---

## 🚧 11. What Comes Next?

We will discuss these VLAN-related topics **in later sections**, but NOT in this one:

- **Day 13 – VLAN Types**  
- **Day 14 & 15 – Inter‑VLAN Routing**  
- **Day 16 – DTP / VTP**  

---