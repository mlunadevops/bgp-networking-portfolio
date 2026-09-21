# TECHNICAL LOG & Lab Guide: eBGP Multihop over Parallel Links with Load Balancing (ECMP)

## eBGP Multihop
**CCNP Miguelangel Luna**

---

## CONTENIDOS:

| Case | Title | Description |
| :--- | :--- | :--- |
| **01** | [Claude & n8n AI Agents](./case-01-Google-Auth/) | Agentes Claude . |
| **02** | [Packet Tracert & VS CODE](./case-02-PT-MCP-VS-Code/) | Conecta Packet Tracert con MCP |
| **03** | [Generar un Archivo Word desde Markdown usando Phyton](./case-03-Markdown-Word-Using-Phyton/) | Convierte un MD a Word usando Phyton |
| **04** | [GNS3 AI](./case04-GNS3-AI/) | Conexion GNS3 MCP. |

[Google](https://www.google.com)

## 1. Introduction and Theoretical Framework



## 2. Topology and BGP Neighbor Establishment

The topology consists of four routers across different Autonomous Systems (AS 100, AS 200, AS 300, and AS 400). Before advertising prefixes, BGP neighbor adjacencies were established on each device.

![eBGP Multihop](images/01Topologia.jpg)

 four routers across different Autonomous Systems (AS 100, AS 200, AS 300, and AS 400). Before advertising prefixes, BGP neighbor adjacencies were established on each device.

## 3. BGP Configuration:

**Router A (RTA - AS 100):**

```text
! 
router bgp 100
!
```

### 4. BGP Configuration:

-------------------------------------------------------------------------------------------

# Step-by-Step Lab Guide: eBGP Multihop over Parallel Links with Load Balancing (ECMP)

## Overview
This laboratory guide outlines the configuration and verification procedures for establishing an eBGP Multihop session between two independent Autonomous Systems (AS 100 and AS 200). 

By utilizing loopback interfaces and static routes across two parallel physical links, this lab demonstrates how to achieve Equal-Cost Multi-Path (ECMP) load balancing for control and data traffic in BGP.

---

## Topology Specifications
* **Autonomous System 100 (Router RTA):**
  * Loopback 0 IP: `203.0.113.1/32`
* **Autonomous System 200 (Router RTB):**
  * Loopback 0 IP: `198.51.100.1/32`
* **Parallel Physical Link 1:**
  * Subnet: `198.18.1.0/30`
  * RTA: `198.18.1.1` | RTB: `198.18.1.2`
* **Parallel Physical Link 2:**
  * Subnet: `198.18.2.0/30`
  * RTA: `198.18.2.1` | RTB: `198.18.2.2`

---

## Step 1: Physical and Logical Interface Configuration

Configure the physical interface IP addresses and the Loopback interfaces on both routers to ensure basic interface up/up status.

### RTA Configuration (AS 100)
```text
RTA# configure terminal
!
interface Loopback 0
 ip address 203.0.113.1 255.255.255.255
 exit
!
interface GigabitEthernet0/0
 ip address 198.18.1.1 255.255.255.252
 no shutdown
 exit
!
interface GigabitEthernet0/1
 ip address 198.18.2.1 255.255.255.252
 no shutdown
 exit
```

### RTB Configuration (AS 200)
```text
RTB# configure terminal
!
interface Loopback 0
 ip address 198.51.100.1 255.255.255.255
 exit
!
interface GigabitEthernet0/0
 ip address 198.18.1.2 255.255.255.252
 no shutdown
 exit
!
interface GigabitEthernet0/1
 ip address 198.18.2.2 255.255.255.252
 no shutdown
 exit
```

---

## Step 2: Configuring Static Routes for ECMP

Because eBGP multihop peers are not directly connected via their loopback addresses, specific static host routes pointing across both physical links are required to enable ECMP load balancing.

### RTA Static Routes
```text
RTA# configure terminal
ip route 198.51.100.1 255.255.255.255 198.18.1.2
ip route 198.51.100.1 255.255.255.255 198.18.2.2
```

### RTB Static Routes
```text
RTB# configure terminal
ip route 203.0.113.1 255.255.255.255 198.18.1.1
ip route 203.0.113.1 255.255.255.255 198.18.2.1
```

---

## Step 3: BGP Routing Protocol Configuration

Configure BGP processes, specifying the remote AS, forcing BGP to use loopback source addresses, and allowing multihop sessions.

### RTA BGP Configuration (AS 100)
```text
RTA# configure terminal
router bgp 100
 neighbor 198.51.100.1 remote-as 200
 neighbor 198.51.100.1 ebgp-multihop 2
 neighbor 198.51.100.1 update-source Loopback 0
 network 203.0.113.1 mask 255.255.255.255
 exit
```

### RTB BGP Configuration (AS 200)
```text
RTB# configure terminal
router bgp 200
 neighbor 203.0.113.1 remote-as 100
 neighbor 203.0.113.1 ebgp-multihop 2
 neighbor 203.0.113.1 update-source Loopback 0
 network 198.51.100.1 mask 255.255.255.255
 exit
```

---

## Step 4: Verification and Troubleshooting Commands

Use the following commands in your GNS3 lab to verify proper operation, route distribution, and load balancing. Paste your CLI output evidence below each command block.

### 1. Verify IP Routing Table and ECMP Paths
* **Command:** `show ip route` or `show ip route [loopback-ip]`
* **Objective:** Confirm that two equal-cost paths exist toward the neighbor's loopback address.
* *[Insert your GNS3 output evidence here]*

### 2. Verify eBGP Peer Summary Status
* **Command:** `show ip bgp summary`
* **Objective:** Check that the BGP neighbor session is established (state shows a numeric prefix count instead of Active/Idle).
* *[Insert your GNS3 output evidence here]*

### 3. Inspect BGP Table Entries
* **Command:** `show ip bgp`
* **Objective:** Validate that local networks and advertised prefixes from the remote AS are correctly received and installed.
* *[Insert your GNS3 output evidence here]*

### 4. Verify CEF and Load Balancing Behavior
* **Command:** `show ip cef [loopback-ip]`
* **Objective:** Confirm that traffic is being split across both parallel physical interfaces (`GigabitEthernet0/0` and `GigabitEthernet0/1`).
* *[Insert your GNS3 output evidence here]*
