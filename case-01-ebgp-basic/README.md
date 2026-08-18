# Case 01: BGP Neighbor Adjacency and Basic Peering

## 🎯 Objective
This laboratory focuses on establishing BGP neighbor relationships (peers) across diverse Autonomous Systems (AS). The goal is to successfully achieve an "Established" state, which signifies that the TCP connection is active and BGP control packets (OPEN, KEEPALIVE) have been successfully exchanged.

## 🗺️ Topology Diagram
![Topología High Availability pfSense](images/topology.png)

 **01** | [PROCEDURE](./procedure.md/) | ACTIVITY: Establishing a basic eBGP peering between two AS. |

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
```

🔍 **Verification and Troubleshooting**

To verify the BGP session status, the `show ip bgp summary` command is essential.

### BGP State Analysis

| State | Technical Description | Common Causes for Stagnation |
| :--- | :--- | :--- |
| **Idle** | Initial state; searching for route to neighbor | Missing IP route, interface down |
| **Active** | TCP connection failed or retrying | Incorrect AS, Firewall/ACL blocking port 179 |
| **Established** | Normal operation; routing updates exchanged | None |


To verify the BGP session status, the `show ip bgp summary` command is essential.
```text
show ip bgp summary
```
### Troubleshooting Output Example (RTB)
The following output from **RTB** confirms successful adjacencies with both its iBGP and eBGP peers (showing active connection times and `0` queue states):

```text
RTB#show ip bgp su
BGP router identifier 10.0.0.2, local AS number 200
BGP table version is 1, main routing table version 1

Neighbor    V    AS  MsgRcvd MsgSent TblVer  InQ OutQ Up/Down    State/PfxRcd
9.0.0.2     4   200       57      63      1    0    0 00:53:29        0
10.0.0.1    4   100       64      64      1    0    0 01:01:36        0
```

## ⚙️ Operational Notes
If a configuration change is applied to a BGP peer, the session must be reset to force the renegotiation of parameters using the clear ip bgp command.

Verification Example (RTC after RTD configuration):
```text
R3# clear ip bgp 8.0.0.1
*Mar  1 01:14:05.507: %BGP-5-ADJCHANGE: neighbor 8.0.0.1 Down User reset
*Mar  1 01:14:05.635: %BGP-5-ADJCHANGE: neighbor 8.0.0.1 Up
```
