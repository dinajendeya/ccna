
# CCNA Configurations — Day 06 (IP Addressing & Interface Basics)

## 1) Enter Privileged EXEC Mode

```bash
# Enter Privileged EXEC Mode
R1> enable
```

---

## 2) View Interfaces & Their Status

```bash
R1# show ip interface brief
```
---

## 3) Enter Global Configuration Mode

```bash
R1# conf t
```
---

## 4) Configure an Interface with IPv4 Address
**Why:** Enable router interface to participate in the network.
**Where:** Global Config → Interface Config.

```bash
R1(config)# interface g0/0
R1(config-if)# ip address 10.255.255.254 255.0.0.0
R1(config-if)# no shutdown
```

**Result:** Interface transitions to `up/up` when link is present.

---

## 5) Verify Interface Configuration
**Why:** Confirm correct addressing and status.
**Where:** Within interface mode using `do`.

```bash
R1(config-if)# do show ip interface brief
```

**Result:** Displays updated interface information.

---

## 6) Add an Interface Description
**Why:** Helps document interface purpose.
**Where:** Interface configuration mode.

```bash
R1(config)# interface g0/0
R1(config-if)# description ## to SW1 ##
```

**Result:** `show running-config` shows the new description.

---

## 7) Verify Descriptions
**Why:** Ensure documentation was applied.
**Where:** Privileged EXEC.

```bash
R1# show interfaces description
```

**Result:** Displays interface status and description.

