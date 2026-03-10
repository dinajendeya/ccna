# Day 40 – Wireless LANs – configuration.md
*Basic Wireless LAN configuration using **GUI only**, aligned with CCNA scope. **see my screenshots folder**

---
# 📘 1. Scenario Overview
You are configuring Wi‑Fi using **GUI interfaces only**, because:
- CCNA does **not** expect CLI configuration of WLCs.
- Most real wireless deployments use a **Wireless LAN Controller (WLC)** GUI.

We will create three Wi‑Fi networks:
- **BlueWave‑Staff** → VLAN 10
- **BlueWave‑Students** → VLAN 20
- **BlueWave‑Guest** → VLAN 50

Access Points automatically join the WLC and broadcast the SSIDs.

---
# 🛠 2. WLC GUI – Create a New WLAN (SSID)
Steps apply to common Cisco WLC GUI (3504/5520/9800 WebUI style).

### ✔ Step 1 — Log in to the WLC
Open browser:
```
https://<WLC-IP>
```
Login with your admin credentials.

### ✔ Step 2 — Go to WLANs
GUI Navigation:
```
WLANs > Create New
```

### ✔ Step 3 — Enter WLAN Information
Fill in:
- **Profile Name:** STAFF
- **SSID:** BlueWave-Staff
- **WLAN ID:** 1

Click **Apply**.

### ✔ Step 4 — Map WLAN to VLAN
Navigate:
```
WLANs > (Select WLAN) > General
```
Set:
- **Interface:** VLAN10

### ✔ Step 5 — Configure Security
Navigate:
```
WLANs > (Select WLAN) > Security > Layer 2
```
Choose:
- **WPA2 Personal** (PSK)
- **Passphrase:** StaffPass123

Click **Apply**.

---
# 🛠 3. Repeat for Students and Guest SSIDs
Repeat the same steps for:

### ✔ Students WLAN
- **SSID:** BlueWave-Students
- **WLAN ID:** 2
- **VLAN:** 20
- **Password:** Student1234

### ✔ Guest WLAN
- **SSID:** BlueWave-Guest
- **WLAN ID:** 3
- **VLAN:** 50
- **Password:** GuestWifi

Guest VLAN typically has:
```
Internet-only access
Short DHCP lease (2 hours)
```

These settings are done on the network switch or firewall (not in CCNA).

---
# 🛠 4. GUI – Verify Access Points Are Joined
Navigate:
```
Wireless > Access Points > All Access Points
```
You should see:
- AP Name
- AP Model
- Joined status: **Connected**
- Assigned AP Group

If an AP is not joined:
- Check DHCP Option 43
- Check AP connected to correct VLAN
- Ensure controller discovery methods are correct (CAPWAP)

(Concept only; CCNA does not require config.)

---
# 🛠 5. Client Connectivity Testing
On a wireless client (laptop/phone):
1. Open Wi‑Fi menu
2. Select **BlueWave‑Staff** (or Students/Guest)
3. Enter password
4. Verify IP address obtained via DHCP
5. Test:
```
ping default gateway
ping google.com
```

If no IP:
- Ensure VLAN interfaces have DHCP helper address
- Check WLAN → Interface mapping

---
# 🧪 6. GUI Verification
### ✔ WLAN Summary
```
WLANs > Summary
```
Shows:
- All SSIDs
- Status: Enabled/Disabled
- VLAN assignments
- Security type

### ✔ Client Summary
```
Monitoring > Clients
```
You should see:
- Username/MAC
- IP Address
- SSID connected
- AP they joined

### ✔ AP Summary
```
Monitoring > Access Points
```
Shows all joined APs and radio status.

---
# 🏁 Final Summary
This configuration file intentionally uses **GUI-only steps**, because:
- GUI is **within CCNA scope**
- CLI for WLC is **beyond CCNA**

You learned how to:
- Create WLANs (SSIDs)
- Map SSIDs to VLANs
- Configure WPA2 security
- Verify AP join
- Check connected clients

This is a complete basic Wireless LAN setup suitable for CCNA-level labs.
