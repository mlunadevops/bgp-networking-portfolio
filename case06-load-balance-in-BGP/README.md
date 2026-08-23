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

Step 2: Network Advertisement
Each router advertises its respective networks into BGP.

```text
! ROUTER A
router bgp 11
 network 1.1.1.1 mask 255.255.255.255
!
```

```text
! ROUTER B
router bgp 10
 network 2.2.2.0 mask 255.255.255.0
!
```

```text
! ROUTER C
router bgp 10
 network 2.2.2.0 mask 255.255.255.0
```

2.1) Routing Table on Router A (RA sh ip ro)

![LOAD BALANCE](images/01RAroutingtable.png)

**🚀 Step 3: Implementing the maximum-paths Command

### 3.1 & 3.2) Command Configuration and Verification
To allow BGP to install multiple equal-cost paths, add `maximum-paths 2` inside the BGP configuration process on Router A:

```text
!
router bgp 11
 no synchronization
 bgp log-neighbor-changes
 network 1.1.1.1 mask 255.255.255.255
 neighbor 150.10.10.2 remote-as 10
 neighbor 160.20.20.2 remote-as 10
 maximum-paths 2
 no auto-summary
!
```

**

* **Router A:** Advertises its Loopback interface (1.1.1.1/32).
* **Router B and C:** Advertise the shared destination network (2.2.2.0/24).

### Advertisement Configuration
