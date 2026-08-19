# BGP Networking Portfolio

This repository serves as a professional portfolio documenting 15 real-world BGP (Border Gateway Protocol) configuration scenarios implemented in GNS3. Each case study provides a complete breakdown of the topology, configuration files, and verification steps.

## Portfolio Overview

| Case | Title | Description |
| :--- | :--- | :--- |
| **01** | [eBGP Basic Configuration](./case-01-ebgp-basic/) | Establishing a basic eBGP peering between two AS. |
| **02** | [BGP to EIGRP Redistribution](./case-02-ibgp-rr/) | BGP Routes into EIGRP. |
| ... | ... | ... |

BGP is the industry-standard protocol used by Internet Service Providers (ISPs) and large enterprises precisely for that: to interconnect their edge routers with one another and move traffic on a global scale across the Internet.

To put it into the broader picture:

* **The Edge:** This is the boundary where an enterprise's internal network (running OSPF or EIGRP) ends, and the network of the operator or another organization begins. This is where edge routers running BGP are deployed.
* **The "Glue" of the Internet:** Since every ISP and large enterprise manages its own Autonomous System (AS) with completely independent and different internal addressing schemes, BGP acts as the universal common language that allows them to negotiate which IP address blocks are reachable through which paths.

---
*Created by [Tu Nombre]* | *Networking Engineer*
