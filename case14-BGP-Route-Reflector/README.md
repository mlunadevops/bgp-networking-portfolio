# TECHNICAL LOG & BGP ENGINEERING GUIDE

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



## 2. Topology and BGP Neighbor Establishment

The topology consists of four routers across different Autonomous Systems (AS 100, AS 200, AS 300, and AS 400). Before advertising prefixes, BGP neighbor adjacencies were established on each device.

![BGP WEIGTH](images/01Topologia.jpg)

 four routers across different Autonomous Systems (AS 100, AS 200, AS 300, and AS 400). Before advertising prefixes, BGP neighbor adjacencies were established on each device.

## 3. BGP Configuration:

**Router A (RTA - AS 100):**

```text
! 
router bgp 100
!
```

---

### 1. Extracción y resumen de comandos por Router

#### **Router R3 (Route Reflector)**

```text
! 
configure terminal
router bgp 65000
bgp router-id 10.0.0.1
neighbor 10.0.0.2 remote-as 65000
neighbor 10.0.0.2 update-source Loopback0
neighbor 10.0.0.2 route-reflector-client
neighbor 10.0.0.3 remote-as 65000
neighbor 10.0.0.3 update-source Loopback0
neighbor 10.0.0.3 route-reflector-client
address-family ipv4
neighbor 10.0.0.2 activate
neighbor 10.0.0.3 activate
network 10.0.0.1 mask 255.255.255.255
exit-address-family
ip route 10.0.0.2 255.255.255.255 192.0.2.2
ip route 10.0.0.3 255.255.255.255 192.0.2.3
end
!
```






* **Finalización y guardado:**
  * `end`
  * `show ip bgp summary`
  * `show ip bgp neighbors 10.0.0.2 advertised-routes`
  * `write memory`

---

#### **Router R1 (RR Client)**
* **Configuración inicial y proceso BGP:**
  * `enable`
  * `configure terminal`
  * `router bgp 65000`
  * `bgp router-id 10.0.0.2`
* **Vecino con RR1 (`10.0.0.1`):**
  * `neighbor 10.0.0.1 remote-as 65000`
  * `neighbor 10.0.0.1 update-source Loopback0`
  * `neighbor 10.0.0.1 password BGPKEY`
* **Activación y red propia:**
  * `address-family ipv4`
  * `neighbor 10.0.0.1 activate`
  * `network 203.0.113.0 mask 255.255.255.0`
  * `exit-address-family`
* **Cierre y guardado:**
  * `end`
  * `write memory`

---

#### **Router R2 (RR Client)**
* **Configuración inicial y proceso BGP:**
  * `enable`
  * `configure terminal`
  * `router bgp 65000`
  * `bgp router-id 10.0.0.3`
* **Vecino con RR1 (`10.0.0.1`):**
  * `neighbor 10.0.0.1 remote-as 65000`
  * `neighbor 10.0.0.1 update-source Loopback0`
  * `neighbor 10.0.0.1 password BGPKEY`
* **Activación y red propia:**
  * `address-family ipv4`
  * `neighbor 10.0.0.1 activate`
  * `network 198.51.100.0 mask 255.255.255.0`
  * `exit-address-family`
* **Cierre y guardado:**
  * `end`
  * `write memory`

---

### 2. Pasos para implementarla en GNS3 desde cero

1. **Diseñar la Topología:**
   * Abre GNS3 e inicia un nuevo proyecto.
   * Coloca tres routers de Cisco (por ejemplo, imágenes IOSv o IOU/Dynamips compatibles con BGP). Nómbralos: **RR1**, **R1** y **R2**.
   * Conecta las interfaces físicas tal como indica el esquema (por ejemplo, interconectando las interfaces de R1 y R2 con el Route Reflector RR1, y añadiendo nubes o switches hacia el exterior para simular las redes de los ISP).

2. **Configurar las Direcciones IP y Loopbacks:**
   * En cada router, configura las interfaces físicas con las subredes correspondientes (/30 para enlaces troncales/ISP y asigna las interfaces Loopback 0 con las IPs `10.0.0.1` para RR1, `10.0.0.2` para R1 y `10.0.0.3` para R2).
   * Asegúrate de habilitar la conectividad básica (haciendo un `ping` entre las Loopbacks antes de levantar BGP).

3. **Aplicar las Configuraciones BGP:**
   * Abre las consolas de cada router (haciendo clic en ellos en GNS3).
   * Copia y pega los bloques de comandos que transcribimos arriba para **RR1**, **R1** y **R2** respectivamente.

4. **Verificar el funcionamiento:**
   * Ejecuta el comando `show ip bgp summary` en el Route Reflector (RR1) para comprobar que las sesiones iBGP con los clientes R1 y R2 estén establecidas (deberán aparecer los estados numéricos de prefijos o valores activos de tiempo, en lugar de estados como *Idle* o *Active*).
   * Comprueba con `show ip bgp` en R1 si estás aprendiendo las rutas anunciadas por R2 a través del Route Reflector sin necesidad de una malla completa (*full mesh*) directa entre R1 y R2.

¿Dudas con alguna interfaz específica o prefieres que ajustemos alguna subred antes de iniciar el laboratorio en GNS3?


### 4. BGP Configuration:


