#  Small Office Network Design
A complete small office network built in **Cisco Packet Tracer** using VLANs, subnetting, DHCP, and inter-VLAN routing. The network is divided into 3 departments, each on its own VLAN — all connected through a central Layer 3 switch and router.
##  What This Project Does
- Divides one network (`192.168.1.0/24`) into **3 subnets** using subnetting (borrowing 2 bits)
- Creates **3 VLANs** — one per department
- Assigns IP addresses automatically using **DHCP pools**
- Allows all 3 VLANs to **talk to each other** using inter-VLAN routing (Router-on-a-Stick)
- Supports both **wired and wireless** devices in each department
##  Network Devices Used
| Device | Model |
|--------|-------|
| Router | Cisco 2911 |
| Switch | Cisco 2960 (multilayer / VLAN trunk) |
| PCs | PC-PT |
| Printers | Printer-PT |
| Access Points | AccessPoint-PT |
| Laptops | Laptop-PT |
| Smartphones | SMARTPHONE-PT |
##  Network Topology
[Router1 - Cisco 2911]
    |   192.168.1.0/24
[Switch0 - Cisco 2960]
    |          |          |
 VLAN 10    VLAN 20    VLAN 30
 Admin      Finance    Customer Support
The router connects to the switch via **sub-interfaces** (Router-on-a-Stick):
- `g0/0.10` → VLAN 10 (Admin)
- `g0/0.20` → VLAN 20 (Finance)
- `g0/0.30` → VLAN 30 (Customer Support)
##  VLANs & Departments
### VLAN 10 — Admin
- **Devices:** PC0, Printer0, Access Point0, Smartphone0, Laptop0
- **DHCP Pool Name:** `admin`
- Wireless devices (Smartphone0, Laptop0) connect via Access Point0
### VLAN 20 — Finance
- **Devices:** PC1, Printer1, Access Point1, Smartphone1, Laptop1
- **DHCP Pool Name:** `finance`
- Wireless devices connect via Access Point1
### VLAN 30 — Customer Support
- **Devices:** PC2, Printer2, Access Point2, Smartphone2, Laptop2
- **DHCP Pool Name:** `Cs`
- Wireless devices connect via Access Point2
##  Subnetting (How IP Addresses Were Split)
**Base Network:** `192.168.1.0/24`
**Subnet Mask:** `255.255.255.0`
To create 3 separate subnets, **2 bits were borrowed** from the host portion:
Binary of last octet after borrowing 2 bits:
11111111 . 11111111 . 11111111 . 11000000
Bits Borrowed = 128 + 64 = 192
New Subnet Mask = 255.255.255.192 (/26)
Each subnet has 64 IP addresses (62 usable hosts)
### Subnet Table
| VLAN | Department | Network Address | Usable Hosts | Broadcast |
|------|------------|-----------------|--------------|-----------|
| 10 | Admin | 192.168.1.0/26 | 192.168.1.1 – 192.168.1.62 | 192.168.1.63 |
| 20 | Finance | 192.168.1.64/26 | 192.168.1.65 – 192.168.1.126 | 192.168.1.127 |
| 30 | Customer Support | 192.168.1.128/26 | 192.168.1.129 – 192.168.1.190 | 192.168.1.191 |
##  Key Configurations Done
### 1. VLANs on Switch
- Created VLAN 10, 20, and 30
- Assigned ports to correct VLANs (access mode)
- Trunk port configured toward the router
### 2. Router Sub-Interfaces (Inter-VLAN Routing)
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.192
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.1.65 255.255.255.192
interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.1.129 255.255.255.192
### 3. DHCP Pools on Router
- **Pool: admin** → hands out IPs for VLAN 10 range
- **Pool: finance** → hands out IPs for VLAN 20 range
- **Pool: Cs** → hands out IPs for VLAN 30 range
### 4. Wireless Setup
- Each department has its own Access Point
- Laptops and Smartphones connect wirelessly to their department's AP
- Wireless devices get IPs from DHCP just like wired devices
##  What Was Tested
- All PCs in each VLAN receive IP addresses automatically via DHCP
- PCs in different VLANs can ping each other (inter-VLAN routing works)
- Wireless devices (laptops and smartphones) connect to their access points successfully
- Printers are reachable within their VLAN
##  Tools Used
- **Cisco Packet Tracer** — Network simulation
- **Manual Subnetting** — Calculated using binary method (borrowing 2 bits)
## Project File
Open the `.pkt` file in Cisco Packet Tracer to explore the full topology and test connectivity.
*Designed as part of a networking project— covering subnetting, VLANs, DHCP, and inter-VLAN routing in a real-world small office scenario.*
