# MPLS Traffic Engineering (MPLS TE) - Class Notes

**MPLS Traffic Engineering \(MPLS TE\)** \(10 Sept 2014\)Lab: MPLS TE 1 \- 2
\- This mechanism allows for a predefined route from source to destination

\- It can use a path that meets the interface property requirements

     \-\> Bandwidth

     \-\> Delay

Starting point

\- MPLS tunnel head end

End point

\- MPLS tunnel tail end

**TE Components**

1. Interface constraints

     \-\> Bandwidth

     \-\> Delay

     \-\> Jitter

2. Link state routing protocol

     \-\> To advertise the link constaints

3. Algorithm

     \-\> To calculate the best routes according to tunnel requirements of interface constraints

     \-\> PCALC

          \-\> Path Calculation

     \-\> OSPF constraint\-based SPF

4. Tunnel signaling

     \-\> The process of requesting and receiving MPLS labels

     \-\> Resource Reservation Protocol

5. Fowarding traffic over the tunnel

Interface Constraints

\- Max bandwidth

     \# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">bandwidth \<kbps\></span></span>

\- Max reservable bandwidth

     \# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip rsvp bandwidth \<kbps\></span></span>

\- Available bandwidth

     \-\> Variable which the router calculates

\- Administrative groups

     \-\> Optional

     \-\> 32 bit value used to define other attributes

          \-\> Delay

          \-\> Jitter

R1\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng tunnels</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  bandwidth \<kbps\></span></span>

     \-\> all interfaces have to set

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  mpls traffic\-eng tunnels</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  ip rsvp bandwidth \<kbps\></span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  mpls traffic\-eng administrative\-group \<value in hex\></span></span>

**Routing Protocols**

\- OSPF and IS\-IS can be used

\- OSPF

     \-\> Opaque LSAs 9, 10, and 11 are used

     \-\> LSA 9

          \-\> Link\-local

     \-\> LSA 10

          \-\> Area\-local

          \-\> Intra\-area MPLS\-TE

     \-\> LSA 11

          \-\> Domain\-local

          \-\> Inter\-area MPLS\-TE

\- IS\-IS

     \-\> Introduces two new TVLs

          \-\> Extended IS Reachability

          \-\> Extended IP Reachability

**OSPF**

\- On all routers

\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">router ospf 1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng router\-id lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-end area 0</span></span>

**IS\-IS**

\- On all routers

\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">router isis ABC</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng router lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng level\-2</span></span>

RSVP

\- RSVP Path Message

\- RSVP Reserve Message

Explicit Router Object \(ERO\)

\- Headend router sends path message with the ERO in the message

     \-\> ERO contains a collection of next\-hops to be used

          \-\> 16.0.0.1

          \-\> 13.0.0.3

          \-\> 34.0.0.4

          \-\> 45.0.0.5

When any router receives the path message, it checks the ERO and removes its own IP address and sends the message to the next\-hop

The path message also contains the bandwidth requirement of the tunnel

When the path message reaches the tailend router, the tailend router generates a RESV message that contains the label to be used

\- Each router that receives the RESV message changes the message with its generated label to be used

     \-\> RSVP label, not an MPLS label

**Headend Tunnel Configuration**

\- Tunnel IP address

     \-\> Unnumbered

     \-\> Linked to a loopback interface

\- Destination IP address

     \-\> Router\-id of the tailend router

\- Bandwidth required

\- Path options

     \-\> Manual

          \-\> Explicit path

     \-\> Dynamic

          \-\> PCALC

          \-\> SPF

\- Setup and holding priorities

     \-\> Tunnel parameters

\- Affinity

\- Tunnel metric

     \-\> IGP

     \-\> TE

R6 \(headend\) \-\> R1 \-\> R3 \-\> R4 \-\> R5 \(tailend\)

R6\(config\)\# i<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">nt tu0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">unnumbered lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mode mpls traffic\-eng</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel destination 5.5.5.5</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng bandwidth \<kbps\></span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng path\-option explicit name \<name\></span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip explicit\-path name \<name\> enable</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">next\-address 16.0.0.1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">next\-address 13.0.0.3</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">next\-address 34.0.0.4</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">next\-address 45.0.0.5</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">show mpls traffic\-eng tunnels \[name\]</span></span>

**Path Option Priority**

\- Multiple path\-option commands can be configured for some tunnel with different priority value

     \-\> 1 to 1000

     \-\> Priority

          \-\> Lower value

![20141010_105514-1.jpeg](image/20141010_105514-1.jpeg)

Scenario \-\>

\- Configure MPLS TE on all routers to support max reservable bandwidth 2Mbps

\- Configure OSPF area 0

\- Configure an MPLS TE tunnel from R1 to R3 with 1Mbps bandwidth requirement

\- Use dynamic path option

R1\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng tunnels</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  mpls traffic\-eng tunnels</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  ip rsvp bandwidth 2000</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">router ospf 1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">network 12.0.0.1 0.0.0.0 area 0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">network 1.1.1.1 0.0.0.0 area 0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng router\-id lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng area 0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tu0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip unnumbered lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mode mpls traffic\-eng</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel destination 3.3.3.3</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng bandwidth 1000</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng path\-options 1 dynamic</span></span>

**Path Option Exclude Address**

R1\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip explicit\-path name NOR3 enable</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">exclude\-address 3.3.3.3</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng path\-option 1 explicit name NOR3</span></span>

**Affinity and Administrative Group**

Tunnel interface

\- Affinity

     \-\> Requirement

     \-\> 32 bit variable in HEX

     \-\> 0x00000000

On any other router in the path, administrative group a a viable resource

     \-\> 32 bit variable in HEX

     \-\> 0x0000FFFF

On headend router

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tu0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng affinity 0x01000000 mask 0x0000FFFF</span></span>

On all of the interfaces of routers in the path

R2\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int g0/0/0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng attribute\-flag 0x01000000</span></span>

**Setup and Holding Priorities**

\- Every tunnel is assigned two priorities

     \-\> Setup Priority

          \-\> Decides whether a tunnel can ask for specific bandwidth or not in case bandwidth is already assigned to another tunnel

          \-\> Preemptive

          \-\> 0 \- 7 \(lower is better\)

     \-\> Holding Priority

          \-\> Decides whether a tunnel can still reserve the bandwidth in cast another tunnel is requesting bandwidth

\- Setup priority cannot be lower than holding priority

Headend Router

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tu0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  tunnel mpls traffic\-eng priority \<setup\> \<holding\></span></span>

**Re\-Optimization**

\- When new links or more resources are available

\- 3 ways to re\-optimize

     \-\> Periodic

          \# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng reoptimize timers frequency \<minutes\></span></span>

     \-\> Event Driven

          \# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-ing reoptimize event linkup</span></span>

     \-\> Manual

          \# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng reoptimize \[tunnel \<number\>\]</span></span>

**RSVP**

\- Tailend router uses explicit null

     \-\> But penultimate router consider it as implicit null

\- On penultimate router \(hidden command\)

     \# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng signalling interpret explicit\-null</span></span>

**Forwarding Traffic over Tunnel Interface**

![20141010_105534-1.jpeg](image/20141010_105534-1.jpeg)

1. Static route

R1\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip route 50.0.0.0 255.0.0.0 tu1</span></span>

2. Policy Based Routing \(PBR\)

     \- Scenario \-\> Traffic from R6 shoulduse tunnel 1 and traffic from R7 should use tunnel 2

R1\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">route\-map MAP1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">set interface tu1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">route\-map MAP2</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">set interface tu2</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip policy route\-map MAP1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip policy route\-map MAP2</span></span>

3. Autoroute Announce

     \-\> The tunnel is included in SPF calculation as a valid exit interface

     \-\> All of the up stream network connected to the tailend router will have tunnel as the exit interface

     \-\> If multiple tunnels are available, then the nearest TE router to the destination is used as the exit interface

\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tu1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng autoroute\-announce</span></span>

**Forwading Adjacency**

\- The tunnel is considered as a single link

\- All of the non\-TE enabled routers will also consider the link when calculating best path

\- The tunnels must be two way to be considered a link

![20141010_105604-1.jpeg](image/20141010_105604-1.jpeg)

On the Headend Routers

\(route\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tu1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng forwarding\-adjacency</span></span>

**MPLS TE on XR Routers**

1. On all routers, activate MPLS TE

     \-\> for interfaces

     \-\> routing protocols

     \-\> RSVP max reservable bandwidth

2. To configure tunnel interface on headend router

     \-\> IOS

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">router ospf 1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng router\-id lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng area 0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng tunnels</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip rsvp bandwidth \<kbps\></span></span>

     \-\> XR

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int s0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">rsvp</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  bandwidth 50000</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int s0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  bandwidth 50000</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">router ospf 1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng router\-id lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">area 0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  mpls traffic\-eng</span></span>

IOS \(attribute flag\)

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng attribute\-flags 0x01000000</span></span>

XR \(attribute flag\)

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int fa0/0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  attribute\-flag 0x01000000</span></span>

IOS \(tunnel configuration\)

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tu1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip unnumbered lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mode mpls traffic\-eng</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel destination 3.3.3.3</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng bandwidth 5000</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">tunnel mpls traffic\-eng path\-option 1 explicit name EXP1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">exit</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ip explicit\-path name EXP1 enable</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">next\-address 12.0.0.2</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">next\-address 23.0.0.3</span></span>

XR \(tunnel configuration\)

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">int tunnel\-te 1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">ipv4 unnumbered lo0</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">destination 3.3.3.3</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">signalled\-bandwidth 5000</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">path\-option 1 explicit name EXP1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">explicit\-path name EXP1</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">index 10 next\-address strict ipv4</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  unicast 12.0.0.2</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">index 20 next\-address strict ipv4</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">  unicast 23.0.0.3</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels \[tunnel\-id\]</span></span>

     \-\> both IOS and XR

IOS \(reoptimization timers\)

\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng reoptimization timers frequency \<minutes\></span></span>

XR \(reoptimization timers\)

\(config\)\# <span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">mpls traffic\-eng</span></span>

<span style="background-color: #ffaaaa"><span style="background-color: #ffaaaa">reoptimization timers frequency \<minutes\></span></span>

**TE Metric**
 \- There are two metrics
     \-\> IGP metric
     \-\> TE metric
 \- By default, TE metric is used to setup the tunnel if the dynamic path\-option is used
 \- By default, TE metric is equal to IGP metric
     \-\> To change change the TE metric

R1\(config\)\# <span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng administrative\-weight \<value\></span>

![20141010_105619-1.jpeg](image/20141010_105619-1.jpeg)

Tunnel R1 \- R4

R1\(config\)\# <span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng administrative\-weight 65</span>

**TE Metric Autoroute Announce**
 \- Autoroute announce the best route metric calculation
     \-\> It uses the tunnel interface as exit for all the prefixes connected to and beyond tail\-end router

![20141010_105635-1.jpeg](image/20141010_105635-1.jpeg)

R1 \-\> R3 \+ autoroute announce

<span style="background-color: #ffaaaa">sh ip route</span>
                    exit interface
4.4.4.4          Tunnel1
3.3.3.3          Tunnel1
34.0.0.0        Tunnel1

The metric calculated for these prefixes follow some specific rules \(four\)

![20141010_105642-1.jpeg](image/20141010_105642-1.jpeg)

**Rule 1**
 \- For prefixes directly connected to the tail\-end router, the tunnel path is always used

Tunnel R1 \- R5 \(dynamic\)
 \- Both the IGP and TE metric will be the same

![20141010_105650-1.jpeg](image/20141010_105650-1.jpeg)

**Rule 2**
 \- Load balancing for the prefixes beyond the tail\-end router is possible with IGP, provided that autoroute announce metric and IGP metric is the same

![20141010_105705-1.jpeg](image/20141010_105705-1.jpeg)

**Rule 3**
 \- Autoroute announce always checks the "least cost IGP" path to reach the tail\-end router when calculating metric for the prefixes connected to and beyond the tail\-end router

![20141010_105713-1.jpeg](image/20141010_105713-1.jpeg)

**Rule 4**
 \- If the tail\-end router is in the path of the IGP route, the IGP route is not considered at all

**Fast Reroute Protection \(FRR\)**
 \- Fault tolerance mechanism
     \-\> Link protection
     \-\> Node protection
 \- FRR allows for the tunnel to continue being used in the case of link or node failure
 \- For this, it uses the concept of a backup tunnel which gets activated in the case of a link or node failure
 \- For any router that wants to protect the link, a backup tunnel is configured to become active when the protected link goes down
 \- The backup tunnel is inactive as long as the protected link is active
     \-\> Very fast switch over
          \-\> Less than 50 ms
 \- Starting point of the backup tunnel is called the Point of Local Repair \(PLR\)
 \- End point of the backup tunnel is called the merge point

![20141010_105723-1.jpeg](image/20141010_105723-1.jpeg)

An explicit tunnel is created on the PLR to reach the merge point by using the backup path
 \- This tunnel is then flagged as a backup tunnel inside the protected link's interface
     \-\> R2's fa0/1
     \-\> By doing this, the backup tunnel becomes inactive as long as the protected link is up

![20141010_105733-1.jpeg](image/20141010_105733-1.jpeg)

R1 must be configured for FRR
 \- The head\-end router of the main tunnel must be configured with the fast reroute feature
     \-\> So that it will continue sending traffic after receiving a path error message

Fast Reroute Configuration 

R1\(config\)\# <span style="background-color: #ffaaaa">int tu1</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng fast\-reroute</span>

R2\(config\)\# <span style="background-color: #ffaaaa">ip explicit\-path name 243 enable</span>
<span style="background-color: #ffaaaa"> next\-address 24.0.0.4</span>
<span style="background-color: #ffaaaa"> next\-address 34.0.0.3</span>
<span style="background-color: #ffaaaa">int tu100</span>
<span style="background-color: #ffaaaa"> ipv4 unnumbered lo0</span>
<span style="background-color: #ffaaaa"> tunnel mode mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng path\-option 1 explicit name 243</span>
<span style="background-color: #ffaaaa"> tunnel destination 3.3.3.3</span>
<span style="background-color: #ffaaaa">     \-\> The merge point</span>
<span style="background-color: #ffaaaa">     \-\> The bandwidth command is not needed</span>
<span style="background-color: #ffaaaa">int fa0/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng backup\-path tu100</span>

PLR
 \- <span style="background-color: #ffaaaa">sh mpls traffic\-eng backup</span>
     \-\> 1 configured \(0 active\)

XR Routers

R1\(config\)\# <span style="background-color: #ffaaaa">int tunnel\-te 1</span>
<span style="background-color: #ffaaaa"> fast\-reroute</span>
<span style="background-color: #ffaaaa"> root</span>
<span style="background-color: #ffaaaa"> commit</span>

R2\(config\)\# <span style="background-color: #ffaaaa">explicit\-path name 243</span>
<span style="background-color: #ffaaaa"> index 10 next\-address strict ipv4 unicast 24.0.0.4</span>
<span style="background-color: #ffaaaa"> index 20 n s i u 34.0.0.3</span>
<span style="background-color: #ffaaaa">int tunnel\-te 100</span>
<span style="background-color: #ffaaaa"> ipv4 unnumbered lo0</span>
<span style="background-color: #ffaaaa"> path\-option 1 explicit 243</span>
<span style="background-color: #ffaaaa"> destination 3.3.3.3</span>
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int fa0/1</span>
<span style="background-color: #ffaaaa">  backup\-path tunnel\-te 100</span>
<span style="background-color: #ffaaaa">  root</span>
<span style="background-color: #ffaaaa">  commit</span>

![20141010_105748-1.jpeg](image/20141010_105748-1.jpeg)

**Node Protection**
 \- Next\-to\-next hop protection
     \-\> NNHOP
 \- In node protection, next\-to\-next hop router from PLR is the merge point
 \- The backup tunnel is created with the merge point as the destination and explicit path\-option excluding the failed node
 \- When the merge point receives the PATH message, it replies with the RESV message including it's "incoming label" expected from the failed node
 \- The main tunnel's head\-end router should be configured with the fast reroute option

IOS Routers

R1\(config\)\# <span style="background-color: #ffaaaa">int tu1</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng fast\-reroute node\-protect</span>

R2\(config\)\# <span style="background-color: #ffaaaa">ip explicit\-path 245 enable</span>
<span style="background-color: #ffaaaa"> exclude\-address 3.3.3.3</span>
<span style="background-color: #ffaaaa">int tu100</span>
<span style="background-color: #ffaaaa"> ip unnumbered lo0</span>
<span style="background-color: #ffaaaa"> tunnel mode mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> tunnel destination 5.5.5.5</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng path\-option 1 explicit name 245</span>
<span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng backup\-path tu100</span>

On PLR
 \- <span style="background-color: #ffaaaa">sh mpls traffic\-eng fast\-reroute database \[detail\]</span>

**Inter\-Area Traffic Engineering**
 \- OPSF
     \-\> Multiple areas
     \-\> Opaque LSA 11
          \-\> Scope \- global \(inter\-area\)
 \- IS\-IS
     \-\> Level\-1 and Level\-2 domains
          \-\> TLV 22 \(type\-length\-value\)
               \-\> within a level
          \-\> TLV 125
               \-\> Between levels \(Level\-1 and Level\-2\)
 \- Behaves like a link state protocol only within the same area
 \- Behaves like a distance vector protocol between multiple areas

![20141010_105800-1.jpeg](image/20141010_105800-1.jpeg)

R1\(config\)\# <span style="background-color: #ffaaaa">int tu1</span>
<span style="background-color: #ffaaaa"> ip unnumbered lo0</span>
<span style="background-color: #ffaaaa"> tunnel mode mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng bandwidth 2000</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng path\-option 1 explicit name ABC</span>
<span style="background-color: #ffaaaa"> tunnel destination 6.6.6.6</span>
<span style="background-color: #ffaaaa">ip explicit\-path name ABC enable</span>
<span style="background-color: #ffaaaa"> next\-address loose 3.3.3.3</span>
     \-\> ABR
 <span style="background-color: #ffaaaa">next\-address loose 5.5.5.5</span>
     \-\> ABR
 <span style="background-color: #ffaaaa">next\-address loose 6.6.6.6</span>
     \-\> Destination

![20141010_105808-1.jpeg](image/20141010_105808-1.jpeg)

 \- On head\-end router, the tunnel is configured with an explicit path\-option with loose next\-address
 \- Loose next\-addresses are the mpls router\-ids of ABRs in the path or Layer\-1\-2 routers
 \- When the path message is to be sent, these loose addresses are expanded to a collection of strict addresses by head\-end router and every ABR

XR Routers
 
<span style="background-color: #ffaaaa">explicit\-path name ABC</span>
<span style="background-color: #ffaaaa"> index 10 next\-address loose ipv4 unicast \<ABR address\></span>

**Restriction on Inter\-Area Traffic Engineering**
 1. Cannot use dynamic path\-option
 2. Cannot use affinity / attribute\-flag
 3. Cannot use autoroute announce

![Screenshot_(17).png](image/Screenshot_(17).png)

**\! R1**
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 1.1.1.1</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng

  exit</span>
<span style="background-color: #ffaaaa">  exit</span>
**\!     \-\> One of the problems with typing the configuration into a text editor and copying and pasting the configuration into the router**
**\!     \-\> “root” also works;  if not, “mpls traffic\-eng” and the interfaces in the next section will get pasted into the “area 0"**
**\!     \-\> The other option is to configure “mpls traffic” before “router ospf 1"**

<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
\!
<span style="background-color: #ffaaaa">int tunnel\-te 16

 ipv4 unnumbered lo0</span>
<span style="background-color: #ffaaaa"> destination 6.6.6.6</span>
<span style="background-color: #ffaaaa"> signalled\-bandwidth 4000</span>
<span style="background-color: #ffaaaa"> path\-option 1 explicit name 16

explicit\-path name 16</span>
<span style="background-color: #ffaaaa"> index 10 next\-address strict ipv4 unicast 18.0.0.8</span>
<span style="background-color: #ffaaaa"> index 20 n s i u 78.0.0.7</span>
<span style="background-color: #ffaaaa"> index 30 n s i u 27.0.0.2</span>
<span style="background-color: #ffaaaa"> index 40 n s i u 23.0.0.3</span>
<span style="background-color: #ffaaaa"> index 50 n s i u 34.0.0.4</span>
<span style="background-color: #ffaaaa"> index 60 n s i u 46.0.0.6</span>
\!
<span style="background-color: #ffaaaa">int lo1</span>
<span style="background-color: #ffaaaa"> ipv4 add 1.1.1.11/32

router static</span>
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa"> 6.6.6.66/32 tunnel\-te 16</span>
 
**\! R2**
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 2.2.2.2</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/2</span>
\!
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng

  exit</span>
<span style="background-color: #ffaaaa">  exit</span>
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>

**\! R3**
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 3.3.3.3</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/2</span>
\!
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng

  exit

  exit</span>
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>

**\! R4**
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 4.4.4.4</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/2</span>
\!
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng

  exit

  exit</span>
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">  bandwidth 25000</span>

**\! R5**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 5.5.5.5</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnels

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">int e1/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng router\-id lo0

 mpls traffic\-eng area 0</span>

**\! R6**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0

router ospf 1

 router\-id 6.6.6.6

 mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnels

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">int e1/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 mpls traffic\-eng area 0</span>
\!
<span style="background-color: #ffaaaa">int tu61

 ip unnumbered lo0</span>
<span style="background-color: #ffaaaa"> tunnel mode mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng bandwidth 4000</span>
<span style="background-color: #ffaaaa"> tunnel mpls traffic\-eng path\-option 1 explicit name 61

 tunnel destination 1.1.1.1</span>
<span style="background-color: #ffaaaa">ip explicit\-path name 61 enable</span>
<span style="background-color: #ffaaaa"> next\-address 56.0.0.5</span>
<span style="background-color: #ffaaaa"> ne 35.0.0.3</span>
<span style="background-color: #ffaaaa"> ne 23.0.0.2</span>
<span style="background-color: #ffaaaa"> ne 27.0.0.7</span>
<span style="background-color: #ffaaaa"> ne 78.0.0.8</span>
<span style="background-color: #ffaaaa"> ne 18.0.0.</span><span style="background-color: #ffaaaa">1</span>
\!
<span style="background-color: #ffaaaa">int lo1</span>
<span style="background-color: #ffaaaa"> ip add 6.6.6.66 255.255.255.255</span>
<span style="background-color: #ffaaaa">ip route 1.1.1.11 255.255.255.255 tu61</span>

**\! R7**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0

int s2/0

 ip ospf 1 area 0

 ip ospf cost 1</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 7.7.7.7</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnels

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">int e1/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">int s2/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng router\-id lo0

 mpls traffic\-eng area 0</span>

**\! R8**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int s2/0

 ip ospf 1 area 0

 ip ospf cost 1</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 8.8.8.8</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnels

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">int s2/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 25000</span>
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 mpls traffic\-eng area 0

</span>

**Verification:**

**IOS:**
<span style="background-color: #ffaaaa">sh ip ospf int bri</span>
<span style="background-color: #ffaaaa">sh ip ospf nei</span>
<span style="background-color: #ffaaaa">sh ip route ospf</span>
<span style="background-color: #ffaaaa">sh mpls int</span>
<span style="background-color: #ffaaaa">sh mpls ldp nei</span>
<span style="background-color: #ffaaaa">sh mpls ldp dis</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng topology</span>
     \-\> You configured the MPLS TE router\-id right?<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa">sh ip explicit\-paths</span>
<span style="background-color: #ffaaaa">sh ip rsvp nei</span>
<span style="background-color: #ffaaaa">sh ip rsvp int</span>
<span style="background-color: #ffaaaa">sh ip rsvp sender</span>
<span style="background-color: #ffaaaa">sh ip rsvp reservation</span>

**XR:**
<span style="background-color: #ffaaaa">sh ospf int rbi</span>
<span style="background-color: #ffaaaa">sh ospf nei</span>
<span style="background-color: #ffaaaa">sh route ospf</span>
<span style="background-color: #ffaaaa">sh mpls int</span>
<span style="background-color: #ffaaaa">sh mpls ldp nei bri</span>
<span style="background-color: #ffaaaa">sh mpls ldp dis</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng topology</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa">sh explicit\-paths</span>
<span style="background-color: #ffaaaa">sh rsvp nei</span>
<span style="background-color: #ffaaaa">sh rsvp int</span>
<span style="background-color: #ffaaaa">sh rsvp sender</span>
<span style="background-color: #ffaaaa">sh rsvp reservation</span>
<span style="background-color: #ffaaaa">

</span>

**\! R1**
<span style="background-color: #ffaaaa">sh ip int bri</span>
     \-\> Is the tunnel up?<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
     \-\> Will tell you why the tunnel is not up
<span style="background-color: #ffaaaa">ping 6.6.6.66</span>

<span style="background-color: #ffaaaa">traceroute 6.6.6.66</span>
<span style="background-color: #ffaaaa">sh rsvp int</span>
     \-\> Did the bandwidth get allocated?
<span style="background-color: #ffaaaa">sh rsvp sender</span>
<span style="background-color: #ffaaaa">sh rsvp reservation</span>

**\! R6**
<span style="background-color: #ffaaaa">sh ip int bri</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa">ping 1.1.1.11</span>

<span style="background-color: #ffaaaa">traceroute 1.1.1.11</span>
<span style="background-color: #ffaaaa">sh rsvp int</span>
<span style="background-color: #ffaaaa">sh ip rsvp sender</span>
<span style="background-color: #ffaaaa">sh ip rsvp reservation</span>

<span style="background-color: #ffaaaa">

</span>

![Screenshot_(18).png](image/Screenshot_(18).png)

**\! R1**
<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 router\-id 1.1.1.1

 mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1

router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
\!
<span style="background-color: #ffaaaa">int tunnel\-te 16</span>
<span style="background-color: #ffaaaa"> ipv4 unnumbered lo0</span>
<span style="background-color: #ffaaaa"> destination 6.6.6.6</span>
<span style="background-color: #ffaaaa"> signalled\-bandwidth 4000</span>
<span style="background-color: #ffaaaa"> path\-option 1 explicit name 16</span>
<span style="background-color: #ffaaaa">explicit\-path name 16</span>
<span style="background-color: #ffaaaa"> index 10 next\-address strict ipv4 unicast 12.0.0.2</span>
<span style="background-color: #ffaaaa"> index 20 n s i u 23.0.0.3</span>
<span style="background-color: #ffaaaa"> index 30 n s i u 35.0.0.5</span>
<span style="background-color: #ffaaaa"> index 40 n s i u 56.0.0.</span><span style="background-color: #ffaaaa">6</span>
\!
<span style="background-color: #ffaaaa">int tunnel\-te 16

 fast\-reroute</span>
\!
<span style="background-color: #ffaaaa">router static</span>
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">  6.6.6.66/32 tunnel\-te 16</span>

**\! R2**
<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 router\-id 2.2.2.2

 mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/2</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
\!
<span style="background-color: #ffaaaa">int tunnel\-te 100</span>
<span style="background-color: #ffaaaa"> ipv4 unnumbered lo0</span>
<span style="background-color: #ffaaaa"> destination 3.3.3.3</span>
<span style="background-color: #ffaaaa"> path\-option 1 explicit name 2743

explicit\-path name 2743</span>
<span style="background-color: #ffaaaa"> index 10 n s i u 27.0.0.7</span>
<span style="background-color: #ffaaaa"> index 20 n s i u 47.0.0.4</span>
<span style="background-color: #ffaaaa"> index 30 n s i u 34.0.0.3</span>
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  backup\-path tunnel\-te 100</span>

**\! R3**
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 3.3.3.3</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/2</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>

**\! R4**
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 4.4.4.4</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/2</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">router ospf 1

 mpls traffic\-eng router\-id lo0

 area 0

  mpls traffic\-eng</span>
<span style="background-color: #ffaaaa">rsvp</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/1</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>
<span style="background-color: #ffaaaa"> int gi0/0/0/2</span>
<span style="background-color: #ffaaaa">  bandwidth 15000</span>

**\! R5**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 5.5.5.5</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnel

router ospf 1

 mpls traffic\-eng area 0

 mpls traffic\-eng router\-id lo0

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>
<span style="background-color: #ffaaaa">int e1/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>

**\! R6**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 6.6.6.6</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnel

router ospf 1

 mpls traffic\-eng area 0

 mpls traffic\-eng router\-id lo0

int e1/0

 mpls traffic\-eng tunnels

 ip rsvp bandwidth 105000

int e1/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>
\!
<span style="background-color: #ffaaaa">int lo1</span>
<span style="background-color: #ffaaaa"> ip add 6.6.6.66 255.255.255.255</span>

**\! R7**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0

int s2/0

 ip ospf 1 area 0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 7.7.7.7</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnel

router ospf 1

 mpls traffic\-eng area 0

 mpls traffic\-eng router\-id lo0

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>
<span style="background-color: #ffaaaa">int e1/1</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>
<span style="background-color: #ffaaaa">int s2/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>

**\! R8**
<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int s2/0

 ip ospf 1 area 0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 8.8.8.8</span>
<span style="background-color: #ffaaaa"> mpls ldp autoconfig</span>
\!
<span style="background-color: #ffaaaa">mpls traffic\-eng tunnel

router ospf 1

 mpls traffic\-eng area 0

 mpls traffic\-eng router\-id lo0

int e1/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>
<span style="background-color: #ffaaaa">int s2/0</span>
<span style="background-color: #ffaaaa"> mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa"> ip rsvp bandwidth 15000</span>

Verification:

**IOS:**
<span style="background-color: #ffaaaa">sh ip ospf int bri</span>
<span style="background-color: #ffaaaa">sh ip ospf nei</span>
<span style="background-color: #ffaaaa">sh ip route ospf</span>
<span style="background-color: #ffaaaa">sh mpls int</span>
<span style="background-color: #ffaaaa">sh mpls ldp nei</span>
<span style="background-color: #ffaaaa">sh mpls ldp dis</span>

<span style="background-color: #ffaaaa">sh mpls traffic\-eng topology</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels protection</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels backup</span><span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng fast\-reroute database state complete</span>
<span style="background-color: #ffaaaa">sh ip explicit\-paths</span>
<span style="background-color: #ffaaaa">sh ip rsvp nei</span>

<span style="background-color: #ffaaaa">sh ip rsvp int</span>
<span style="background-color: #ffaaaa">sh ip rsvp sender</span>
<span style="background-color: #ffaaaa">sh ip rsvp reservation</span>

<span style="background-color: #ffaaaa">

</span>

**XR:**
<span style="background-color: #ffaaaa">sh ospf int rbi</span>
<span style="background-color: #ffaaaa">sh ospf nei</span>
<span style="background-color: #ffaaaa">sh route ospf</span>
<span style="background-color: #ffaaaa">sh mpls int</span>
<span style="background-color: #ffaaaa">sh mpls ldp nei bri</span>
<span style="background-color: #ffaaaa">sh mpls ldp dis</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng topology</span>

<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels protection</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels backup</span><span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng fast\-reroute database state complete</span>
<span style="background-color: #ffaaaa">sh explicit\-paths</span>
<span style="background-color: #ffaaaa">sh rsvp nei</span>

<span style="background-color: #ffaaaa">sh rsvp int</span>
<span style="background-color: #ffaaaa">sh rsvp sender</span>
<span style="background-color: #ffaaaa">sh rsvp reservation</span>

**\! R1**

<span style="background-color: #ffaaaa">sh ip int bri</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
<span style="background-color: #ffaaaa">ping 6.6.6.66</span>
<span style="background-color: #ffaaaa">traceroute 6.6.6.66</span>
     \-\> Shutdown interface gi0/0/0/0 on R2 and run traceroute again
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels protection</span>
<span style="background-color: #ffaaaa">sh rsvp int</span>
<span style="background-color: #ffaaaa">sh rsvp sender</span>
<span style="background-color: #ffaaaa">sh rsvp reservation</span>

<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
**\! R2**
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels backup</span><span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh mpls traffic\-eng fast\-reroute database state complete</span>
     \-\> Shutdown interface gi0/0/0/0 and run this commands again<span style="background-color: #ffaaaa">

</span>

**\! R6**
<span style="background-color: #ffaaaa">sh mpls traffic\-eng tunnels</span>
