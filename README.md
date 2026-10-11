# Enterprise Dual-Site WAN & Inter-VLAN Routing Topology

## Overview
This project models a scalable, dual-site enterprise network infrastructure designed in Cisco Packet Tracer. It simulates a corporate Headquarters (HQ) connected to a remote Branch office (Branch) via a dedicated point-to-point Wide Area Network (WAN) link. The architecture emphasizes strict departmental network segmentation using VLANs, sub-interface-based Inter-VLAN Routing (Router-on-a-Stick), dot1Q encapsulation, and Static Routing to achieve full end-to-end communication across geographically separated sites.

## Key Features
* **Dual-Site Architecture:** Dedicated Headquarters (HQ) and Branch sites connected over a `/24` WAN transit network.
* **Departmental Segmentation:** Isolated VLANs per department to enforce broadcast domain containment and network security.
* **Inter-VLAN Routing:** Router-on-a-Stick (RoaS) implementation leveraging sub-interfaces with IEEE 802.1Q encapsulation.
* **Static WAN Routing:** Direct route propagation ensuring bidirectional traffic flow between all endpoints across sites.
* **Trunking & Access Control:** Standardized IEEE 802.1Q trunk links connecting Layer 2 access switches to Layer 3 routers.

---

## Topology Architecture & Addressing Scheme

```text
+-----------------------------------+               +-----------------------------------+
|          HQ-IT-DEPARTMENT         |               |          BRANCH-SALES             |
|          (VLAN 10)                |               |          (VLAN 10)                |
|  - Network: 192.168.1.0/24        |               |  - Network: 10.0.1.0/24           |
|  - Gateway: 192.168.1.5           |               |  - Gateway: 10.0.1.5              |
|                                   |               |                                   |
|                                   |    WAN LINK   |                                   |
|                                   |---------------+                                   |
|          HQ-FINANCE DEPARTMENT    | [172.16.1.0/24] |          BRANCH-LOGISTICS         |
|          (VLAN 20)                |               |          (VLAN 20)                |
|  - Network: 192.168.2.0/24        |               |  - Network: 10.0.2.0/24           |
|  - Gateway: 192.168.2.5           |               |  - Gateway: 10.0.2.5              |
+-----------------------------------+               +-----------------------------------+
Site,Department / Segment,VLAN ID,Subnet,Mask,Default Gateway,Router Interface
Site 1 (HQ),HQ-IT-DEPARTMENT,10,192.168.1.0,255.255.255.0,192.168.1.5,Gig0/0/0.10
Site 1 (HQ),HQ-FINANCE DEPARTMENT,20,192.168.2.0,255.255.255.0,192.168.2.5,Gig0/0/0.20
Site 1 (HQ),WAN Transit Link,-,172.16.1.0,255.255.255.0,N/A,Gig0/0/1 (172.16.1.3)
Site 2 (Branch),BRANCH-SALES,10,10.0.1.0,255.255.255.0,10.0.1.5,Gig0/0/0.10
Site 2 (Branch),BRANCH-LOGISTICS,20,10.0.2.0,255.255.255.0,10.0.2.5,Gig0/0/0.20
Site 2 (Branch),WAN Transit Link,-,172.16.1.0,255.255.255.0,N/A,Gig0/0/1 (172.16.1.2)
Technologies & Concepts Used
Simulation Tool: Cisco Packet Tracer v8.x

Routing Protocols & Concepts: Static Routing, Sub-interfaces, Default Gateways

Switching Concepts: VLAN Creation, Access & Trunk Ports, 802.1Q Trunking/Encapsulation

Hardware Profile: Cisco ISR4331 Routers, Cisco Catalyst 2960 Switches

Configuration Highlights
Router-on-a-Stick Configuration (Branch Router Example)
! Physical Interface Activation
interface GigabitEthernet0/0/0
 no ip address
 no shutdown
 exit

! VLAN 10 Sub-interface Configuration
interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 10.0.1.5 255.255.255.0
 exit

! VLAN 20 Sub-interface Configuration
interface GigabitEthernet0/0/0.20
 encapsulation dot1Q 20
 ip address 10.0.2.5 255.255.255.0
 exit
 Static Route Configuration
 ! Static route pointing traffic towards the remote site network via WAN transit IP
ip route 192.168.1.0 255.255.255.0 172.16.1.2
ip route 192.168.2.0 255.255.255.0 172.16.1.2
How to Use
Clone or download the repository.

Open the .pkt file using Cisco Packet Tracer.

Test end-to-key connectivity across different departments using ping simulations.
