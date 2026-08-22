# Default Routes Between ASs in BGP

## Introduction

A local Internet Service Provider (ISP-R1, represented by CANTV) provides one of its clients with a network block of `192.168.0.0/16`. In this practice, the client divides the `192.168.0.0/16` network into `/24` masks and only uses the `192.168.1.0/24` and `192.168.2.0/24` networks.

The ISP router (R1) configures a static route for `192.168.0.0/16` pointing toward the client's router (R2). The ISP CANTV connects to an ISP backbone represented by router BB-R3. Router R3 sends a default route to ISP-R1 (CANTV) and receives the `192.168.0.0/16` network via BGP from ISP-R1.

Accessibility is now guaranteed through the Internet (ISP Backbone router R3) to the client router (R2) because R2 has a default route configured to point to ISP-R1 (CANTV). However, if packets are destined for network blocks not in use outside the `192.168.0.0/16` range, the client router R2 uses the default route to ISP-R1, which forwards the packets.
