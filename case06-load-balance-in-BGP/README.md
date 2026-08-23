## Introduction & Key Concepts

By default, BGP selects only a single best path to reach a destination network and does not perform load balancing[cite: 3]. When multiple paths exist between a local AS and a remote AS, BGP chooses just one based on its path selection algorithm.

To achieve load sharing with equal-cost paths, you can use the BGP configuration command:

 ```text
maximum-paths [1-6]
 ```
This command allows you to change the maximum number of parallel equal-cost paths allowed in the routing table (accepting values from 1 to 6).

⚙️ Step 1: EBGP Neighbor Configuration
Configure BGP on Router A (AS 11) to establish eBGP peering relationships with Routers B and C in AS 10.

Router A (RA - AS 11)

 ```text
!
router bgp 11
 no synchronization
 bgp log-neighbor-changes
 neighbor 150.10.10.2 remote-as 10
 neighbor 160.20.20.2 remote-as 10
 no auto-summary
!
 ```
Router B (RB - AS 10)

 ```text
!
router bgp 10
 no synchronization
 bgp log-neighbor-changes
 neighbor 160.20.20.1 remote-as 11
 no auto-summary
!
 ```
Router C (RC - AS 10)

```text
!
router bgp 10
 no synchronization
 bgp log-neighbor-changes
 neighbor 150.10.10.1 remote-as 11
 no auto-summary
!
 ```
Analysis Questions:
1.1) Use the `show ip bgp summary` command to verify adjacencies.
1.2) Has the BGP neighbor relationship been established? Why? (Verify physical connectivity, IP addressing, and neighbor policies).
