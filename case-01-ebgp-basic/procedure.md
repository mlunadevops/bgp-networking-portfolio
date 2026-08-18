## BGP Neighbors

### Introduction
Two BGP routers become neighbors once a TCP connection has been established between them. This TCP connection is essential for the routers to initiate the exchange of routing updates. 

Once the TCP connection is established, the routers send `OPEN` messages to exchange information. The values exchanged between routers include the Autonomous System (AS) number, the BGP version currently running, the BGP Router ID, and Keepalive timers. 

After these values are confirmed and accepted, the neighbor relationship is established. Any state other than "Established" indicates that the routers have failed to become neighbors, preventing the exchange of BGP updates.

### Objective
In this laboratory, we will learn how to configure BGP neighbor relationships using the following command:

```text
neighbor <ip-address> remote-as <as-number>
