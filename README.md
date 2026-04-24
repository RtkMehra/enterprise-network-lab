# 🏗️ Enterprise Network Infrastructure Simulation with VLAN Segmentation & OSPF

## 📌 Project Overview

This project simulates a real-world enterprise network for a fictional company **TechNova Solutions Pvt Ltd** using Cisco Packet Tracer.

The goal was to design a **scalable, secure, and segmented network architecture** that supports multiple departments, enforces access control, and enables communication between headquarters and a branch office.

---

## 🎯 Objectives

* Segment network using VLANs to reduce broadcast domains
* Enable communication between VLANs using Inter-VLAN routing
* Implement dynamic routing using OSPF
* Enforce security policies using ACLs
* Automate IP address allocation using DHCP
* Simulate real enterprise network topology

---

## 🏢 Network Architecture

### 📍 Locations

* Headquarters (HQ)
* Branch Office

### 🧑‍💼 Departments (HQ VLANs)

| Department | VLAN ID | Network         |
| ---------- | ------- | --------------- |
| HR         | 10      | 192.168.10.0/24 |
| Finance    | 20      | 192.168.20.0/24 |
| IT         | 30      | 192.168.30.0/24 |
| Guest      | 40      | 192.168.40.0/24 |

---

## 🧱 Devices Used

* 2 Routers (HQ + Branch)
* 2 Switches (Layer 2)
* PCs (for each VLAN)
* 1 Server (DHCP + DNS)

---

## 🖼️ Topology

(Add `topology.png` here)

---

## ⚙️ Step-by-Step Implementation

---

### 🔹 Step 1: Create Basic Topology

* Add Router (HQ)
* Add Switch
* Add 4–8 PCs
* Connect:

  * PC → Switch (Straight-through)
  * Switch → Router

---

### 🔹 Step 2: Configure VLANs (Switch)

Enter CLI on Switch:

```bash
enable
configure terminal

vlan 10
name HR

vlan 20
name Finance

vlan 30
name IT

vlan 40
name Guest
```

Assign ports:

```bash
interface range fa0/1-2
switchport mode access
switchport access vlan 10

interface range fa0/3-4
switchport mode access
switchport access vlan 20

interface range fa0/5-6
switchport mode access
switchport access vlan 30

interface range fa0/7-8
switchport mode access
switchport access vlan 40
```

---

### 🔹 Step 3: Configure Trunk Port

```bash
interface fa0/24
switchport mode trunk
```

---

### 🔹 Step 4: Inter-VLAN Routing (Router-on-a-Stick)

On Router:

```bash
enable
configure terminal

interface g0/0
no shutdown
```

Create subinterfaces:

```bash
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0

interface g0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0

interface g0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
```

---

### 🔹 Step 5: Configure DHCP Server

On Server:

* Go to Services → DHCP

Create pools:

Example (HR):

* Default Gateway: 192.168.10.1
* DNS: 8.8.8.8
* Start IP: 192.168.10.10

Repeat for all VLANs.

---

### 🔹 Step 6: Configure Branch Office

* Add Router (Branch)
* Add Switch + PCs
* Assign network: `192.168.50.0/24`

---

### 🔹 Step 7: Configure OSPF

On HQ Router:

```bash
router ospf 1
network 192.168.10.0 0.0.0.255 area 0
network 192.168.20.0 0.0.0.255 area 0
network 192.168.30.0 0.0.0.255 area 0
network 192.168.40.0 0.0.0.255 area 0
```

On Branch Router:

```bash
router ospf 1
network 192.168.50.0 0.0.0.255 area 0
```

---

### 🔹 Step 8: Configure ACL (Security)

Block Guest → Finance:

```bash
access-list 100 deny ip 192.168.40.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 100 permit ip any any

interface g0/0
ip access-group 100 in
```

---

## 🧪 Testing & Validation

* Ping between VLANs → SUCCESS
* Guest → Finance → BLOCKED
* HQ ↔ Branch communication → SUCCESS
* DHCP IP assignment → VERIFIED

---

## 🔐 Security Features

* VLAN segmentation
* ACL-based traffic filtering
* Restricted guest access

---

## 📊 Key Learnings

* VLAN reduces broadcast traffic
* OSPF enables scalable routing
* ACL enforces network security policies

---

## 🚀 Future Improvements

* Add firewall simulation
* Implement NAT
* Upgrade to Layer 3 switching
* Add redundancy (HSRP)

---

## 🧠 Author

Ritik Mehra

---

## 📎 Files Included

* `network.pkt` → Packet Tracer file
* `topology.png` → Network diagram
* `configs/` → CLI configurations

---

## ⭐ Conclusion

This project demonstrates practical implementation of enterprise networking concepts including segmentation, routing, and security enforcement.
