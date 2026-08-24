# Lab Guide: BGP Load Balancing in GNS3

## 1. Lab Topology and Prerequisites
* **Software:** GNS3.
* **Devices:** 2 Routers (e.g., Cisco IOS Routers, such as c3725 or c7200 series).
* **Required Interfaces per Router:**
  * At least two serial interfaces (e.g., `Serial0/0` and `Serial0/1`) to simulate multiple equal-cost links between the routers.
  * Loopback interface (`Loopback0`) to simulate the routers' IP addresses for BGP neighbors.

---

## 2. Lab Step-by-Step and Analysis Questions

### Step 1: Initial BGP Configuration (Direct EBGP)
Initially, configure routers RA (AS 11) and RB (AS 10) with basic BGP commands:  

**Router A (RA):**

