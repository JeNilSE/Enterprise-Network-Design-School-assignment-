# GlobalTech Solutions - Enterprise Network Design School Project

![Network Topology](Main%20Window.png)

## 🌄 Project Overview
This project contains the logical and physical network design for "GlobalTech Solutions" which is a made-up company. The goal was to build a scalable, secure, and redundant network infrastructure simulated in **Cisco Packet Tracer**.

**Design Philosophy:**
I designed the IP addressing scheme with scalability in mind. Floors 1 and 2 uses **/25 subnets**, while Floor 3 uses **/26**. This decision was made to ensure every department has at least **25% extra capacity** for future growth.

## 📀 Hardware & Topology
* **Core Layer:** Cisco 2911 Router (`R_Core`) handling WAN connectivity and EIGRP routing.
* **Distribution Layer:** 3x Cisco 3560-24PS Layer 3 Switches (`L3SW_V1`, `L3SW_V2`, `L3SW_V3`).
* **Access Layer:** 10x Cisco 2960 Switches distributed across three floors.
* **Redundancy:** Layer 3 EtherChannels (LACP) configured between floors for failover protection.

## 💻 IP Addressing & VLAN Structure
The network is segmented into VLANs for security and control:

| Floor | VLAN | Department | Subnet | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **Floor 1** | 10 | Marketing | 192.168.10.0/25 | .1 |
| | 20 | Sales | 192.168.10.128/25 | .129 |
| **Floor 2** | 30 | IT Dept | 192.168.20.0/25 | .1 |
| | 40 | Engineering | 192.168.20.128/25 | .129 |
| | 60 | Servers (DNS Core) | 192.168.21.0/27 | .1 |
| **Floor 3** | 50 | Management | 192.168.30.0/26 | .1 |
| | 51 | Finance | 192.168.30.64/26 | .65 |
| | 61 | Servers (Backup) | 192.168.31.0/27 | .1 |

## 🛣️ Routing Protocols & Design Choices
The network uses a multi-protocol strategy to simulate a complex enterprise environment. **Floor 2 (`L3SW_V2`) acts as the central redistribution point.**

### 1. EIGRP AS 100 (Core)
* **Scope:** Runs between `R_Core` and all Layer 3 switches.
* **Reasoning:** Chosen for its fast convergence and easy implementation in the Core. It distributes the default route to all floors.

### 2. RIP v2 (Floor 1 ↔ Floor 2)
* **Link:** `Po12` (192.168.200.0/30)
* **Reasoning:** Used for the simple Point-to-Point link to separate the segments. *Note: While rarely used in large modern production networks, it was required for the school assignment*

### 3. OSPF Area 0 (Floor 2 ↔ Floor 3)
* **Link:** `Po13` (192.168.200.4/30)
* **Reasoning:** Chosen as it is the industry standard for scalable routing with fast neighbor detection.

## ⛨ Security & Services
* **SSH v2:** Configured on all devices with RSA 1024-bit keys for secure management.
* **NAT/PAT:** Configured on `R_Core` (Gig0/0/0) to translate internal traffic to the ISP address (203.0.113.2).
* **DNS:** Redundant setup with a Primary DNS on Floor 2 and a manually mirrored Backup DNS on Floor 3.

## ᯓ🏃🏻‍♀️‍➡️ How to Run
1.  Ensure you have **Cisco Packet Tracer** installed (Version 8.2 recommended as the file doesn't work with 9.0).
2.  Download the `.pkt` file from this repository.
3.  Open the file to visualize the topology and inspect the CLI configurations.
4.  Use the simulation mode to test connectivity (e.g., PING from Marketing PC to Internet Server).

## 🧐** Detailed Topology Views
### Floor 1 (Marketing & Sales)
![Floor 1](CLUSTE~2.PNG)

### Floor 2 (IT & Engineering & Server)
![Floor 1](CLUSTE~3.PNG)

### Floor 3 (Management & Server)
![Floor 3](CLUSTE~4.PNG)

### ISP & Internet Edge
![ISP](CLUSTE~1.PNG)

## 🪪 Verification Commands
If you run the simulation, use these commands to verify the configuration:

* **Routing:** `show ip route` (Look for 'D' for EIGRP, 'R' for RIP, 'O' for OSPF)
* **Neighbors:** `show ip eigrp neighbors` | `show ip ospf neighbor`
* **Redundancy:** `show etherchannel summary` (Check for 'SU' or 'RU' flags)
* **Connectivity:** `ping dnscore.gts.local`

---
*Created by [Me](https://github.com/JeNilSE)*
