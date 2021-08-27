# IPv6 MPLS L3VPN (6PE) - Class Notes

**IPv6 MPLS L3VPN \(6PE\)** \(15 Sept 2014\)
Lab: 6PE

 \- 6PE
 \- 6vPE
     \-\> Small "v" means VRF
 \- Client side
     \-\> IPv6
 \- PE routers
     \-\> IPv6 over IPv4 \(6to4\)
     \-\> Dual stack
          \-\> IPv4 towards core of SP
          \-\>IPv6 towards the client
 \- P \(provider\) routers
     \-\> IPv4 only

LDP only works with IPv4

**6PE**
 \- Without VRFs on PE routers
 \- IPv6 address\-family between PE \- PE  and PE \- CE routers
 \- Not as easy to configure as it should seem

**6vPE**
 \- with VRFs on PE routers
 \- VPNv6 address\-family between PE \- PE routers
 \- IPv6 address\-family between PE \- CE routers

![20141010_140933-1.jpeg](image/20141010_140933-1.jpeg)

![20141010_140939-1.jpeg](image/20141010_140939-1.jpeg)

Transport label
 \- LDP
     \-\> for BGP next\-hop

VPN label
 \- BGP VPNv4
     \-\> for client networks

![20141010_140946-1.jpeg](image/20141010_140946-1.jpeg)

IPv6 Table

               next\-hop
               ::ffff:\<ipv4 address of the PE router\>

**6PE Rules**
 1. Client has an IPv6 neighbor relationship with PE router
 2. PE routers will perform mutual redistribution between "IPv6 IGP / eBGP" and "MP\-BGP IPv6"
 3. PE \- PE will be IPv6 \+ label

**6PE on IOS Routers**

R4\(config\)\# <span style="background-color: #ffaaaa">ipv6 eigrp 1</span>
<span style="background-color: #ffaaaa"> no shut</span>
<span style="background-color: #ffaaaa"> eigrp router\-id 4.4.4.4</span>
<span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> ipv6 eigrp 1</span>
<span style="background-color: #ffaaaa">int lo0</span>
<span style="background-color: #ffaaaa"> ipv6 eigrp 1</span>

R1\(config\)\# <span style="background-color: #ffaaaa">ipv6 router eigrp 1</span>
<span style="background-color: #ffaaaa"> no shut</span>
<span style="background-color: #ffaaaa"> eigrp router\-id 1.1.1.1</span>
<span style="background-color: #ffaaaa">int fa0/0</span>
<span style="background-color: #ffaaaa"> ipv6 eigrp 1</span>
<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> no bgp default ipv4\-unicast</span>
<span style="background-color: #ffaaaa"> neghbor 3.3.3.3 remote\-as 100</span>
<span style="background-color: #ffaaaa"> neighbor 3.3.3.3 update\-source lo0</span>
<span style="background-color: #ffaaaa"> address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  neighbor 3.3.3.3 activate</span>
<span style="background-color: #ffaaaa">  neighbor 3.3.3.3 send\-label</span>
     \-\> Have to force the router to send labels in IPv6
     \-\> Sending labels is the default in IPv4
  <span style="background-color: #ffaaaa">redistribute eigrp 1</span>
<span style="background-color: #ffaaaa">ipv6 router eigrp 1</span>
<span style="background-color: #ffaaaa"> redistribute bgp 100 metric 1 1 1 1 1</span>

Problem with 6PE
 \- There is no isolation that can be provided by VRFs
 \- PE routers have to be dedicated for each customer
     \-\> Otherwise, the customers will see the IPv6 routes of any other customer connected to the PE router

R3
 \- LDP
     \-\> Label for 1.1.1.1
          \-\> 16 \(transport label\)
 \- BGP
     \-\> Label for 2002:14::4
          \-\> 20 \(VPN label\)

<span style="background-color: #ffaaaa">sh bgp ipv6 unicast</span>
                         next\-hop
2002:14::4         ::ffff:1.1.1.1

<span style="background-color: #ffaaaa">sh bgp ipv6 u label</span>
 \- If labels are not present, then "neighbor x.x.x.x send\-label" configuration is missing

     | 16 | 20 | IPv6 |

To check PE \- PE IPv6 communicastion, the following command is required

R1\(config\)\# <span style="background-color: #ffaaaa">mpls ipv6 source\-interface \<int\></span>

![20141010_140958-1.jpeg](image/20141010_140958-1.jpeg)

**eBGP is required for PE \- CE on XR routers**

R1\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  allocate\-label all</span>
<span style="background-color: #ffaaaa"> neighbor 3.3.3.3</span>
<span style="background-color: #ffaaaa">  remote\-as 100</span>
<span style="background-color: #ffaaaa">  update\-source lo0</span>
<span style="background-color: #ffaaaa">  address\-family ipv6 </span><span style="background-color: #ffaaaa">labeled\-unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2002:14::4</span>
<span style="background-color: #ffaaaa">  remote\-as 50</span>
<span style="background-color: #ffaaaa">  address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">   route\-policy PASS in</span>
<span style="background-color: #ffaaaa">   route\-policy PASS out</span>
<span style="background-color: #ffaaaa">   as\-override</span>
<span style="background-color: #ffaaaa">route\-pocily PASS</span>
<span style="background-color: #ffaaaa"> pass</span>
<span style="background-color: #ffaaaa"> root</span>
<span style="background-color: #ffaaaa"> show config</span>
<span style="background-color: #ffaaaa"> commit</span>
<span style="background-color: #ffaaaa">

</span>

**IOS:**

<span style="background-color: #ffaaaa">sh bgp vpnv4 u all sum</span>
<span style="background-color: #ffaaaa">sh bgp vpnv4 u all</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh bgp vpnv6 u all sum</span>
<span style="background-color: #ffaaaa">sh bgp vpnv6 u all</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
**XR:**
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh bgp vpnv4 u sum</span>
<span style="background-color: #ffaaaa">sh bgp vpnv4 u</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh bgp vpnv6 u sum</span>
<span style="background-color: #ffaaaa">sh bgp vpnv6 u</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
![Screenshot_(9).png](image/Screenshot_(9).png)

**\! R1**
<span style="background-color: #ffaaaa">router bgp 50</span>
<span style="background-color: #ffaaaa"> bgp router\-id 1.1.1.1</span>
<span style="background-color: #ffaaaa"> address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  network 2002:1:1:1::1/128</span>
<span style="background-color: #ffaaaa"> neighbor 2002:12::2</span>
<span style="background-color: #ffaaaa">  remote\-as 100

  address\-family ipv6 unicast

  route\-policy allow\-all in

  route\-policy allow\-all out

route\-policy allow\-all

 pass

 end</span>
 
**\! R2**
<span style="background-color: #ffaaaa">router ospf 1

 router\-id 2.2.2.2

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!
<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 mpls ldp auto\-config</span>
\!
<span style="background-color: #ffaaaa">router bgp 100

 bgp router\-id 2.2.2.2

 address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa">  allocate\-label all</span>
<span style="background-color: #ffaaaa"> neighbor 2002:12::1</span>
<span style="background-color: #ffaaaa">  remote\-as 50

  address\-family ipv6 unicast

   route\-policy allow\-all in

   route\-policy allow\-all out

   as\-override

route\-policy allow\-all

 pass

 end</span>
\!
<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor 5.5.5.5</span>
<span style="background-color: #ffaaaa">  remote\-as 100

  update\-source lo0

  address\-family ipv6 labeled\-unicast

</span>

**\! R3**
<span style="background-color: #ffaaaa">int lo0

 ip ospf 1 area 0

int e1/0

 ip ospf 1 area 0

int e1/1

 ip ospf 1 area 0</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> router\-id 3.3.3.3</span>
\!
<span style="background-color: #ffaaaa">mpls label protocol ldp</span>
<span style="background-color: #ffaaaa">mpls ldp router\-id lo0 force</span>
<span style="background-color: #ffaaaa">int e1/0</span>
<span style="background-color: #ffaaaa"> mpls ip

int e1/1</span>
<span style="background-color: #ffaaaa"> mpls ip</span>
<span style="background-color: #ffaaaa">

</span>
**\! R4**
<span style="background-color: #ffaaaa">router ospf 1

 router\-id 4.4.4.4

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!
<span style="background-color: #ffaaaa">mpls ldp</span>
<span style="background-color: #ffaaaa">router ospf 1</span>
<span style="background-color: #ffaaaa"> mpls ldp auto\-config</span>

**\! R5**
<span style="background-color: #ffaaaa">router ospf 1

 router\-id 5.5.5.5

 area 0</span>
<span style="background-color: #ffaaaa">  int lo0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/0</span>
<span style="background-color: #ffaaaa">  int gi0/0/0/1</span>
\!
<span style="background-color: #ffaaaa">mpls ldp

router ospf 1

 mpls ldp auto\-config</span>
\!
<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa">bgp router\-id 5.5.5.5</span>
<span style="background-color: #ffaaaa">address\-family ipv6 unicast</span>
<span style="background-color: #ffaaaa"> allocate\-label all</span>
<span style="background-color: #ffaaaa"> neighbor 2002:56::6</span>
<span style="background-color: #ffaaaa">  remote\-as 50

  address\-family ipv6 unicast

   route\-policy allow\-all in

   route\-policy allow\-all out

   as\-override

route\-policy allow\-all

 pass

 end</span>
\!
<span style="background-color: #ffaaaa">router bgp 100</span>
<span style="background-color: #ffaaaa"> neighbor 2.2.2.2</span>
<span style="background-color: #ffaaaa">  remote\-as 100

  update\-source lo0

  address\-family ipv6 labeled\-unicast</span>

**\! R6**
<span style="background-color: #ffaaaa">router bgp 50

 bgp router\-id 6.6.6.6</span>
<span style="background-color: #ffaaaa"> no bgp default ipv4\-unicast</span>
<span style="background-color: #ffaaaa"> neighbor 2002:56::5 remote\-as 100</span>
<span style="background-color: #ffaaaa"> address\-family ipv6</span>
<span style="background-color: #ffaaaa">  network 2002:6:6:6::6/128</span>
<span style="background-color: #ffaaaa">  neighbor 2002:56::5 activate</span>

**Verification:**
**\! R1 and R6**
<span style="background-color: #ffaaaa">sh bgp ipv6 u sum</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 u</span>
<span style="background-color: #ffaaaa">sh route ipv6</span>
<span style="background-color: #ffaaaa">ping ipv6 2002:6:6:6::6 source 2002:1:1:1::1</span>
<span style="background-color: #ffaaaa">ping ipv6 2002:1:1:1::1 source 2002:6:6:6::6</span>

**\! R2 and R5**
<span style="background-color: #ffaaaa">sh mpls int</span>

<span style="background-color: #ffaaaa">sh mpls ldp nei bri</span>
<span style="background-color: #ffaaaa">sh mpls forwarding</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 u sum</span>
<span style="background-color: #ffaaaa">sh bpg ipv6 u</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 all sum</span>
     \-\> To see information about the MP\-BGP neighbor<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 labeled\-unicast sum</span>
     \-\> To see information about the MP\-BGP neighbor
<span style="background-color: #ffaaaa">sh bgp ipv6 labeled\-unicast</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 u 2002:1:1:1::1</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 u 2002:6:6:6::6</span>
<span style="background-color: #ffaaaa">sh bgp ipv6 u labels</span>
<span style="background-color: #ffaaaa">sh route ipv6</span>
<span style="background-color: #ffaaaa">

</span>
<span style="background-color: #ffaaaa">

</span>
