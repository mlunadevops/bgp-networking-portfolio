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
Router C (RTC - AS 300)

```text
!
router bgp 300
 no synchronization
 bgp log-neighbor-changes
 neighbor 2.2.2.2 remote-as 100
 neighbor 3.3.3.1 remote-as 200
 no auto-summary
!
```
Router B (RTB - AS 200)

```text
!
router bgp 200
 no synchronization
 bgp log-neighbor-changes
 neighbor 3.3.3.2 remote-as 300
 no auto-summary
!
```

Analysis Questions:

Use the show ip bgp summary command on each router to inspect neighbor states.

Has the BGP neighbor relationship been established successfully? Verify physical links, IP connectivity, and AS configurations.

📡 Step 2: Advertising Networks from RTA

**RTA Configuration**

**Router A (RTA - AS 100)**

 ```text
!
router bgp 100
 network 150.10.0.0 mask 255.255.0.0
!
 ```
