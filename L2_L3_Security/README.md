# Enterprise Campus Routing and Switching Laboratory

This laboratory simulates a small enterprise campus network integrating **Layer 3 switching, inter-VLAN routing, WAN connectivity, and access-layer security features**.  
The environment combines a multilayer switch acting as the core/distribution device, access switches, edge routing, and an external WAN connection.

The design focuses on **segmentation, security, and controlled access to external networks**, while supporting wired and wireless clients, servers, and management access.

---

## Network Topology Overview

The topology is composed of **five main network devices**:

- **MLS (Multilayer Switch)**  
  Acts as the core/distribution device, providing:
  - Inter-VLAN routing
  - DHCP services for users and wireless clients
  - Default gateway functionality for all VLANs
  - Uplink to the edge router

- **RTR (Edge Router)**  
  Provides:
  - Connectivity between the internal campus network and the WAN
  - Static routing toward internal VLANs
  - Basic traffic filtering for external access

- **WAN (Internet Router)**  
  Simulates an external Internet network and serves as the next hop for outbound traffic.

- **S1 (Access Switch 1)**  
  Provides access for wired user devices and uplinks to the MLS.

- **S2 (Access Switch 2)**  
  Provides access for servers, wireless infrastructure, and management and maintenance hosts.

---

## VLAN and IP Addressing Design

The network is segmented using multiple VLANs to improve security and manageability:

- **VLAN 10 – Users**
  - Wired user devices
  - DHCP provided by MLS

- **VLAN 20 – Servers**
  - Internal servers connected to S2
  - Static or server-managed IP addressing

- **VLAN 30 – Wireless**
  - Wireless clients connected through an access point
  - DHCP provided by MLS

- **VLAN 99 – Management**
  - Switch and infrastructure management interfaces
  - Restricted access via ACLs and SSH

Inter-VLAN routing is performed on the MLS using switched virtual interfaces (SVIs).

---

## Access Layer Devices (Not Shown in Logical Topology)

In addition to the core devices, the laboratory includes several **end devices that are not represented in the logical topology diagrams**:

### Devices Connected to S1

- **PC 1**
  - Connected to **GigabitEthernet0/1**
  - Member of **VLAN 10 (Users)**

- **PC 2**
  - Connected to **GigabitEthernet1/1**
  - Member of **VLAN 10 (Users)**

These ports implement:
- Port security
- DHCP snooping
- Dynamic ARP inspection (trusted via uplink)

---

### Devices Connected to S2

- **Server 1**
  - Connected to **GigabitEthernet0/1**
  - Member of **VLAN 20 (Servers)**

- **Server 2**
  - Connected to **GigabitEthernet1/1**
  - Member of **VLAN 20 (Servers)**

- **Wireless Access Point**
  - Connected to **GigabitEthernet3/1**
  - Management access via **VLAN 99**
  - Provides wireless connectivity for VLAN 30

- **Wireless PC**
  - Connected wirelessly to the Access Point
  - Member of **VLAN 30 (Wireless)**

- **Maintenance / Management PC**
  - Connected to **FastEthernet0**
  - Used for configuration, testing, and troubleshooting tasks
  - Located in a controlled datacenter environment
  - Typically associated with **VLAN 99**

This interface is intentionally left **without port security** to allow temporary connection of maintenance laptops during operational and troubleshooting activities.

---

## Wireless Security Design

The wireless network is secured using modern encryption and access control mechanisms:

- **Authentication:** WPA2-PSK
- **Encryption:** AES (CCMP)
- **Dedicated Wireless VLAN:** VLAN 30
- **Client Isolation:** Enforced at Layer 3 using ACLs

Wireless clients are treated as a semi-trusted network segment and are subject to stricter access controls than wired users.

---

## Switching and Security Features

The access switches implement multiple Layer 2 security mechanisms:

- **Port Security**
  - Limits MAC addresses on access ports
  - Prevents unauthorized device connections

- **DHCP Snooping**
  - Protects against rogue DHCP servers
  - Trusted only on uplink interfaces

- **Dynamic ARP Inspection**
  - Prevents ARP spoofing attacks
  - Enabled on user and wireless VLANs

- **Spanning Tree (PVST)**
  - Prevents Layer 2 loops
  - PortFast enabled on edge ports

Unused interfaces are placed in a non-routable parking VLAN and administratively shut down.

---

## Routing and WAN Connectivity

- The MLS uses a **default route** pointing to the edge router (RTR).
- The edge router maintains **static routes** back to all internal VLANs.
- The WAN router simulates Internet access and provides a default route back to the edge router.
- Basic ACLs are applied on the edge router to control inbound external traffic.

---

## Inter-VLAN Traffic Control

To reduce the attack surface and enforce the principle of least privilege, access control policies are applied between VLANs:

- **Wireless (VLAN 30) → Management (VLAN 99): DENIED**
- **Management VLAN access:** restricted to authorized hosts
- **User and wireless traffic:** permitted only where operationally required

These controls prevent wireless clients from accessing infrastructure management interfaces.

---

## Management and Access Control

- **SSH v2** is enabled on all managed devices
- Management access is restricted using:
  - ACLs applied to VTY lines
  - Dedicated management VLAN (VLAN 99)
- Password encryption and local user authentication are configured

---

## Purpose of the Laboratory

This lab is designed to demonstrate:

- Enterprise-style campus network design
- Inter-VLAN routing using a multilayer switch
- Secure access-layer configuration
- Secure wireless deployment with WPA2-PSK and AES
- Wired and wireless client integration
- Edge routing and WAN connectivity
- Traffic filtering and management-plane isolation
