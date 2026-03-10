
# Day 11 – Subnetting & VLSM  

## 🧠 1. The Story: Why Subnetting Was Invented

In the early days of networking, IPv4 addresses were given out in **huge fixed blocks**:

- Class A → 16 million addresses  
- Class B → 65,536 addresses  
- Class C → 256 addresses  

This caused a major problem:

➡️ **Most networks didn’t need that many addresses**, so thousands were wasted.

Imagine giving a classroom of 20 students a building with 256 rooms.  
This waste would eventually **exhaust IPv4 addresses**, and networks would become messy and slow.

Engineers needed a way to:

- Organize networks  
- Reduce unnecessary traffic  
- Save IPv4 addresses  
- Improve performance  
- Make routing more structured  

💡 **Subnetting** was created to solve all of these problems.

---

## 🧩 2. What Is Subnetting?

**Subnetting = dividing a large network into smaller, more efficient networks (subnets).**

Each subnet becomes:

- Its own broadcast domain  
- More secure  
- More organized  
- Easier to manage  

Think of dividing a big school into grades → halls → classrooms.

---

## 🎯 3. Why Do We Use Subnetting?

### ✔ Reduce Broadcast Traffic  
Smaller subnets = fewer broadcasts = faster network.

### ✔ Save IPv4 Addresses  
Use only what is needed.

### ✔ Increase Security  
Different departments = different subnets.

### ✔ Better Network Organization  
Routers can easily route between clean subnet boundaries.

---

## 🧮 4. Subnet Masks Explained 

A **subnet mask** tells devices:

- What part of the IP address is the **network**
- What part is the **host**

Example:

```

IP:         192.168.1.20
Subnet Mask:255.255.255.0

```

Meaning:

- First 3 octets = network  
- Last octet = host  

CIDR notation:  
```

/24 = 255.255.255.0

```

---

## 🔢 5. CIDR Notation  

CIDR = **Classless Inter‑Domain Routing**  
The number after `/` = number of **network bits**.

| CIDR | Subnet Mask         | Usable Hosts |
|------|----------------------|--------------|
| /24  | 255.255.255.0        | 254          |
| /25  | 255.255.255.128      | 126          |
| /26  | 255.255.255.192      | 62           |
| /27  | 255.255.255.224      | 30           |
| /28  | 255.255.255.240      | 14           |
| /29  | 255.255.255.248      | 6            |
| /30  | 255.255.255.252      | 2            |

---

## 🧠 6. Subnetting Formulas  
### Hosts formula  
```

Hosts = 2^H – 2

    (H = host bits)

    ### Subnets formula  

Subnets = 2^N

    (N = borrowed bits)

    ### Block size  

Block Size = 256 – mask value

```

---

## 📐 7. Subnetting Example (Equal-Sized Subnets)

Network:  
```

192.168.1.0/24

```

Goal:  
Create **4 subnets**.

### Step 1 — Determine borrowed bits  
Need 4 subnets →  
```

2 bits → 2^2 = 4

```

### Step 2 — New subnet mask  
```

/24 → /26

```

### Step 3 — Block size  
```

256 – 192 = 64

```

### Step 4 — List the subnets

| Subnet | Network | First Host | Last Host | Broadcast |
|--------|----------|-------------|------------|------------|
| 1 | 192.168.1.0/26     | .1   | .62  | .63  |
| 2 | 192.168.1.64/26    | .65  | .126 | .127 |
| 3 | 192.168.1.128/26   | .129 | .190 | .191 |
| 4 | 192.168.1.192/26   | .193 | .254 | .255 |

---

# 🔥 8. What Is VLSM?

**VLSM = Variable Length Subnet Mask**

Subnetting creates *equal-sized* subnets.  
But real networks need *different-sized* subnets.

Example:

- IT → 50 hosts  
- HR → 20 hosts  
- Sales → 10 hosts  
- CCTV → 4 hosts  

If you use equal subnets → tons of waste.

💡 **VLSM allows you to create subnets of different sizes.**

This is how real networks are designed.

---

## 🎯 9. Why Use VLSM?

- Saves massive numbers of IPv4 addresses  
- Lets you match subnet sizes to actual needs  
- Makes routing tables more efficient  
- Used in all enterprise networks  

---

## 🚀 10. VLSM Step-by-Step Method

1. **List all required subnets**  
2. **Sort them from largest → smallest**  
3. **Start with the largest subnet**  
4. **Assign it the lowest available address range**  
5. **Repeat for each smaller network**

---

## 📘 11. Complete VLSM Example

Main network:
```

192.168.1.0/24

```

Departments need:

- 50 hosts  
- 20 hosts  
- 10 hosts  
- 4 hosts  

### Step 1 — Sort needs (largest first)
- 50 → /26  
- 20 → /27  
- 10 → /28  
- 4 → /29  

---

### Step 2 — Assign /26 (50 hosts)

| Network | Range | Broadcast |
|---------|--------|-----------|
| **192.168.1.0/26** | .1–.62 | .63 |

---

### Step 3 — Assign /27 (20 hosts)

| Network | Range | Broadcast |
|---------|--------|-----------|
| **192.168.1.64/27** | .65–.94 | .95 |

---

### Step 4 — Assign /28 (10 hosts)

| Network | Range | Broadcast |
|---------|--------|-----------|
| **192.168.1.96/28** | .97–.110 | .111 |

---

### Step 5 — Assign /29 (4 hosts)

| Network | Range | Broadcast |
|---------|--------|-----------|
| **192.168.1.112/29** | .113–.118 | .119 |

---

# 🧠 12. Key Differences: Subnetting vs VLSM

| Feature | Subnetting | VLSM |
|---------|-------------|-------|
| Subnet Size | Equal | Variable |
| Address Efficiency | Medium | Very High |
| Real-World Usage | Limited | Everywhere |
| Flexibility | Low | High |

---

# 🏁 Final Summary

- **Subnetting** divides a network into equal-sized subnets.  
- **VLSM** divides a network into different-sized subnets to match real needs.  
- Both save IPv4 addresses and improve performance.  
- VLSM is the modern, flexible, real-world solution.  
- Mastering both is essential for CCNA and real job roles like SOC & networking.

