# Default Routes Between ASs in BGP

## Introduction

A local Internet Service Provider (ISP-R1, represented by CANTV) provides one of its clients with a network block of `192.168.0.0/16`. In this practice, the client divides the `192.168.0.0/16` network into `/24` masks and only uses the `192.168.1.0/24` and `192.168.2.0/24` networks.

The ISP router (R1) configures a static route for `192.168.0.0/16` pointing toward the client's router (R2). The ISP CANTV connects to an ISP backbone represented by router BB-R3. Router R3 sends a default route to ISP-R1 (CANTV) and receives the `192.168.0.0/16` network via BGP from ISP-R1.

Accessibility is now guaranteed through the Internet (ISP Backbone router R3) to the client router (R2) because R2 has a default route configured to point to ISP-R1 (CANTV). However, if packets are destined for network blocks not in use outside the `192.168.0.0/16` range, the client router R2 uses the default route to ISP-R1, which forwards the packets.

### TOPOLOGY:  Route Aggregation in BGP.

![Static Route in BCP](images/01Topology.jpg)

```text
neighbor {ip-address | peer-group-name} default-originate [route-map map-name]
no neighbor {ip-address | peer-group-name} default-originate [route-map map-name]
```
# Practical Exercise

## Step 1: Configure Connections
* Configure all physical and logical connections between the links as illustrated in the network topology.
  
### Router 3 (R3)
```text
!
interface Loopback0
 ip address 10.3.3.3 255.255.255.255
!
interface Serial1/0
 ip unnumbered Loopback0
 serial restart-delay 0
!
ip route 10.1.1.1 255.255.255.255 Serial1/0
!
```
### Router 1 (R1)
```text
!
!
interface Loopback0
 ip address 10.1.1.1 255.255.255.255
!
interface Serial1/0
 ip unnumbered Loopback0
 serial restart-delay 0
!
interface Serial1/1
 ip address 10.0.0.1 255.255.255.252
 serial restart-delay 0
!
ip route 10.3.3.3 255.255.255.255 Serial1/0
ip route 192.168.0.0 255.255.0.0 Serial1/1
!
```
### Router 2 (R2)
```text
!
interface Loopback0
 ip address 10.2.2.2 255.255.255.0
!
interface Ethernet0/0
 ip address 192.168.1.1 255.255.255.0
 half-duplex
!
interface Ethernet0/1
 ip address 192.168.2.1 255.255.255.0
 half-duplex
!
ip route 0.0.0.0 0.0.0.0 10.0.0.1
!
```

## Step 2: Verify Connectivity
* Verify that the interfaces between all devices have proper physical and logical connectivity.

## Step 3: Configure BGP and Routing
* Configure Router 1 (R1) to establish an EBGP relationship with Router 3 (R3) and Router 2 (R2), while advertising the necessary networks.
