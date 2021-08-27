# Multicast Distribution Tree (MDT) - Class Notes

**Multicast Distribution Tree \(MDT\)** \(19 Sept 2014\)

Lab: MDT 1, MDT BGP, MDT with XR

 \- a.k.a

     \-\> Multicast VPN \(MVPN\)

     \-\> Multicast VRF \(MVRF\)

 \- Allows clients to exchange multicast traffic over MPLS L3VPN

![20141013_084819-1.jpeg](image/20141013_084819-1.jpeg)

![20141013_084824-1.jpeg](image/20141013_084824-1.jpeg)

**Configuration**

 1. Configure multicast in the ISP

 2. Configure VRF multicast on PE routers

 3. Configure multicast in the customer networks

 4. MDT address will be configured on all PE routers

 5. MDT address will be used to form an MDT tunnel between PE routers

 6. Any multicast traffic \( control plane / data plane \) coming from the customer will be encapsulated in MDT packets and sent to other PE routers

 7. The receiving PE routers deencapsulate the MDT packets if the internal multicast packet is destined for local VRF

IOS Router

R1\(config\)\# <span style="background-color: #ffaaaa">ip multicast\-routing</span>

<span style="background-color: #ffaaaa"> ip multicast\-routing vrf ABC</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> ip pim sparse\-mode</span>

<span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> ip pim sparse\-mode</span>

<span style="background-color: #ffaaaa">ip pim rp\-address 2.2.2.2</span>

<span style="background-color: #ffaaaa">ip pim vrf ABC rp\-address 4.4.4.4</span>

<span style="background-color: #ffaaaa">ip vrf ABC</span>

<span style="background-color: #ffaaaa"> mdt default 239.1.1.1</span>

R4, R5, R6

 \- Basic multicast configuration

XR Router

R3\(config\)\# <span style="background-color: #ffaaaa">multicast\-routing</span>

<span style="background-color: #ffaaaa"> address\-family ipv4</span>

<span style="background-color: #ffaaaa">  int s0/0</span>

<span style="background-color: #ffaaaa">   enable</span>

<span style="background-color: #ffaaaa"> vrf ABC</span>

<span style="background-color: #ffaaaa">  address\-family ipv4</span>

<span style="background-color: #ffaaaa">   mdt default ipv4 239.1.1.1</span>

<span style="background-color: #ffaaaa">   int s0/1</span>

<span style="background-color: #ffaaaa">    enable</span>

<span style="background-color: #ffaaaa">   int s0/2</span>

<span style="background-color: #ffaaaa">    enable</span>

<span style="background-color: #ffaaaa">router pim</span>

<span style="background-color: #ffaaaa"> address\-family ipv4</span>

<span style="background-color: #ffaaaa">  rp\-address 2.2.2.2</span>

<span style="background-color: #ffaaaa">  int s0/0</span>

<span style="background-color: #ffaaaa">   enable</span>

<span style="background-color: #ffaaaa"> vrf ABC</span>

<span style="background-color: #ffaaaa"> address\-family 4.4.4.4</span>

<span style="background-color: #ffaaaa">  int s0/1</span>

<span style="background-color: #ffaaaa">   enable</span>

<span style="background-color: #ffaaaa">  int s0/2</span>

<span style="background-color: #ffaaaa">   enable</span>

![20141013_084841-1.jpeg](image/20141013_084841-1.jpeg)

**Inter\-AS Option A | B | C**

**MDT Tunnel**

 \- PE Address

     \-\> Not exchanged by default

          \-\> Solution is to send this information in BGP by using a special address\-family called "mdt"

 \- MDT Group

     \-\> Not exchanged by default

          \-\> Solution is to send this information in BGP by using a special address\-family called "mdt"

 \- ASBR routers add additional information to MDT updates called "Proxy RPF Vector"

 \- Proxy RPF Vector contains the intermediate next\-hop for RPF checks and join message propogation

 \- MSDP will be required once the PE address and MDT group of the other AS is known

     \-\> For this, the RP address for both ASs must be reachable from both ends

**Configuration**

 1. Configure MPLS L3VPN between ASs

 2. Activate multicast in both ASs separately

 3. Activate MDT on PE routers

 4. Configure BGP MDT address\-family on PE and ASBR routers

 5. Configure MSDP between RPs

 6. Activate multicast on customer network

IOS Router

R1\(config\)\# <span style="background-color: #ffaaaa">ip multicast\-routing</span>

<span style="background-color: #ffaaaa"> ip multicast\-routing vrf ABC</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> ip pim sparse\-mode</span>

<span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> ip pim sparse\-mode</span>

<span style="background-color: #ffaaaa">ip pim rp\-address 2.2.2.2</span>

<span style="background-color: #ffaaaa">ip pim vrf ABC rp\-address 5.5.5.5</span>

<span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa"> no bgp default ipv4\-unicast</span>

<span style="background-color: #ffaaaa"> neighbor 2.2.2.2 remotes\-as 100</span>

<span style="background-color: #ffaaaa"> neighbor 2.2.2.2 update\-source lo0</span>

<span style="background-color: #ffaaaa"> address\-family ipv4 mdt</span>

<span style="background-color: #ffaaaa">  neighbor 2.2.2.2 activate</span>

<span style="background-color: #ffaaaa">ip vrf ABC</span>

<span style="background-color: #ffaaaa"> mdt default 239.1.1.1</span>

XR Router

R2\(config\)\# <span style="background-color: #ffaaaa">router bgp 100</span>

<span style="background-color: #ffaaaa"> address\-family ipv4 mdt</span>

<span style="background-color: #ffaaaa"> neighbor 1.1.1.1</span>

<span style="background-color: #ffaaaa">  address\-family ipv4 mdt</span>

<span style="background-color: #ffaaaa">   next\-hop\-self</span>

<span style="background-color: #ffaaaa">  remote\-as 100</span>

<span style="background-color: #ffaaaa">  update\-source lo0</span>

<span style="background-color: #ffaaaa"> neighbor 23.0.0.2</span>

<span style="background-color: #ffaaaa">  address\-family ipv4 mdt</span>

**IOS Router**

<span style="background-color: #ffaaaa">sh bgp ipv4 mdt all</span>

<span style="background-color: #ffaaaa">sh ip pim mdt</span>

<span style="background-color: #ffaaaa">sh ip pim mdt bgp</span>

<span style="background-color: #ffaaaa">sh ip pim rp mapping</span>

<span style="background-color: #ffaaaa">sh ip pim vrf ABC rp mapping</span>

<span style="background-color: #ffaaaa">sh ip mspd summary</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">sh ip mroute</span>

<span style="background-color: #ffaaaa">sh ip mroute vrf ABC</span>

**XR Router**

<span style="background-color: #ffaaaa">sh bgp ipv4 mdt</span>

<span style="background-color: #ffaaaa">sh pim mdt</span>

<span style="background-color: #ffaaaa">sh pim range\-list</span>

<span style="background-color: #ffaaaa">sh msdp summary</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">sh pim vrf ABC rp mapping</span>

<span style="background-color: #ffaaaa">sh route ipv4 multicast</span>

<span style="background-color: #ffaaaa">sh route vrf ABC ipv4 multicast</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">sh mfib route</span>

<span style="background-color: #ffaaaa">sh mfib vrf ABC route</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">sh mfib mdt statistics</span>

<span style="background-color: #ffaaaa">sh mfib vrf ABC mdt statistics</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">

</span>

**MDT Tunnel Source Address**

 \- When MDT is configured between PE routers, they create an MDT tunnel

     \-\> The source address of the tunnel is the LDP router\-id;  or if BGP is configured, then the source address of the tunnel is the highest update\-source

     \-\> To configure the tunnel source manually

IOS Router

<span style="background-color: #ffaaaa">ip vrf ABC</span>

<span style="background-color: #ffaaaa"> mdt defautl 239.1.1.1</span>

<span style="background-color: #ffaaaa"> bgp next\-hop lo0</span>

XR Router

<span style="background-color: #ffaaaa">multicast\-routing</span>

<span style="background-color: #ffaaaa"> address\-family ipv4</span>

<span style="background-color: #ffaaaa">  mdt source lo0</span>

**MDT Data Groups**

 \- MDT default group is joined by all PE routers, so the traffic always goes to all PE routers

 \- If multicast traffic crosses a pre\-specified threshold, another tunnel can be created and joined only by interested PE routers

     \-\> This is called an MDT data group

![20141013_084853-1.jpeg](image/20141013_084853-1.jpeg)

Tunnel 239.1.1.1 \-\> Joined by PE 2, 3, 4, 5

TUnnel 238.1.1.1 \-\> Joined by PE 2, 3

Scenario \-\> MDT Default Group \- 239.1.1.1; MDT Data Group \- 238.1.1.0 \- 3

IOS Routers

 \- On all PE routers

<span style="background-color: #ffaaaa">ip vrf ABC</span>

<span style="background-color: #ffaaaa"> mdt default 239.1.1.1</span>

<span style="background-color: #ffaaaa"> mdt data 238.1.1.0 0.0.0.3 threshold 500</span>

XR Routers

 \- On all PE routers

<span style="background-color: #ffaaaa">vrf ABC</span>

<span style="background-color: #ffaaaa"> address\-family ipv4</span>

<span style="background-color: #ffaaaa">  mdt default ipv4 239.1.1.1</span>

<span style="background-color: #ffaaaa">  mdt data ipv4 238.1.1.0 0.0.0.3 threshold 500</span>
