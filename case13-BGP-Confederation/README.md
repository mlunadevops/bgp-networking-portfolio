# TECHNICAL LOG & BGP CONFEDERATION

## Traffic Manipulation via the Weight Attribute
**CCNP Miguelangel Luna**

---

## CONTENIDOS:

| Case | Title | Description |
| :--- | :--- | :--- |
| **01** | [Claude & n8n AI Agents](./case-01-Google-Auth/) | Agentes Claude . |
| **02** | [Packet Tracert & VS CODE](./case-02-PT-MCP-VS-Code/) | Conecta Packet Tracert con MCP |
| **03** | [Generar un Archivo Word desde Markdown usando Phyton](./case-03-Markdown-Word-Using-Phyton/) | Convierte un MD a Word usando Phyton |
| **04** | [GNS3 AI](./case04-GNS3-AI/) | Conexion GNS3 MCP. |

[Google](https://www.google.com)

## 1. Introduction and Theoretical Framework

**BGP Confederation (defined in RFC 5065):** is a scalability technique that allows a large Autonomous System (AS) to be divided into multiple smaller internal sub-ASes. To external ASes, the entire setup still appears as a single Autonomous System, but internally, it avoids the scalability issues of traditional iBGP design.

## 2. Topology and BGP Neighbor Establishment

Topologia
![BGP Confederation Topology](images/00Topology.jpg)

 four routers across different Autonomous Systems (AS 100, AS 200, AS 300, and AS 400). Before advertising prefixes, BGP neighbor adjacencies were established on each device.

## 3. BGP Confederation Configuration:

**R1 (AS 300):**

```text
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface Ethernet0/0
 description Link to R2
 ip address 9.9.1.1 255.255.255.0
!
```

**R2 (Core 400, Sub-AS 10):**

```text
!
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
!
interface Ethernet0/0
 description Link to R1
 ip address 9.9.1.2 255.255.255.0
 !
interface Ethernet0/1
 description Link to R4
 ip address 9.9.3.2 255.255.255.0
 !
```

**R3 (Core 400, Sub-AS 10):**

```text
!
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
!
interface Ethernet0/0
 ip address 9.9.3.3 255.255.255.0
!
interface Ethernet0/1
 ip address 9.9.4.3 255.255.255.0
 !
```


### 4. BGP Configuration:

