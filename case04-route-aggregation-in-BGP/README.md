# Case Study 04: Route Aggregation in BGP

One of the major improvements of BGP4 over BGP3 is **Classless Inter-Domain Routing (CIDR)**. CIDR, or supernetting, is a new way of looking at IP addresses. With CIDR, the concept of classes, such as Class A, B, or C, does not exist. 

For example, the network `192.213.0.0/16`, which was once an illegal Class C network, is now a legal supernet: `192.213.0.0/16`. The "16" represents the number of bits in the subnet mask, counted from the extreme left of the IP address. This representation is similar to `192.213.0.0 255.255.0.0`.

You can use aggregates in order to minimize the size of routing tables. Aggregation is the process that combines the characteristics of different routes in such a way that advertising a single route is possible. In this practice, RTB generates the `160.10.0.0` network; therefore, we are going to configure RTC to propagate a supernet of that route to `160.0.0.0` RTA:

### TOPOLOGY:  Route Aggregation in BGP.

![Route Aggregation in BCP](images/01topology.jpg)

## 2. Lab Practice: Physical Connectivity & Initial BGP Setup

1. Configure all physical connections between the router links.
2. Verify that interfaces between devices have end-to-end IP connectivity.
3. Configure the routers to establish EBGP neighbor relationships using the parameters below:

### Router A (RTA)
```text
!
router bgp 100
 no synchronization
 bgp log-neighbor-changes
 neighbor 2.2.2.1 remote-as 300
 no auto-summary
!
```

