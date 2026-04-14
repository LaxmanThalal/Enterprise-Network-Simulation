#Enterprise Network Simulation (Cisco Packet Tracer)

## 📌 Project Overview
A complete network design and implementation for a small, corporate branch. This topology demonstrates key CCNA-level concepts including logical segmentation, dynamic addressing, and inter-departmental mobility.

The design utilizes a **Hierarchical Network Model** (Access Layer and Distribution/Core Hybrid Layer) to ensure simplified management and secure traffic isolation.

## 🛠️ Technical Design Summary
* **Hardware:** 1x Cisco 2911 Router, 1x Cisco 2960 24-Port Switch.
* **Architecture:** 1 Router, 1 Switch, 3 Access Points, 9 end devices.
* **Segmentation:** Utilized **VLSM (Variable Length Subnet Masking)** to logically isolate departments into three VLANs, optimizing the ISP-provided `192.168.1.0/24` address space.
* **Inter-VLAN Routing:** Implemented **"Router-on-a-Stick"** (sub-interfaces) using **802.1Q encapsulation** for secure communication between subnets.
* **DHCP (Dynamic Host Configuration Protocol):** Configured local DHCP pools directly on the branch router to automate host addressing across all VLANs.

---

## 📐 Network Segmentation & VLSM Schema
The network is segmented into three logical zones. Below is the implemented addressing scheme derived using VLSM.

| Department | VLAN ID | Logical Block | Network Address | Subnet Mask | Usable Host Range | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Admin & IT** (Green Zone) | 10 | `/26` | `192.168.1.0` | `255.255.255.192` | `.2` – `.62` | `.1` |
| **Finance & HR** (Cyan Zone) | 20 | `/26` | `192.168.1.64` | `255.255.255.192` | `.66` – `.126` | `.65` |
| **Customer Care** (Orange Zone) | 30 | `/26` | `192.168.1.128` | `255.255.255.192` | `.130` – `.190` | `.129` |

---

## 💻 Configuration Highlights (CLI)
This section outlines the critical configuration commands used to realize this topology.

### 1. Switched Infrastructure (Switch3)
```bash
# Define VLANs
vlan 10
 name ADMIN_IT
vlan 20
 name FINANCE_HR
vlan 30
 name CUSTOMER_CARE

# Configure Access Ports (Admin & IT Zone)
interface range fastEthernet 0/2 - 4
 switchport mode access
 switchport access vlan 10

# Configure Trunk to Router
interface fastEthernet 0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
