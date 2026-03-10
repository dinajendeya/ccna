### Rapid Spanning Tree Protocol (RSTP)

## Purpose & Improvements
- Faster convergence than classic STP; handshake‑driven, not timer‑driven.  
- Based on IEEE **802.1w**; Cisco implementation = Rapid‑PVST+.  

## RSTP Similarities with STP
- Prevents loops, elects Root Bridge, same port cost & BPDU logic.  

## RSTP Differences
### Port States
- **Discarding** (replaces Blocking/Listening), **Learning**, **Forwarding**.  

### Port Roles
- **Root Port** & **Designated Port** same as STP.  
- Non‑designated split into:
  - **Alternate Port**: Backup path to Root Port; instant failover.  
  - **Backup Port**: Backup for Designated Port (rare; requires hub).  

### Built‑in Enhancements
- RSTP integrates **UplinkFast** and **BackboneFast** automatically.  

## RSTP BPDU Behavior
- All switches generate BPDUs every 2 seconds.  
- Switch considers neighbor lost after 3 missed BPDUs (6s).  

## RSTP Link Types
- **Edge**: Host-facing; immediate forwarding (PortFast behavior).  
- **Point‑to‑Point**: Full‑duplex switch‑to‑switch links.  
- **Shared**: Half‑duplex, hub‑based segments.  

## Compatibility
- RSTP interoperates with STP; RSTP ports fall back to STP when required. 

---


