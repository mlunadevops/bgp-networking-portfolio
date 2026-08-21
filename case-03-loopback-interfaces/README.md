# Case Study 03: BGP Peering over Loopback Interfaces

## Introduction
This document details the configuration process for establishing BGP neighbor relationships using loopback interfaces instead of physical interfaces. By default, BGP uses physical interface IP addresses to form TCP connections. Leveraging loopback interfaces ensures session stability and independence from physical hardware link status, which is an essential design pattern in large-scale network architecture.

### TOPOLOGY: BGP Loopback Peering

![BGP Loopback Topologia](images/01topologia.png)

### Main Objective: BGP Loopback Peering Summary
The primary goal is to successfully configure BGP peering using loopback interfaces across an Autonomous System (AS 200). This lab demonstrates how to overcome default BGP source-IP behaviors by implementing the `update-source` command to stabilize neighbor establishment.

#### Case Study Details
To achieve this objective, the lab guides the student through the following technical steps:

* **Loopback Addressing**: Utilizing virtual loopback interfaces to anchor the BGP router ID and peer IP addresses.
* **OSPF Integration**: Enabling an Interior Gateway Protocol (IGP), specifically OSPF, to provide reachability to the loopback IP addresses across the topology.
* **Source Modification**: Forcing BGP to use the loopback address as the source for TCP and BGP connection packets using `update-source`.

In summary, this lab demonstrates how to build a fault-tolerant BGP peering architecture independent of physical link fluctuations.
