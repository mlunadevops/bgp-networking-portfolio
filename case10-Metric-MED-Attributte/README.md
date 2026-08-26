# Case Study 10: MED (Multi-Exit Discriminator)

## The Metric Attribute (MED)

The metric attribute is also known as MULTI_EXIT_DISCRIMINATOR, MED (BGP4), or INTER_AS (BGP3). This attribute acts as a suggestion to external neighbors regarding route preference within an Autonomous System (AS). It provides a dynamic way to influence another AS, such as determining how to reach a specific route when there are multiple entry points into that AS. A lower metric value is preferred.

Unlike local preference, the metric (MED) is exchanged between ASes. A metric is carried into an AS, but it does not leave it. When an update enters an AS with a specific metric, that metric is used to make routing decisions within the AS. When the update from that same AS is passed to a third AS, the metric is reset to 0. The diagram in this section illustrates the set of metrics. The default metric value is 0.

Unless a router receives other instructions, it compares metrics for routes from neighbors in the same AS. For the router to compare metrics from neighbors originating in different ASes, you must execute the special configuration command `bgp always-compare-med` on the router.

**Router A (RTA - AS 100):**
 
 ```text
!
!
interface Serial1/0
 ip address 4.4.4.4 255.255.255.0
 serial restart-delay 0
!
interface Serial1/1
 ip address 3.3.3.2 255.255.255.0
 serial restart-delay 0
!
interface Serial1/2
 ip address 2.2.2.2 255.255.255.0
 serial restart-delay 0
!
 ```

**Router C (RTC - AS 300):**

 ```text
!
interface Ethernet0/0
 ip address 1.1.1.2 255.255.255.0
 half-duplex
!
interface Serial1/2
 ip address 2.2.2.1 255.255.255.0
 serial restart-delay 0
!
interface Serial1/2
 ip address 2.2.2.2 255.255.255.0
 serial restart-delay 0
!
 ```

**Router B (RTC - AS 400):**

 ```text
!
interface Serial1/0
 ip address 4.4.4.3 255.255.255.0
 serial restart-delay 0
!
 ```

