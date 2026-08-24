# Lab Guide: BGP Load Balancing in GNS3

## 1. Lab Topology and Prerequisites
* **Software:** GNS3.
* **Devices:** 2 Routers (e.g., Cisco IOS Routers, such as c3725 or c7200 series).
* **Required Interfaces per Router:**
  * At least two serial interfaces (e.g., `Serial0/0` and `Serial0/1`) to simulate multiple equal-cost links between the routers.
  * Loopback interface (`Loopback0`) to simulate the routers' IP addresses for BGP neighbors.

---

## 2. Lab Step-by-Step and Analysis Questions

### Step 1: Initial BGP Configuration (Direct EBGP)
Initially, configure routers RA (AS 11) and RB (AS 10) with basic BGP commands:  

### Router A (RA):

```text
!
router bgp 11
 no synchronization
 neighbor 2.2.2.2 remote-as 10
 no auto-summary
!
```

**Router B (RB):

```text
!
router bgp 11
 no synchronization
 neighbor 2.2.2.2 remote-as 10
 no auto-summary
!
```

**Analysis Question:** Use the `show ip bgp summary` command. What is the reason why the BGP neighbor relationship has not been established?

**Answer / Solution:** By default, EBGP (External BGP) sessions require routers to be directly connected and the TTL (Time to Live) value of BGP packets to be 1. If you try to establish a session using IP addresses that are not directly connected (such as Loopback interfaces) or if there are intermediate hops, the adjacency will not come up.



