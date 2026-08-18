# Case 01: BGP Neighbor Adjacency and Basic Peering

## 🎯 Objective
This laboratory focuses on establishing BGP neighbor relationships (peers) across diverse Autonomous Systems (AS). The goal is to successfully achieve an "Established" state, which signifies that the TCP connection is active and BGP control packets (OPEN, KEEPALIVE) have been successfully exchanged.

## 🗺️ Topology Diagram
![Topología High Availability pfSense](images/topology.png)

## 📋 Addressing & AS Table

| Device | Interface | IP Address | Subnet Mask | Autonomous System (AS) |
| :--- | :--- | :--- | :--- | :--- |
| **RTA** | s1/0 | 10.0.0.1 | 255.0.0.0 | AS 100 |
| **RTB** | s1/0 | 10.0.0.2 | 255.0.0.0 | AS 200 |
| **R3** | e0/0 | 9.0.0.2 | 255.0.0.0 | AS 200 |
| **RTC** | e0/1 | 8.0.0.1 | 255.0.0.0 | AS 200 |
| **RTD** | s1/0 | 11.0.0.1 | 255.0.0.0 | AS 300 |



## ⚙️ Configuration Snippets
The routing configurations enable BGP processes across the infrastructure. Note the neighbor definitions for both eBGP and iBGP sessions.

**Example (RTA Configuration):**

```text
router bgp 100
 no synchronization
 bgp log-neighbor-changes
 neighbor 10.0.0.2 remote-as 200
 no auto-summary
