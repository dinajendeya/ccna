
# **Day 51 Virtualization**

## **1. What Is Virtualization?**

Virtualization is the technology that allows multiple operating systems or network devices to run on a single physical machine by sharing its hardware resources.  
In networking, it helps create virtual routers, switches, firewalls, and servers.

***

## **2. Why Virtualization Matters in CCNA**

Virtualization appears in CCNA because modern networks rely heavily on:

*   Virtual machines (VMs)
*   Virtual network functions (VNF)
*   Cloud infrastructure (like AWS, Azure)
*   Virtualized routers/switches (CSR1000v, Nexus 9000v)

Ccna expects you to understand *concepts*, not to configure hypervisors.

***

## **3. Types of Virtualization**

### **a. Server Virtualization**

Runs multiple server OS instances on one physical server.

Examples:

*   Windows Server VM
*   Linux VM

### **b. Network Virtualization**

Creates virtual versions of networking devices and network segments.

Examples:

*   Virtual switches (vSwitch, DVS)
*   Virtual routers (CSR1000v)
*   VLANs (logical network segmentation)

### **c. Desktop Virtualization**

Runs a desktop environment from a central server.

Example:

*   VDI (Virtual Desktop Infrastructure)

### **d. Storage Virtualization**

Combines multiple storage devices into one logical pool.

***

## **4. Key Virtualization Components**

### **Hypervisor**

Software that manages VMs and allocates CPU, RAM, storage, and NIC resources.

#### **Type 1 (Bare‑Metal Hypervisors)**

Runs directly on hardware.  
Examples:

*   VMware ESXi
*   Microsoft Hyper‑V
*   KVM

#### **Type 2 (Hosted Hypervisors)**

Runs on top of an operating system.  
Examples:

*   VMware Workstation
*   Oracle VirtualBox

***

## **5. Virtual Network Elements**

### **Virtual Network Interface Cards (vNICs)**

Allow VMs to communicate with each other and the physical network.

### **Virtual Switches**

Simulate Layer 2 switch functions.

Features include:

*   VLAN support
*   MAC address table
*   Trunking

### **Virtual Routers**

Software-based routers that provide routing and WAN services.

***

## **6. Benefits of Virtualization**

*   **Cost savings** (fewer physical devices)
*   **Scalability** (quick deployment of VMs)
*   **Flexibility** (test labs, development environments)
*   **High availability** (VM migration, snapshots)
*   **Efficient resource usage**

***

## **7. Virtualization in Cisco Networking**

*   Cisco uses virtualization in:
    *   **Cisco CSR1000v** (virtual router)
    *   **Cisco ASA 1000v** (virtual firewall)
    *   **Cisco Nexus 1000v** (virtual switch)
*   Network automation and SDN rely heavily on virtualization.

***

## **8. Basic CCNA-Level Terms**

| Term    | Meaning                                      |
| ------- | -------------------------------------------- |
| VM      | Virtual Machine                              |
| VMM     | Virtual Machine Monitor (same as hypervisor) |
| vSwitch | Virtual switch for connecting VMs            |
| vNIC    | Virtual network interface card               |
| VNF     | Virtual Network Function                     |

***

## **9. How Virtualization Relates to Cloud**

Cloud platforms = virtualization + automation.  
Public cloud examples:

*   AWS
*   Azure
*   Google Cloud

You don’t configure the hypervisor — cloud providers manage that.
*See Day 52 - cloud Computing*

***

## **10. Summary**

Virtualization is a key foundation for:

*   Cloud computing
*   Software‑defined networking
*   Modern enterprise networks

For CCNA, focus on knowing **concepts**, **hypervisor types**, and how virtual devices work. not deep configuration.
