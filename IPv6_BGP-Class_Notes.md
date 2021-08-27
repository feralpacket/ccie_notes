# IPv6 BGP - Class Notes

**IPv6 BGP** \(28 Aug 2014\)Lab:  IPv6 1 and 2

Forming neighbors

\- Enpoint IP addresses

     \-\> IPv4

     \-\> IPv6

Exchanging networks \(payload\)

\- Address families

     \-\> IPv4

     \-\> IPv6

     \-\> VPNv4

     \-\> VPNv6

     \-\> VRF

When other address families are activated, the IPv4 address family is added

![20141003_110543-1.jpeg](image/20141003_110543-1.jpeg)

Address Family Identifier \(AFI\)

 \- ipv4

 \- ipv6

 \- vpnv4

 \- vpnv6

Subsequent Address Family Identifier \(SAFI\)

 \- unicast

 \- multicast

 \- vrf

<span style="background-color: #ffaaaa">router bgp 100</span>

 <span style="background-color: #ffaaaa">address\-family \<afi\> \<safi\></span>

IPv4 is the default address family

 \- To disable

<span style="background-color: #ffaaaa">router bgp 100</span>

 <span style="background-color: #ffaaaa">no bgp default ipv4\-unicast</span>

 

<span style="background-color: #ffaaaa">router bgp 100</span>

 <span style="background-color: #ffaaaa">no bgp default ipv4\-unicast</span>

 n<span style="background-color: #ffaaaa">eighbor 2001::2 remotes\-as 200</span>

 <span style="background-color: #ffaaaa">address\-family ipv6 unicast</span>

  <span style="background-color: #ffaaaa">neighbor 2001::2 activate</span>

  <span style="background-color: #ffaaaa">network 1111::1/128</span>

<span style="background-color: #ffaaaa">sh bgp ipv6 unicast</span>
