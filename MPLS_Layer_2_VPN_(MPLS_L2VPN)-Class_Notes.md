# MPLS Layer 2 VPN (MPLS L2VPN) - Class Notes

**MPLS Layer 2 VPN \(MPLS L2VPN\)** \(20 Sept 2014\)Lab: EoMPLS, PPP\_ETHERNET, PPP\_ETHERNET

\- Point\-to\-point

     \-\> Any Transport over MPLS \(AToM\)

     \-\> L2TPv3

 \- Point\-to\-multipoint

     \-\> Virtual Private LAN Service \(VPLS\)

 \- Used to transport layer 2 payloads transparently

     \-\> HDLC

     \-\> PPP

     \-\> ATM

     \-\> Frame\-relay

     \-\> Ethernet

L2VPN Label

 \- Label exchange between PE routers

Transport Label

 \- Label to reach next\-hop

Targeted LDP

 \- Multihop LDP session

     \-\> Uses LDP router\-ids to form TCP connection

 \- Pseudowire\-id

     \-\> 32 bit number which must match on both PE routers for a single connection

<span style="background-color: #ffaaaa">int fa0/0</span>

     \-\> Attachment circuit

<span style="background-color: #ffaaaa"> xconnect \<remote LDP router\-id\> \<pseudowire\-id\> encapsulation mpls</span>

**Ethernet Configuration**

R1\(config\)\# <span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> xconnect 3.3.3.3 100 encapsulation mpls</span>

R3\(config\)\# <span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> xconnect 1.1.1.1 100 encapsulation mpls</span>

sh mpls l2transport vc \<id\> \[detail\]

**PPP Configuration**

R1\(config\)\# <span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> encap ppp</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> xconnect 3.3.3.3 100 encapsulation mpls</span>

R3\(config\)\# <span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> encap ppp</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> xconnect 1.1.1.1 100 encapsulation mpls</span>

**HDLC Configuration**

R1\(config\)\# <span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> encap hdlc</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> xconnect 3.3.3.3 encapsulation mpls</span>

R3\(config\)\# <span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> encap hdlc</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> xconnect 1.1.1.1 encapsultation mpls</span>

**FRoAToM**

 \- Frame\-relay over AToM

R1\(config\)\# <span style="background-color: #ffaaaa">int s0/1</span>

<span style="background-color: #ffaaaa"> no ip add</span>

<span style="background-color: #ffaaaa"> encap frame\-relay</span>

<span style="background-color: #ffaaaa"> frame\-relay intf\-type dce</span>

<span style="background-color: #ffaaaa"> exit</span>

<span style="background-color: #ffaaaa">int s0/1 100 l2transport</span>

     \-\> 100 = DLCI

<span style="background-color: #ffaaaa"> xconnect 3.3.3.3 200 encapsulation</span>

     \-\> 200 = pseudowire\-id

R4\(config\)\# <span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> encap frame\-relay</span>

<span style="background-color: #ffaaaa"> ip add 10.0.0.4 255.0.0.0</span>

<span style="background-color: #ffaaaa"> frame\-relay ip 10.0.0.5 100 broadcast</span>

     \-\> 100 = DLCI

     \-\> broadcast \(optional\)

**Pseudowire Class**

 \- Set of AToM / L2TPv3 specific parameters which are applied to xconnect

<span style="background-color: #ffaaaa">pseudowire\-class \<name\></span>

<span style="background-color: #ffaaaa"> encapsulation { mpls | l2tpv3 }</span>

<span style="background-color: #ffaaaa"> control\-word</span>

     \-\> Keeps properties of client and sent to the other end

     \-\> Similar to extended communities in BGP

     \-\> Used by some layer 2 protocols

          \-\> Frame\-relay

               \- FECN

               \- BECN

               \-DE\-bits

          \-\> ATM

               \- Cell loss priority \(CLP\)

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> xconnect \<ip add\> \<pw\-id\> pw\-class \<name\></span>
