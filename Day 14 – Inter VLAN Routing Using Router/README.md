# Day 14 – Inter‑VLAN Routing (Using a Router)

> **Note:** All configuration commands for Day 14 are in a separate `configuration.md` file.  
> Day 15 will cover **Inter‑VLAN Routing using a Layer 3 Switch (SVI Routing)** and explain how it differs from the router-based method described here.

---

# 🧠 1. The Story: Why Do We Need Inter‑VLAN Routing?

In Day 12 and Day 13, we learned that VLANs separate a network into isolated sections:

- VLAN 10 → IT  
- VLAN 20 → HR  
- VLAN 30 → Students  

This improves security and reduces broadcast traffic.  
But it also creates a new problem:

➡️ **Devices in different VLANs cannot communicate**, even if they’re in the same physical building.

Example:

- A printer in VLAN 10  
- A student device in VLAN 30  

If a student wants to print a document, they **cannot reach** the printer because VLANs are **separate networks**.

This separation is good for organization and security,  
but in real life **networks need controlled communication between VLANs.**

And that’s where **Inter‑VLAN Routing** comes in.

---

# 🧩 2. What Is Inter‑VLAN Routing?

**Inter‑VLAN Routing = enabling communication between VLANs through a routing device.**

VLANs are Layer 2 networks.  
To connect Layer 2 networks together, you must use a **Layer 3 device**.

In this day (Day 14), we focus on:

## 💡 Inter‑VLAN Routing using a Router  
Also known as:

- *Router-on-a-Stick*  
- *One-armed Router*  

This is the classic and foundational method.

---

# 🏫 3. Simple High-School Analogy

Imagine a school with 3 separate floors:

- Floor 1 → IT  
- Floor 2 → HR  
- Floor 3 → Students  

There are walls between them.  
No student can go to HR.  
No IT employee can go to Students.  

But sometimes they need to send documents to each other.

So the school brings a **mailman**.

The mailman moves between floors and delivers messages correctly.

That mailman = **the router**.

---

# 🔥 4. How Router-on-a-Stick Works (Core Idea)

A switch has many VLANs:  
- VLAN 10  
- VLAN 20  
- VLAN 30  

The router has **only one physical interface** connected to the switch.

But we want this router to handle traffic from **all VLANs**.

Solution:

👉 We divide the router’s physical interface into **sub‑interfaces**,  
like:

- `Gig0/0.10` → handles VLAN 10  
- `Gig0/0.20` → handles VLAN 20  
- `Gig0/0.30` → handles VLAN 30  

Each sub‑interface acts like a **virtual router port** for that VLAN.

So one cable can carry traffic for many VLANs.

---

# 📡 5. The Role of the Switch Trunk

To transport multiple VLANs from the switch to the router,  
the switch port connecting to the router must be a **trunk**.

Trunking adds tags to frames so:

- VLAN 10 frames go to sub‑interface 10  
- VLAN 20 frames go to sub‑interface 20  
- VLAN 30 frames go to sub‑interface 30  

The router reads the tags and routes traffic accordingly.

This is why it's called **Router-on-a-Stick** —  
multiple VLANs “ride” on one cable.

---

# 🧠 6. Why Inter‑VLAN Routing Using a Router Is Still Important

Even though modern networks often use L3 switches (Day 15),  
Router-on-a-Stick remains essential because:

- It's used in labs and CCNA exams  
- It explains the concept of routing between VLANs  
- Small networks still use it  
- It teaches sub‑interfaces and 802.1Q encapsulation  

Understanding this method makes learning L3 switching easier later.

---

# 🔍 7. Router-on-a-Stick: Step-by-Step Explanation

### 1. Create VLANs on the switch  
(Already covered in Days 12 & 13)

### 2. Make the switch port a trunk  
So multiple VLANs can travel to the router.

### 3. Create sub‑interfaces on the router  
One sub‑interface per VLAN.

### 4. Assign IP addresses  
Each sub‑interface becomes the **default gateway** for its VLAN.

### 5. Routing takes place  
Now devices in different VLANs can communicate.

---

# 🧭 8. Example of Inter‑VLAN Communication

PC in VLAN 10 wants to reach a server in VLAN 20.

Steps:

1. PC sends packet to its default gateway (`192.168.10.1`)  
2. Router receives it on sub‑interface `.10`  
3. Router routes the packet to sub‑interface `.20`  
4. Switch delivers packet to server in VLAN 20  
5. Response goes back the same way  

This is classic IP routing.

---

# 🧠 9. When Should You Use Router-on-a-Stick?

### ✔ Suitable for:
- Small offices  
- Learning environments  
- Cheap networks without L3 switches  
- CCNA labs  

### ❌ Not suitable for:
- High traffic environments  
- Campus networks  
- Large enterprise networks  

Reason: The router’s single link becomes a **bottleneck**.

---

# 🔄 10. Relationship with Day 15 (L3 Switch Routing)

Today (Day 14) we cover:

### ✔ Inter‑VLAN Routing using a Router  
- Uses sub‑interfaces  
- Uses a trunk  
- Slower, limited scalability  
- Traditional method

Tomorrow (Day 15) will cover:

### ✔ Inter‑VLAN Routing using a Layer 3 Switch  
- Switch performs routing internally  
- Faster (hardware-based)  
- Uses SVIs (Interface VLAN X)  
- No need for router-on-a-stick  
- Enterprise standard today

Think of Day 14 as the **foundation**,  
and Day 15 as the **modern high-performance version**.

---

# 📝 11. Reminder

This is the **README.md** for Day 14.  
All related configurations are in the file:

➡ **Day 14 configuration.md**

---
