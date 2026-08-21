One of the major improvements of BGP4 over BGP3 is **Classless Inter-Domain Routing (CIDR)**. CIDR, or supernetting, is a new way of looking at IP addresses. With CIDR, the concept of classes, such as Class A, B, or C, does not exist. 

For example, the network `192.213.0.0/16`, which was once an illegal Class C network, is now a legal supernet: `192.213.0.0/16`. The "16" represents the number of bits in the subnet mask, counted from the extreme left of the IP address. This representation is similar to `192.213.0.0 255.255.0.0`.

You can use aggregates in order to minimize the size of routing tables. Aggregation is the process that combines the characteristics of different routes in such a way that advertising a single route is possible. In this practice, RTB generates the `160.10.0.0` network; therefore, we are going to configure RTC to propagate a supernet of that route to `160.0.0.0` RTA:
