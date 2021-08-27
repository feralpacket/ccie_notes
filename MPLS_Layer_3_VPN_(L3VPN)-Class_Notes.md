# MPLS Layer 3 VPN (L3VPN) - Class Notes

**MPLS Layer 3 VPN \(L3VPN\)** \(12 Sept 2014\)

Lab: L3VPN

![20141003_113210-1.jpeg](image/20141003_113210-1.jpeg)

MP\-BGP

\- Multiprotocol Border Gateway Protocol

PE

\- Provider Edge router

P

\- Provider router

CE

\- Customer Edge router

Control Plane

\- PE \- CE

     \-\> IGP or eBGP

\- PE \- PE

     \-\> MP\-BGP

Data Plane

\- When traffic will be sent, MPLS will be used

PE routers create one VRF \(Virtual Routing and Forwarding\) instance per customer

\- VRF has a separate FIB table per customer

\- VRF parameters

     \-\> Name of VRF table

     \-\> Route Distinguisher \(RD\)

          \-\> 64 bit value represented as \<32 bit\>:\<32 bit\>

          \-\> 50:60

          \-\> Used to make customer routes unique

RD \+ IPv4 address = VPNv4 router

     \-\> 64 bit \+ 32 bits = 96 bits

![20141003_113222-1.jpeg](image/20141003_113222-1.jpeg)

     \-\> Route Target \(RT\)

          \-\> 64 bit value represented as \<32 bit\>:\<32 bit\>

          \-\> To control the distination of routes, the concept of import and export is used

![20141003_113230-1.jpeg](image/20141003_113230-1.jpeg)

**Configuration on PE routers**

**Phase 1**

1. Create the VRF

2. Adding RD value

     \-\> Only one RD value per VRF

3. One or more RTs

     \-\> Import

     \-\> Export

4. Interface association with the VRF

5. PE \- CE protocol association

R1 \(config\)\# <span style="background-color: #ffaaaa">ip vrf c1b1</span>

<span style="background-color: #ffaaaa">rd 100:100</span>

<span style="background-color: #ffaaaa">route\-target import 100:100</span>

<span style="background-color: #ffaaaa">route\-target export 100:100</span>

<span style="background-color: #ffaaaa">int s0/0</span>

  <span style="background-color: #ffaaaa">ip vrf forwarding c1b1</span>

  <span style="background-color: #ffaaaa">ip add 14.0.0.1 255.0.0.0</span>

<span style="background-color: #ffaaaa">sh ip route</span>

  C     12.0.0.0

  C     1.0.0.0

<span style="background-color: #ffaaaa">sh ip route vrf c1b1</span>

  C     14.0.0.0

R1\(config\)\# <span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa">address\-family ipv4 vrf c1b1</span>

  <span style="background-color: #ffaaaa">version 2</span>

  <span style="background-color: #ffaaaa">no auto\-summary</span>

  <span style="background-color: #ffaaaa">network 14.0.0.0</span>

<span style="background-color: #ffaaaa">sh ip route vrf c1b1</span>

  C     14.0.0.0

  R     4.4.4.4

Configure RIP on the CE routers

After Phase 1, the PE routers will have the local client routes

![20141003_113248-1.jpeg](image/20141003_113248-1.jpeg)

**Phase 2**

1. Configure MP\-BGP on PE routers

2. Form VPNv4 neighbor relationship between PE routers using /32 loopback interfaces

3. Do mutual redistribution between the PE \- CE IGP and the MP\-BGP VRF\-to\-VRF

RD \+ IPv4 Address = VPNv4 route

100:100:4.4.4.4

R1\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa">neighbor 3.3.3.3 remote\-as 100</span>

<span style="background-color: #ffaaaa">neighbor 3.3.3.3 update\-source lo0</span>

<span style="background-color: #ffaaaa">address\-family vpnv4 unicast</span>

  <span style="background-color: #ffaaaa">neighbor 3.3.3.3 activate</span>

  <span style="background-color: #ffaaaa">neighbor 3.3.3.3 send\-community extended</span>

     \-\> This is automatically added to the configuration when the neighbor is activated

<span style="background-color: #ffaaaa">  address\-family ipv4 vrf c1b1

   redistribute rip</span>

Extended community is used to send and receive IGP parameters from one customer site to another customer site

R1\(config\)\# <span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa">address\-family ipv4 vrf c1b1</span>

  <span style="background-color: #ffaaaa">redistribute bgp 100 metric { transparent | \<value\> }</span>

Transparent

  RIP \-\> BGP's MED \-\> BGP's MED \-\> RIP metric

![20141003_113258-1.jpeg](image/20141003_113258-1.jpeg)

BGP adds a VPN label when routes are redistributed into BGP

BGP VPN Label = 100 for 4.4.4.4

![20141003_113311-1.jpeg](image/20141003_113311-1.jpeg)

<span style="background-color: #ffaaaa">sh mpls forwarding\-table</span>

     \-\> To check the LFIB

BGP

\- IPv4

\- IPv6

**XR Routers**

<span style="background-color: #ffaaaa">router bgp \<asn\>

 address\-family \<afi\> \<safi\></span>

     \-\> Address\-family Identifier

          \-\> ipv4

               \-\> 32 bit

          \-\> ipv6

               \-\> 128 bit

          \-\> vpnv4

               \-\> RD \+ ipv4

               \-\> 64 bits \+ 32 bits = 96 bits

               \-\> Client IPv4 addresses

          \-\> vpnv6

               \-\> RD \+ ipv6

               \-\> 64 bits \+ 128 bits = 192 bits

               \-\> Client IPv6 addresses

     \-\> Subsequent Address\-family Identifier

          \-\> unicast

          \-\> multicast

          \-\> mdt

               \-\> Multicast Distribution Tree

<span style="background-color: #ffaaaa">router bgp \<asn\>

address\-family \<afi\> \<safi\>

  network \<ip add\>/\<nm\>

  redistribute \<protocol\>

  exit

neighbor \<ip add\>

  remote\-as \<asn\>

  update\-source \<int\>

  ebgp\-multihop \<ttl\>

address\-family \<afi\> \<safi\>

  route\-reflector\-client

  next\-hop\-self

  route\-policy \<name\> in | out

  as\-override

  default\-originate</span>

![20141010_140829-1.jpeg](image/20141010_140829-1.jpeg)

Configure BGP between R1 and R2 for IPv4 and IPv6
 \- Advertise loopbacks
 \- Disable the default address\-familty
     \-\> This is commonly seen in the lab

R1\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> no bgp default ipv4\-unicast</span>
<span style="background-color: #ffaaaa"> neighbor 12.0.0.2 remote\-as 100</span>
<span style="background-color: #ffaaaa"> neighbor 2002:12::2 remote\-as 100</span>
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">  neighbor 12.0.0.2 activate</span>
<span style="background-color: #ffaaaa">  network 12.0.0.0 mask 255.0.0.0</span>
<span style="background-color: #ffaaaa">  network 1.1.1.1 mask 255.255.255.255</span>
<span style="background-color: #ffaaaa"> address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  neighbor 2002:12::2 activate</span>
<span style="background-color: #ffaaaa">  network 2002:12::/64</span>
<span style="background-color: #ffaaaa">  network 2002:1:1:1::1/128</span>

R2\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>
      \-\> On the XR router, the is no default address\-family
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">  network 2.2.2.2/32</span>
<span style="background-color: #ffaaaa">  network 12.0.0.0/8</span>
<span style="background-color: #ffaaaa"> address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  network 2002:12::/8</span>
<span style="background-color: #ffaaaa">  network 2002:2:2:2::2/128</span>
<span style="background-color: #ffaaaa"> neighbor 12.0.0.1</span>
<span style="background-color: #ffaaaa">  remote\-as 100</span>
<span style="background-color: #ffaaaa">  address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2002:12::1</span>
<span style="background-color: #ffaaaa">  remote\-as 100</span>
<span style="background-color: #ffaaaa">  address\-family ipv6 unicast</span>

<span style="background-color: #ffaaaa">sh bgp \<afi\> \<safi\></span>
<span style="background-color: #ffaaaa">sh bgp ipv4 unicast</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 unicast</span>

eBGP
 \- By default, no IPv4 or IPv6 updates can be exchanged
     \-\> A route\-policy has to be applied to the incoming and outgoing directions inside the address\-family
 \- Create the route\-policy in global config

<span style="background-color: #ffaaaa">route\-policy PASS</span>
<span style="background-color: #ffaaaa"> pass</span>
<span style="background-color: #ffaaaa"> exit</span>

 \- Apply the route\-policy to the address\-family

<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor \<ip add\></span>
<span style="background-color: #ffaaaa">  address\-family \<afi\> \<safi\></span>
<span style="background-color: #ffaaaa">   route\-policy PASS in</span>
<span style="background-color: #ffaaaa">   route\-policy PASS out</span>

![20141010_140838-1.jpeg](image/20141010_140838-1.jpeg)

![20141010_140858-1.jpeg](image/20141010_140858-1.jpeg)

L3VPN
 1. VRF creation
 2. PE \- CE interface association
 3. IGP association \( PE \- CE \)
 4. MP\-BGP \( PE \- PE \)
 5. Mutual redistribution between VRF IGP and MP\-BGP

 \- ISP Core
     \-\> IGP \+ LDP

XR Routers

R1\(config\)\# <span style="background-color: #ffaaaa">vrf ABC</span>
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">  import route\-target 1:1</span>
<span style="background-color: #ffaaaa">  export route\-target 1:1</span>

RD value is configured inside BGP on XR routers
 **\- This is easy to forget**

VRF interface association
 \- On XR routers, the IPv4 and IPv6 addresses must be removed before associating the interface to the VRF

R1\(config\)\# <span style="background-color: #ffaaaa">int g0/0/0/0</span>
<span style="background-color: #ffaaaa"> no ipv4 add</span>
<span style="background-color: #ffaaaa"> no ipv6 add</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa"> ipv4 \<ip add\>/\<nm\></span>
<span style="background-color: #ffaaaa"> ipv6 \<ip add\>/\<nm\></span>
<span style="background-color: #ffaaaa">router rip</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  int g0/0/0/0</span>
<span style="background-color: #ffaaaa">router eigrp 100</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">   autonomous\-system 100</span>
<span style="background-color: #ffaaaa">   int g0/0/0/0</span>
<span style="background-color: #ffaaaa">  address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">   autonomous\-system 100</span>
<span style="background-color: #ffaaaa">   int g0/0/0/0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  area 0</span>
<span style="background-color: #ffaaaa">   int g0/0/0/0</span>
<span style="background-color: #ffaaaa">root</span>
<span style="background-color: #ffaaaa">show config</span>
<span style="background-color: #ffaaaa">commit</span>

MP\-BGP
 \- VPNv4 address\-family
     \-\> PE \- PE
 \- Mutual redistribution with VRF IGP

R1\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa"> address\-family vpn4 unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2.2.2.2</span>
<span style="background-color: #ffaaaa">  remote\-as 100</span>
<span style="background-color: #ffaaaa">  update\-source lo0</span>
<span style="background-color: #ffaaaa">  address\-family vpn4 unicast</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  rd 1:1</span>
     **\-\> This is easy to forget**
<span style="background-color: #ffaaaa">  address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">   redistribute ospf 1</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  redistribute bgp 100</span>
<span style="background-color: #ffaaaa">  area 0</span>
<span style="background-color: #ffaaaa">  int g0/0/0/0</span>

During the SP lab, there will be 6 PE routers and 6 CE routers

If PE \- CE protocol is BGP

R1\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa"> address\-family vpnv4 unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2.2.2.2</span>
<span style="background-color: #ffaaaa">  remote\-as 100</span>
<span style="background-color: #ffaaaa">  update\-source lo0</span>
<span style="background-color: #ffaaaa">  address\-family vpnv4 unicast</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa">  rd 1:1</span>
<span style="background-color: #ffaaaa">  address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">   redistribute connected</span>
<span style="background-color: #ffaaaa">  neighbor 13.0.0.3</span>
<span style="background-color: #ffaaaa">   remote\-as 50</span>
<span style="background-color: #ffaaaa">   address\-family ipv4 unicast</span>
<span style="background-color: #ffaaaa">    route\-policy PASS in</span>
<span style="background-color: #ffaaaa">    route\-policy PASS out</span>
<span style="background-color: #ffaaaa">    as\-override</span>

![Screenshot_(3).png](image/Screenshot_(3).png)

**\! R1**
<span style="background-color: #ffaaaa">router eigrp 1

 address\-family ipv4

  int lo0

  int gi0/0/0/1

</span>

**\! R2**
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 2.2.2.2</span>
<span style="background-color: #ffaaaa"> area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!

<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 mpls ldp auto\-config

</span> \!

<span style="background-color: #ffaaaa">router bgp 100

 bgp router\-id 2.2.2.2

 address\-family vpnv4 unicast

 neighbor 5.5.5.5

  remote\-as 100

  update\-source lo0

  address\-family vpnv4 unicast

</span> \!

<span style="background-color: #ffaaaa">vrf ABC

 address\-family ipv4 unicast

  import route\-target 1:1

  export route\-target 1:1

router bgp 100

 vrf ABC

  rd 1:1

  address\-family ipv4 unicast</span>
\!
<span style="background-color: #ffaaaa">int g0/0/0/0</span>
<span style="background-color: #ffaaaa"> no ipv4 address 12.0.0.2 255.255.255.0</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa"> ipv4 address 12.0.0.2 255.255.255.0</span>
\!

<span style="background-color: #ffaaaa">router eigrp 1

 vrf ABC

  address\-family ipv4</span>
<span style="background-color: #ffaaaa">   autonomous\-system 1</span>
<span style="background-color: #ffaaaa">   int gi0/0/0/0</span>
\!

<span style="background-color: #ffaaaa">router bgp 100

 vrf ABC

 address\-family ipv4 unicast

  redistribute eigrp 1

router eigrp 1

 vrf ABC

 address\-family ipv4</span>
<span style="background-color: #ffaaaa">  redistribute bgp 100</span>

**\! R3**
<span style="background-color: #ffaaaa">int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0

router ospf 1

 router\-id 3.3.3.3

</span> \!

<span style="background-color: #ffaaaa">mpls label protocol ldp

mpls ldp router\-id lo0 force

int e1/0

 mpls ip

int e1/1</span>
<span style="background-color: #ffaaaa"> mpls ip</span>
 

**\! R4**
<span style="background-color: #ffaaaa">router ospf 1

 router\-id 4.4.4.4

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!

<span style="background-color: #ffaaaa">mpls ldp

router ospf 1</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config</span>

**\! R5**
<span style="background-color: #ffaaaa">router ospf 1

 router\-id 5.5.5.5

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!

<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 mpls ldp auto\-config

</span> \!

<span style="background-color: #ffaaaa">router bgp 100

 bgp router\-id 5.5.5.5</span>
<span style="background-color: #ffaaaa"> address\-family vpnv4 unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2.2.2.2</span>
<span style="background-color: #ffaaaa">  remote\-as 100

  update\-source lo0

  address\-family vpnv4 unicast

</span> \!

<span style="background-color: #ffaaaa">vrf ABC

 address\-family ipv4 unicast

  import route\-target 1:1

  export route\-target 1:1

router bgp 100

 vrf ABC

  rd 1:1

  address\-family ipv4 unicast</span>
\!
<span style="background-color: #ffaaaa">int gi0/0/0/0</span>
<span style="background-color: #ffaaaa"> no ipv4 address 35.0.0.3 255.255.255.0</span>
<span style="background-color: #ffaaaa"> vrf ABC</span>
<span style="background-color: #ffaaaa"> ipv4 address 35.0.0.3 255.255.255.0</span>
\!

<span style="background-color: #ffaaaa">router eigrp 1

 vrf ABC

  address\-family ipv4</span>
<span style="background-color: #ffaaaa">   autonomous\-system 1</span>
<span style="background-color: #ffaaaa">   int gi0/0/0/0</span>
\!

<span style="background-color: #ffaaaa">router bgp 100

 vrf ABC

 address\-family ipv4 unicast

  redistribute eigrp 1

router eigrp 1

 vrf ABC

 address\-family ipv4

  redistribute bgp 100</span>

**\! R6�**�
<span style="background-color: #ffaaaa">router eigrp 1</span>
<span style="background-color: #ffaaaa"> network 6.6.6.6 0.0.0.0</span>
<span style="background-color: #ffaaaa"> network 56.0.0.6 0.0.0.0</span>

**Verification**

**\! R1**
<span style="background-color: #ffaaaa">sh eigrp int</span>
<span style="background-color: #ffaaaa">sh eigrp nei</span>
<span style="background-color: #ffaaaa">sh ip route</span>
<span style="background-color: #ffaaaa">ping 6.6.6.6</span>

**\! R2 and R5**
<span style="background-color: #ffaaaa">sh ospf nei</span>
<span style="background-color: #ffaaaa">sh route ospf</span>
<span style="background-color: #ffaaaa">sh mpls int</span>
<span style="background-color: #ffaaaa">sh mpls ldp nei bri</span>
<span style="background-color: #ffaaaa">sh bgp vpnv4 u sum</span>
<span style="background-color: #ffaaaa">sh bgp vpnv4 u</span>
<span style="background-color: #ffaaaa">sh vrf ABC</span>
<span style="background-color: #ffaaaa">sh eigrp vrf ABC nei</span>
<span style="background-color: #ffaaaa">sh route vrf ABC eigrp</span>
<span style="background-color: #ffaaaa">ping vrf ABC 1.1.1.1</span>
<span style="background-color: #ffaaaa">ping vrf ABC 6.6.6.6</span>
<span style="background-color: #ffaaaa">sh bgp vrf ABC</span>
<span style="background-color: #ffaaaa">sh route vrf ABC bgp</span>
<span style="background-color: #ffaaaa">sh eigrp vrf ABC topology</span>

**\! R6**
<span style="background-color: #ffaaaa">sh ip eigrp int</span>
<span style="background-color: #ffaaaa">sh ip eigrp nei</span>
<span style="background-color: #ffaaaa">sh ip route</span>
<span style="background-color: #ffaaaa">ping 1.1.1.1</span>
