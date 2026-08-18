Case 01: BGP Neighbor Adjacency and Basic Peering🎯 ObjectiveThis laboratory focuses on establishing BGP neighbor relationships (peers) across diverse Autonomous Systems (AS). The goal is to successfully achieve an "Established" state, which signifies that the TCP connection is active and BGP control packets (OPEN, KEEPALIVE) have been successfully exchanged.  🗺️ Topology Diagram(Ensure your GNS3 topology image is located in the /images folder)📋 Addressing & AS TableDeviceInterfaceIP AddressSubnet MaskAutonomous System (AS)RTAs1/010.0.0.1255.0.0.0AS 100RTBs1/010.0.0.2255.0.0.0AS 200R3e0/09.0.0.2255.0.0.0AS 200RTCe0/18.0.0.1255.0.0.0AS 200RTDs1/011.0.0.1255.0.0.0AS 300⚙️ Configuration SnippetsThe routing configurations enable BGP processes across the infrastructure. Note the neighbor definitions for both eBGP and iBGP sessions.  Example (RTA Configuration):Plaintextrouter bgp 100
 no synchronization
 bgp log-neighbor-changes
 neighbor 10.0.0.2 remote-as 200
 no auto-summary
🔍 Verification and TroubleshootingTo verify the BGP session status, the show ip bgp summary command is essential.  BGP State AnalysisStateTechnical DescriptionCommon Causes for StagnationIdleInitial state; searching for route to neighbor.Missing IP route, interface down.ActiveTCP connection failed or retrying.Incorrect AS, Firewall/ACL blocking port 179.EstablishedNormal operation; routing updates exchanged.None.Operational NotesIf a configuration change is applied to a BGP peer, the session must be reset to force the renegotiation of parameters.Verification Example (RTC after RTD configuration):PlaintextRTC# show ip bgp summary
BGP router identifier 11.0.0.2, local AS number 200
Neighbor    V    AS  MsgRcvd MsgSent TblVer  InQ OutQ Up/Down    State/PfxRcd
8.0.0.2     4    200 85      84      1       0   0    01:21:29   0
11.0.0.1    4    300 8       8       1       0   0    00:04:07   0
The output confirms a stable peering session in the "Established" state with a duration of 00:04:07.
