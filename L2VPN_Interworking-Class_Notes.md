# L2VPN Interworking - Class Notes

**L2VPN Interworking** \(20 Sept 2014\)

Lab: EoMPLS, PPP\_ETHERNET, PPP\_ETHERNET

 \- Mechanism to connect two different layer 2 protocols over a L2VPN network

![20141014_102112-1.jpeg](image/20141014_102112-1.jpeg)

R1\(config\)\# <span style="background-color: #ffaaaa">pweudowire\-class ABC</span>

<span style="background-color: #ffaaaa"> encapsulation mpls</span>

<span style="background-color: #ffaaaa"> interworking ip</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation ppp</span>

<span style="background-color: #ffaaaa"> xconnect 3.3.3.3 100 pw\-class ABC</span>

XR Router

<span style="background-color: #ffaaaa">l2vpn</span>

<span style="background-color: #ffaaaa"> pw\-class ABC</span>

<span style="background-color: #ffaaaa">  encapsulation mpls</span>

<span style="background-color: #ffaaaa">  interworking ip</span>

<span style="background-color: #ffaaaa"> xconnect group GROUP1</span>

<span style="background-color: #ffaaaa">  p2p R1\-to\-R3</span>

<span style="background-color: #ffaaaa">   int g0/0/0/0</span>

<span style="background-color: #ffaaaa">    neighbor 3.3.3.3 pw\-id 100 pw\-class ABC</span>

Frame\-relay special scenario

![20141014_102124-1.jpeg](image/20141014_102124-1.jpeg)

R2\(config\)\# <span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encapsulation frame\-relay</span>

<span style="background-color: #ffaaaa">do sh frame\-relay pvc</span>

<span style="background-color: #ffaaaa">     \-\> To get the DLCI information</span>

<span style="background-color: #ffaaaa"> frame\-relay interface\-dlci 200 switched</span>

<span style="background-color: #ffaaaa">connect fr x0/0 200 l2transport</span>

<span style="background-color: #ffaaaa"> xconnect 1.1.1.1 100 encapsulation mpls</span>

During the lab, you will only be asked to configure VPLS on the XR routers

 \- All other L2VPN configuration will be done on the IOS routers

     \-\> Virtual 7200 for IOS doesn't support VPLS

<span style="background-color: #ffaaaa">sh mpls l2transport vc \<id\></span> \(IOS\)

<span style="background-color: #ffaaaa">sh l2vpn xconnect \[detail\]</span> \(XR\)

![20141014_102137-1.jpeg](image/20141014_102137-1.jpeg)
