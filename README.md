 Enterprise Campus Network Design & Implementation
Multi-Site Hierarchical Infrastructure with Redundancy, Security, and Unified Communications
---
Project Overview
This project presents a comprehensive Enterprise Campus Network designed and simulated using Cisco Packet Tracer. The network follows a three-tier hierarchical model (Core → Distribution → Access) with full redundancy, security hardening, and unified communications support across multiple departments.
> **Domain:** `enterprisenetdesign.com`  
> **Simulation Tool:** Cisco Packet Tracer  
---
Network Topology
```
         [ ISP1 ]————————————[ ISP2 ]
            |    (Dual WAN)      |
         (203.0.113.5)    (203.0.113.1)
            |                   |
            +———[ R1 - Core Router ]———+
                  (192.168.1.1)
                        |
                 [ CSW1 - Core Switch ]
                 (192.168.1.2)
                /       |        \
     [DSW1]——————   [DSW2]———   [DSW3]
   (192.168.10.2)  (192.168.20.2) (192.168.30.2)
      /   |   \       |    \          |
  ASW1  ASW2  ASW3  ASW4   ASW6   [File Server]
   |     |     |     |      |      VLAN 100
  PCs   PCs   PCs   PCs    PCs   (172.16.31.0/29)
  VLAN VLAN  VLAN  VLAN   VLAN
  30&40 30&40  30  10&15   20
   |                 |
[LWAP1]           [LWAP2]
(VLAN 60)         (VLAN 60)
   |
  [WLC]——[CSW1]
(172.16.31.17)

[Printer1]——[DSW1]    [Printer2]——[DSW2]
 VLAN 50               VLAN 50
```
---
```
Devices Used
Device	Hostname	Role
Cisco 2911 Router	ISP1, ISP2	Internet Service Providers
Cisco 2911 Router	R1	Core Router / Gateway
Cisco 3650 (L3)	CSW1	Core Switch
Cisco 3650 (L3)	DSW1, DSW2, DSW3	Distribution Switches
Cisco 2960 (L2)	ASW1–ASW6	Access Switches
WLC-PT	WLC	Wireless LAN Controller
LWAP	LWAP1–LWAP4	Lightweight Access Points
Server-PT	File/DNS/NTP/DHCP Server	Centralized Services
---
IPv4 Addressing & VLAN Plan
VLAN	Department	Subnet	Gateway	Purpose
10	Sales (Data)	10.0.10.0/24	10.0.10.254	CRM, PCs, General Office
15	Sales (Voice)	10.0.15.0/24	10.0.15.254	IP Phones (QoS prioritized)
20	IT & Engineering	10.0.20.0/24	10.0.20.254	Management & High-Privilege
30	Finance/Accounting	10.0.30.0/24	10.0.30.254	Sensitive Financial Data
40	HR & Admin	10.0.40.0/24	10.0.40.254	Payroll & Employee Data
50	Printers	10.0.50.0/29	10.0.50.6	Shared Print Servers
60	Wi-Fi	10.0.60.0/24	10.0.60.254	Wireless Users
99	Management	10.0.99.0/24	10.0.99.254	Network Devices (DSW1 side)
99	Management	10.1.99.0/24	10.1.99.254	Network Devices (DSW2 side)
99	WLC Management	172.16.31.16/28	172.16.31.30	WLC Only
100	File Server	172.16.31.0/29	172.16.31.6	Isolated Central Servers
777	Native/Blackhole	—	—	Trunk Security
---
Technologies & Protocols Implemented
Routing
OSPF — Dynamic routing between R1, CSW1, DSW1, DSW2, DSW3
Static Default Routes — ISP1 / ISP2 dual WAN links
NAT/PAT — Internet access via R1 overload (inside source list)
Switching
VLANs — 10, 15, 20, 30, 40, 50, 60, 99, 100, 777
Inter-VLAN Routing — Via Layer 3 switches (DSW1, DSW2, DSW3)
STP/RSTP (Rapid-PVST) — Loop prevention with PortFast & BPDU Guard on access ports
EtherChannel (LACP) — Port-Channel 1–6 for redundant uplinks
Security
ACLs — SSH restricted to VLAN 20 (IT & Engineering only)
DHCP Snooping — Enabled on all access VLANs
Dynamic ARP Inspection (DAI) — Enabled on all access VLANs
Port Security — Sticky MAC with violation restrict on access ports
Native VLAN 777 — Blackhole VLAN for trunk security
SSH v2 — Encrypted remote management (no Telnet)
Service Password Encryption — All passwords encrypted
Services
DHCP — Centralized on R1, with excluded addresses per VLAN
DNS — Internal DNS server at 172.16.31.1
NTP — NTP server sync with MD5 authentication
Syslog — Centralized logging to 172.16.31.1
WLC + LWAP — Wireless LAN Controller managing lightweight APs (VLAN 60/99)
```
---
Repository Structure
```
Enterprise-Campus-Network-Design-Implementation/
│
├── README.md                        # Project overview (this file)
├── network-topology.pkt             # Cisco Packet Tracer simulation file
│
├── configs/                         # sh run configs (13 devices)
│   ├── isp1-config.txt
│   ├── isp2-config.txt
│   ├── r1-config.txt
│   ├── csw1-config.txt
│   ├── dsw1-config.txt
│   ├── dsw2-config.txt
│   ├── dsw3-config.txt
│   ├── asw1-config.txt
│   ├── asw2-config.txt
│   ├── asw3-config.txt
│   ├── asw4-config.txt
│   └── asw6-config.txt
│
├── docs/
│   └── ipvaddressing&vlanplan_lists.xlsx   # Full IP addressing & VLAN plan
│   └── wlc-gui screenshots/          WLC screenshots
│
├── verification/     ← show commands output
│   ├── show-vlan-brief
    ├── show-int-trunk
│   ├── show-ip-route
│   ├── show-ip-ospf
│   ├── show-etherchannel-summary
│   ├── show-ip-nat(R1)
│   └── ip dhcp binding(R1).png
│
└── diagrams/
    └── Enterprise Campus Network Design & Implementation.png      # Network topology screenshot
```
---
How to Open the Project
Install Cisco Packet Tracer (version 8.x recommended)
Clone this repository:
```bash
   git clone https://github.com/chaardd127/Enterprise-Campus-Network-Design-Implementation.git
   ```
```
Open `Enterprise Campus Network Design & Implementation _UPDATED.pkt` in Cisco Packet Tracer
Refer to `configs/` folder for individual device configurations
Refer to `docs/ipvaddressing&vlanplan_lists.xlsx` for the full IP plan
```
---
 Author
chaardd127  
Enterprise Campus Network Design & Implementation  
Simulated with Cisco Packet Tracer
---
 License
This project is for educational purposes.
---
Disclaimer & Known Packet Tracer Limitations
> This project was built and simulated entirely in **Cisco Packet Tracer** as a virtual home lab for learning and portfolio purposes. Packet Tracer is a network simulation tool and **does not fully replicate real Cisco IOS behavior**. The following limitations and bugs were encountered during this project:

> Known Issues Experienced Issue	Description

>NTP Not Syncing	NTP failed to synchronize across all devices even with correct configuration and MD5 authentication. This is a known Packet Tracer simulation limitation.

> MD5 Auth Removed on Reopen	MD5 authentication configured on access switches was lost after saving and reopening the project, despite using `write memory`. Configs had to be re-applied manually.

> CAPWAP Not Fully Supported	After research, CAPWAP (Control and Provisioning of Wireless Access Points) is not properly supported in Cisco Packet Tracer. The WLC and LWAP association did not function as expected even during active configuration — this is a known tool limitation, not a configuration error. In a real network environment with physical WLC and AP hardware, CAPWAP would operate correctly.
Floating Static Route Reset	Floating static routes reverted to default upon reopening, requiring manual reconfiguration each session.
In addition of that, as I said, Cisco Packet Tracer has some limitations, specifically in Port-Channels, so instead I implement the interface configuration in OSPF, I implement the 'old fashioned-way' global statements per network (<net><wildcard-mask> area <no.>). Plus I applied DHCP Snooping and DAI only in the Layer 2 Switches not in the Port-Channel Switches because it is not applicable and can cause APIPA in the long run.



 Author's Note
> *"This was my first large-scale virtual network project and it was genuinely challenging — not just in terms of design and configuration, but also in dealing with the unpredictable behavior of Packet Tracer on a complex topology. Every bug I encountered pushed me to research deeper, troubleshoot more carefully, and understand the protocols at a fundamental level. In a real production environment with actual Cisco hardware, these limitations would not exist — but working through them gave me hands-on troubleshooting experience that I believe is just as valuable."*
>
> — **chaardd127**
 Workarounds Applied
```
Used `Alt+D` to fast-forward simulation time for DHCP convergence
Manually reconfigured floating static routes per session
Re-entered MD5 authentication keys on access switches after reopening
```
---
>  **Note:** All configurations in the `/configs` folder are in **`show running-config` format** — the standard used by network engineers for configuration documentation, backups, and audits. They represent the **intended final state** of each device. Due to the Packet Tracer limitations noted above, some configurations may need to be re-applied when opening the `.pkt` file.
