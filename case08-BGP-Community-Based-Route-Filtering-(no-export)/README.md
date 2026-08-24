## Implementing BGP Communities for Route Control and Filtering

**Objective of this Practice:** Configure RTB (AS 200) to set the community attribute on BGP routes it advertises so that RTC (AS 300) does not propagate those routes to external peers like RTA (AS 100), using the `no-export` well-known community filter.

⚙️ Step 1: Initial EBGP Neighbor Configuration
Configure the BGP routing processes on RTA, RTB, and RTC to establish peering sessions.

**Router A (RTA - AS 100)**


 ```text
!
router bgp 100
 no synchronization
 bgp log-neighbor-changes
 neighbor 2.2.2.1 remote-as 300
 no auto-summary
!
 ```
