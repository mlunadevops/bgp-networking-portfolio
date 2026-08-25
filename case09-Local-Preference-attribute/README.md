# Case Study 09: LOCAL PREFERENCE ATTRIBUTE

The Local Preference attribute is used to influence the preferred exit point from an Autonomous System. Local preference is propagated throughout the entire local Autonomous System. If there are multiple exit points from the AS, this attribute is used to select the exit path for a specific route. A route with a higher Local Preference value is chosen, and the default value is 100. The command to use is as follows:

![BGP Local preference](images/01lotopology.jpg)

## PRACTICE

1. Configure all connections between the links.
2. Verify that the interfaces between the devices have connectivity.
3. Configure all routers to have connectivity among all their BGP neighbors.


**Router C (RTC - AS 256):**
 
 ```text
!
router bgp 256
 no synchronization
 bgp log-neighbor-changes
 neighbor 1.1.1.1 remote-as 100
 neighbor 128.213.11.2 remote-as 256
 no auto-summary
!
 ```
**Router B (RTB - AS 300):**

 ```text
!
router bgp 300
 no synchronization
 bgp log-neighbor-changes
 neighbor 3.3.3.3 remote-as 256
 neighbor 4.4.4.2 remote-as 400
 no auto-summary
!
 ```

**Router D (RTD - AS 256):**

 ```text
!
router bgp 256
 no synchronization
 bgp log-neighbor-changes
 neighbor 3.3.3.4 remote-as 300
 neighbor 128.213.12.1 remote-as 256
 no auto-summary
!
 ```

**Router 6 (R6 - AS 256):**

 ```text
!
router bgp 256
 no synchronization
 bgp log-neighbor-changes
 neighbor 128.213.11.1 remote-as 256
 neighbor 128.213.12.2 remote-as 256
 no auto-summary
!
 ```

**Router 7 (R7 - AS 256):**

 ```text
!
router bgp 400
 no synchronization
 bgp log-neighbor-changes
 neighbor 2.2.2.1 remote-as 100
 neighbor 4.4.4.1 remote-as 300
 no auto-summary
!
 ```
