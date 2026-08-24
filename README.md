# BGP Networking Portfolio

This repository serves as a professional portfolio documenting 15 real-world BGP (Border Gateway Protocol) configuration scenarios implemented in GNS3. Each case study provides a complete breakdown of the topology, configuration files, and verification steps.

BGP is the industry-standard protocol used by Internet Service Providers (ISPs) and large enterprises precisely for that: to interconnect their edge routers with one another and move traffic on a global scale across the Internet.

```text

+---------------------------------------------------+
       |                    INTERNET                       |
       |            (Global BGP Routing Table)             |
       +-------------------------+-------------------------+
                                 |
                                 | eBGP (External BGP)
                                 | AS 100 <---> AS 200
                                 v
       +---------------------------------------------------+
       |                 EDGE ROUTER                       |
       |       [ Router B (RTB) / Edge Router ]            |
       |  - Runs BGP to talk to ISPs                       |
       |  - Runs IGP (EIGRP/OSPF) inside the enterprise    |
       +-------------------------+-------------------------+
                                 |
                                 | Redistribution / iBGP
                                 v
       +---------------------------------------------------+
       |              ENTERPRISE INTERNAL NETWORK          |
       |              (EIGRP / OSPF Domain)                |
       |         [ R3 ] <-----> [ RTC ]                    |
       +---------------------------------------------------+
```

To put it into the broader picture:

* **The Edge:** This is the boundary where an enterprise's internal network (running OSPF or EIGRP) ends, and the network of the operator or another organization begins. This is where edge routers running BGP are deployed.
* **The "Glue" of the Internet:** Since every ISP and large enterprise manages its own Autonomous System (AS) with completely independent and different internal addressing schemes, BGP acts as the universal common language that allows them to negotiate which IP address blocks are reachable through which paths.

---



## Portfolio Overview

| Case | Title | Description |
| :--- | :--- | :--- |
| **01** | [eBGP Basic Configuration](./case-01-ebgp-basic/) | Establishing a basic eBGP peering between two AS. |
| **02** | [BGP to EIGRP Redistribution](./BGP-to-EIGRP-Redistribution/) | BGP Routes into EIGRP. |
| **03** | [BGP Loopback Interface & OSPF](./case-03-loopback-interfaces/) | Why Loopbacks break your BGP? (And how to fix it) . |
| **04** | [Route aggregation in BGP](./case04-route-aggregation-in-BGP/) | Route aggregation in BGP. |
| **05** | [Default Routes](./case05-default-routes-in-BGP/) | Defaul Routes in BGP |
| **06** | [BGP Load Balance](./case06-load-balance-in-BGP/) | Load Balance in BGP |
| **07** | [BGP Load Balance](./case07-load-balancing-in-BGP/) | Load Balance in BGP |
| **08** | [BGP Community](./case08-BGP-Community-Based-Route-Filtering-(no-export)/) | configuring BGP community attributes and route maps. |
| ... | ... | ... |


*Created by [Miguelangel Luna]* | *Networking Engineer*
