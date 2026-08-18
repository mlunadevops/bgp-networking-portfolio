# Case 01: BGP Neighbor Adjacency and Basic Peering

## 🎯 Objective
This laboratory focuses on establishing BGP neighbor relationships (peers) across diverse Autonomous Systems (AS). The goal is to successfully achieve an "Established" state, which signifies that the TCP connection is active and BGP control packets (OPEN, KEEPALIVE) have been successfully exchanged.

## 🗺️ Topology Diagram
![Topología High Availability pfSense](images/topology.png)

 **01** | [procedure](./procedure.md/) | ACTIVITY: Establishing a basic eBGP peering between two AS. |

## 📋 Addressing & AS Table

| Device | Interface | IP Address | Subnet Mask | Autonomous System (AS) |
| :--- | :--- | :--- | :--- | :--- |
| **RTA** | s1/0 | 10.0.0.1 | 255.0.0.0 | AS 100 |
| **RTB** | s1/0 | 10.0.0.2 | 255.0.0.0 | AS 200 |
| **R3** | e0/0 | 9.0.0.2 | 255.0.0.0 | AS 200 |
| **RTC** | e0/1 | 8.0.0.1 | 255.0.0.0 | AS 200 |
| **RTD** | s1/0 | 11.0.0.1 | 255.0.0.0 | AS 300 |

🔍 **Verification and Troubleshooting**

To verify the BGP session status, the `show ip bgp summary` command is essential.

### BGP State Analysis

| State | Technical Description | Common Causes for Stagnation |
| :--- | :--- | :--- |
| **Idle** | Initial state; searching for route to neighbor | Missing IP route, interface down |
| **Active** | TCP connection failed or retrying | Incorrect AS, Firewall/ACL blocking port 179 |
| **Established** | Normal operation; routing updates exchanged | None |

## ⚙️ Configuration Snippets
The routing configurations enable BGP processes across the infrastructure. Note the neighbor definitions for both eBGP and iBGP sessions.

**Example (RTA Configuration):**

```text
router bgp 100
 no synchronization
 bgp log-neighbor-changes
 neighbor 10.0.0.2 remote-as 200
 no auto-summary
```
## ⚙️ Operational Notes
If a configuration change is applied to a BGP peer, the session must be reset to force the renegotiation of parameters.

Verification Example (RTC after RTD configuration):
```text
RTC# show ip bgp summary
BGP router identifier 11.0.0.2, local AS number 200
Neighbor    V    AS  MsgRcvd MsgSent TblVer  InQ OutQ Up/Down    State/PfxRcd
8.0.0.2     4    200 85      84      1       0   0    01:21:29   0
11.0.0.1    4    300 8       8       1       0   0    00:04:07   0
```
🔍 **Verification and Troubleshooting**

To verify the BGP session status, the `show ip bgp summary` command is essential.

### BGP State Analysis

| State | Technical Description | Common Causes for Stagnation |
| :--- | :--- | :--- |
| **Idle** | Initial state; searching for route to neighbor | Missing IP route, interface down |
| **Active** | TCP connection failed or retrying | Incorrect AS, Firewall/ACL blocking port 179 |
| **Established** | Normal operation; routing updates exchanged | None |
