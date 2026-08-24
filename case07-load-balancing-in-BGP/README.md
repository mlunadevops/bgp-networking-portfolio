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
router bgp 10
 no synchronization
 bgp log-neighbor-changes
 neighbor 1.1.1.1 remote-as 11
!
```

**Analysis Question:** Use the `show ip bgp summary` command. What is the reason why the BGP neighbor relationship has not been established?

**Answer / Solution:** By default, EBGP (External BGP) sessions require routers to be directly connected and the TTL (Time to Live) value of BGP packets to be 1. If you try to establish a session using IP addresses that are not directly connected (such as Loopback interfaces) or if there are intermediate hops, the adjacency will not come up.

Step 2: Using the ebgp-multihop CommandContinue configuring both routers to allow multiple hops in EBGP:  


### Router A (RA):

```text
!
router bgp 11
neighbor 2.2.2.2 ebgp-multihop 255
ip http server
!
```

**Router B (RB):

```text
!
router bgp 10
neighbor 1.1.1.1 ebgp-multihop 255
!
```

Analysis Questions:

2.1) What is the ebgp-multihop command used for?

Answer: It allows an EBGP session to be established between routers that are not directly connected (meaning they are more than one network hop away), by modifying the default TTL value.

2.2) What does the value of 255 at the end of the ebgp-multihop command mean?

Answer: It defines the maximum TTL value in the IP header of the BGP session packets, allowing the packet to traverse up to 255 intermediate routers without being dropped.

**Status Question:** Use the `show ip bgp summary` command. What is the reason why the BGP neighbor relationship has not been established?

**Answer / Solution:** Even though we already allowed multiple hops with a TTL of 255, the routers still do not know where to route traffic to reach the neighbor's Loopback IP address (`2.2.2.2` or `1.1.1.1`), because the output interface or static route to that IP has not been specified, nor has the physical IP address that the router should use as its source been indicated (`update-source`).

### Step 3: Configuring the Source Address and Static Routes
Configure the Loopback interface as the source for BGP updates and add static routes to reach the multiple physical links (`Serial0/0` and `Serial0/1`).

Router A:

```text
!
router bgp 11
 neighbor 2.2.2.2 update-source Loopback0
!
ip route 2.2.2.2 255.255.255.255 Serial0/1
ip route 2.2.2.2 255.255.255.255 Serial0/0
!
```


Router B:

```text
!
router bgp 10
 neighbor 1.1.1.1 update-source Loopback0
!
ip route 1.1.1.1 255.255.255.255 Serial0/0
ip route 1.1.1.1 255.255.255.255 Serial0/1
!
```

**Analysis Question:** 3.4) What is the reason why the BGP neighbor relationship has not been established? Perform troubleshooting and communicate the routers.  
*Answer / Solution:* When configuring static routes pointing directly to multi-access or serial interfaces without a clear next-hop IP on point-to-point or broadcast networks, BGP may have issues resolving recursion if proper Layer 2/Layer 3 connectivity is missing. Additionally, IP addresses on the serial interfaces must be secured (e.g., configuring PPP/HDLC encapsulation or valid `/30` subnets). Once static routes are corrected with the proper next-hop IPs or interfaces, communication is established.

**3.5) Device Verification:**  
Run `show ip bgp summary` on both routers to confirm that the adjacency state shows a number (indicating exchanged prefixes) instead of states like `Idle` or `Active`.

### Steps 4 and 5: Verifying Load Balancing
* **Check the Routing Table:** Run `show ip route bgp` on Router RA to verify that multiple equal-cost routes appear toward the destination network.
* **Perform Traceroute:** Run a `traceroute` toward the neighbor's Loopback address to check which interfaces traffic alternates through, proving load balancing is working.

