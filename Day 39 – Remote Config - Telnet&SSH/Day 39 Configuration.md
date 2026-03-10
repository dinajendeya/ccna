# Day 39 – Remote Config (Telnet & SSH) – configuration.md
*Configuration commands for secure remote access using Telnet (legacy) and SSH (recommended), with explanations, verification, and expected behavior.*

---
# 📘 1. Scenario Overview
Company: **BlueWave University**  
Devices: **Cisco switches & routers** used across multiple buildings.

**Goals**
- Enable remote management on network devices.
- Understand the difference between **Telnet** (insecure) and **SSH** (secure).
- Configure device login, user accounts, VTY access, SSH keys, and banners.
- Restrict remote access using ACLs.

**Assumptions**
- Device already has basic IP connectivity and a management VLAN.
- Device hostname and domain name will be configured for SSH key generation.

---
# 🛠 2. Basic Device Identity Requirements (Hostname + Domain)
SSH requires both a hostname and a domain name.

```bash
configure terminal
hostname SW-IT-BUILDING1
ip domain-name bluewave.local
end
```

---
# 🛠 3. Create Local User Accounts (Used by Telnet & SSH)
It is a best practice to use local usernames with privilege levels.

```bash
configure terminal
username admin privilege 15 secret StrongPassword123
username support privilege 5 secret UnivSupport123
end
```

**Explanation**
- `privilege 15` = full admin access
- `secret` encrypts passwords using MD5 or modern hashing

---
# 🛠 4. VTY Lines – Allow Remote Login (Common for Telnet & SSH)
```bash
configure terminal
line vty 0 15
 login local
 transport input ssh telnet
end
```

**Explanation**
- All VTY lines use `login local` → require username/password.
- Both Telnet and SSH allowed (SSH will later be preferred).

---
# 🛠 5. Telnet Setup (Not Secure – For Labs Only)
Telnet sends all passwords in clear text → unsafe for production.

```bash
configure terminal
line vty 0 15
 transport input telnet
 password telnet123    ! Only used if login local is not configured
end
```

**Warning:** Only use Telnet in controlled lab environments.

---
# 🛠 6. SSH Setup (Secure – Recommended)
Generate RSA keys required for SSH.

```bash
configure terminal
crypto key generate rsa general-keys modulus 2048
ip ssh version 2
end
```

**Explanation**
- `modulus 2048` = strong encryption
- SSHv2 is more secure and must be forced

---
# 🛠 7. Restrict Remote Access Using an ACL (Highly Recommended)
Allow only IT management network (e.g., 10.1.100.0/24) to access the device.

```bash
configure terminal
ip access-list standard MGMT-SSH
 permit 10.1.100.0 0.0.0.255
 deny   any
!
line vty 0 15
 access-class MGMT-SSH in
end
```

**Explanation**
- Blocks remote login attempts from unauthorized networks.

---
# 🛠 8. Login Banner (Legal Notice)
```bash
configure terminal
banner motd ^
************************************************
* Authorized Personnel Only
* Unauthorized access is prohibited.
************************************************^
end
```

---
# 🛠 9. Optional – Disable Telnet Completely
```bash
configure terminal
line vty 0 15
 transport input ssh
end
```

---
# 🧪 10. Verification Commands & Expected Output

### ✔ Check SSH Status
```bash
show ip ssh
```
**Expected:** SSH enabled, version 2, key size 2048.

### ✔ Test VTY Access Rules
```bash
show running-config | section vty
```
**Expected:** `login local`, SSH allowed, ACL applied.

### ✔ Check Local Users
```bash
show running-config | include username
```
**Expected:** Correct user accounts listed.

### ✔ Check RSA Keys
```bash
show crypto key mypubkey rsa
```
**Expected:** Public key data displayed.

### ✔ Attempt Remote Login (From PC)
```bash
ssh admin@192.168.10.5
```
Expected: Successful login with encrypted communication.

### ✔ Telnet Test (If enabled)
```bash
telnet 192.168.10.5
```
Expected: Works only if Telnet is allowed; not secure.

---
# 🔍 11. Expected Behavior in This Lab
- Telnet works, but is vulnerable to packet sniffing.
- SSH provides secure, encrypted remote access.
- Users authenticate with local usernames.
- ACLs restrict which networks can remotely manage the device.
- Banner appears before login.
- Device is fully manageable from the IT admin network.

---
# ⚠ Common Remote Access Misconfigurations
| Issue | Symptom | Fix |
|-------|---------|------|
| No hostname/domain set | SSH key generation fails | Set hostname + ip domain-name |
| RSA key too small | SSH rejects client | Use 1024–2048 bits |
| ACL too strict | Admins locked out | Update access-list |
| `login local` missing | Cannot login with usernames | Add to VTY lines |
| Telnet left enabled | Risk of password theft | Disable Telnet (SSH only) |

---
# 🏁 Final Summary
This configuration file includes:
- Local user accounts and VTY login
- Telnet (legacy) and SSH (secure) remote access
- RSA key generation and SSHv2 settings
- Optional Telnet disablement
- Management ACLs to restrict access
- Verification steps to confirm security

SSH is the modern, recommended standard for all secure remote device management.
