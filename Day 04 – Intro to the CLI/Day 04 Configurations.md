# CCNA Configurations — Day 04 (Intro to the CLI)

## 1) Console Access (Local, First-Time Configuration)
**Why:** Establish out-of-band access to a new or misconfigured device.
**Where:** PC ↔ Cisco device **Console** port using a terminal emulator (Serial).

```bash
# Configure terminal emulator (example values)
# Serial line: COM3          # Use your actual COM port
# Speed (baud): 9600         # Default Cisco console speed
# Data bits: 8
# Stop bits: 1
# Parity: None
# Flow control: None
```
**Result:** Terminal opens to the device's login/banner (or directly to the CLI prompt on lab gear).

---

## 2) Moving Between CLI Modes
**Why:** Different tasks require different privilege levels.  
**Where:** From User EXEC to Privileged EXEC; into/out of Global Configuration.

```bash
# From User EXEC (hostname>) to Privileged EXEC (hostname#)
enable

# Enter Global Configuration Mode
configure terminal
# or: conf t

# Exit back to Privileged EXEC
exit
```
**Result:**
- Prompt changes `>` → `#` when you enter **enable**.  
- Prompt changes to `(config)#` in **global config**; returns to `#` on **exit**.

---

## 3) Set Privileged Password (legacy) — `enable password`
**Why:** Protect Privileged EXEC access on older systems or for lab parity.  
**Where:** Global configuration; **superseded** by `enable secret` if both exist.

```bash
configure terminal
enable password MyLegacyPass123
exit
```
**Result:** Privileged EXEC requires a password; stored in plaintext **unless** you enable global password encryption (see §5).

---

## 4) Set Privileged Hashed Secret — `enable secret`
**Why:** Stronger protection for Privileged EXEC; takes precedence over `enable password`.  
**Where:** Global configuration.

```bash
configure terminal
enable secret MyStrongSecret!@#
exit
```
**Result:** The device stores a **hashed** secret (commonly shown as type 5 in configs). When both are set, the **secret** is used for authentication.

---

## 5) Obfuscate Plaintext Passwords — `service password-encryption`
**Why:** Prevent plain text display of passwords such as `enable password` or line passwords in the configuration.  
**Where:** Global configuration; affects existing and future plaintext-type passwords.

```bash
configure terminal
service password-encryption
exit
```
**Result:** Existing eligible passwords are encrypted (legacy reversible style), and future ones are stored encrypted. Disabling later does **not** decrypt existing entries.

---

## 6) View Active vs Boot Config — `show running-config` / `show startup-config`
**Why:** Verify current and saved configurations.  
**Where:** Privileged EXEC.

```bash
show running-config
show startup-config
```
**Result (snippet):**
```bash
Building configuration...
Current configuration : 2543 bytes
!
version 15.2
service password-encryption
!
enable secret 5 $1$abCDe/FG$hijkLMNOPQRSTuvWXyz01
!
line con 0
 login local
!
end
```

---

## 7) Save Changes — `copy running-config startup-config` (or `write` / `wr mem`)
**Why:** Persist current (RAM) configuration to NVRAM so it survives reloads.  
**Where:** Privileged EXEC.

```bash
# Explicit copy
do copy running-config startup-config
# On prompt, press Enter to accept the default filename

# Legacy equivalents
write
write memory
```
**Result:**
```bash
Destination filename [startup-config]? 
Building configuration...
[OK]
```

---

## 8) Remove/Disable a Setting — `no <command>`
**Why:** Roll back or disable a configuration previously applied.  
**Where:** Global configuration (or appropriate sub-mode) matching the original command’s scope.

```bash
configure terminal
no service password-encryption
exit
```
**Result:** New plaintext-type passwords will no longer be stored encrypted; existing encrypted passwords remain encrypted; `enable secret` remains in effect.

---

## 9) Quick Mode/Help Aids — `?` and TAB
**Why:** Discover available commands and speed up typing.  
**Where:** Any mode.

```bash
# List commands available in current mode
?

# List completions for a partial command
show ru?

# Use TAB to auto-complete
show running-config    # (after typing 'show run' + TAB)
```
**Result:** Context-aware lists or auto-completion of valid commands.

---

## 10) End-to-End Hardening Flow (Example)
**Why:** Minimal baseline to protect privileged access and persist configuration changes.  
**Where:** Console session on a new switch/router.

```bash
# Enter privileged and global configuration
enable
configure terminal

# Set strong privileged secret
enable secret S3cure!P@ssw0rd

# Obfuscate eligible plaintext passwords (if any used elsewhere)
service password-encryption

# (Optional) Set a legacy enable password for lab parity only
enable password LegacyLabOnly123

# Exit to privileged and save
end
write memory
```
**Result:**
- Privileged access requires the **strong secret**.
- Plaintext-type passwords (if present) are encrypted in the config.
- Configuration is saved to **startup-config**.
