# Case Study 02: BGP to EIGRP Redistribution

## Introduction
This document details the configuration process for redistributing BGP routes into an Interior Gateway Protocol (IGP), specifically EIGRP. The ability to exchange routing information between BGP and IGP protocols is crucial for managing traffic flow in complex, multi-protocol network environments.

## Objective
The primary goal is to successfully redistribute a BGP-learned route (`129.213.1.0/24`) into an EIGRP domain, ensuring reachability across different autonomous systems and protocols.

## Technical Context
The BGP `network` command is used to advertise specific routes from an IGP into BGP. Conversely, redistribution allows an IGP to learn routes that originated in BGP, facilitating end-to-end connectivity.

## Implementation Steps

### 1. BGP Configuration
The routers are configured to establish BGP neighbor relationships across different autonomous systems.

**RTA Configuration:**

```text
! 
router bgp 100
 no synchronization
 bgp log-neighbor-changes
 network 129.213.1.0 mask 255.255.255.0
 neighbor 2.2.2.1 remote-as 300
 no auto-summary
!
```
```text
! 
router bgp 300
 no synchronization
 bgp log-neighbor-changes
 neighbor 1.1.1.2 remote-as 200
 neighbor 2.2.2.2 remote-as 100
 no auto-summary
!
```
```text
! 
router bgp 200
 no synchronization
 bgp log-neighbor-changes
 neighbor 1.1.1.1 remote-as 300
 no auto-summary
!
```
