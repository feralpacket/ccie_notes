# Introduction to XR Routers - Class Notes

**Introduction to XR Routers** \(8 Sept 2014\)

What's different

 \- IOS kernel is monolithic

     \-\> All processes share

\(config\)\# <span style="background-color: #ffaaaa">int g0/0/0/1</span>

 <span style="background-color: #ffaaaa">ipv4 address 10.0.0.1/24</span>

 <span style="background-color: #ffaaaa">no shut</span>

 <span style="background-color: #ffaaaa">root</span>

 <span style="background-color: #ffaaaa">sh config</span>

 <span style="background-color: #ffaaaa">commit</span>

<span style="background-color: #ffaaaa">sh config failed</span>

<span style="background-color: #ffaaaa">clear</span>

<span style="background-color: #ffaaaa">sh ipv4 int bri</span>

<span style="background-color: #ffaaaa">sh ipv6 int bri</span>

Routing Protocols

 \- Static

 \- RIPv2

 \- EIGRP

 \- OSPF

 \- IS\-IS

 \- BGP

**Static Routing**

\(config\)\# <span style="background-color: #ffaaaa">router static</span>

 <span style="background-color: #ffaaaa">address\-family ipv4 unicast</span>

  <span style="background-color: #ffaaaa">1.1.1.1/32 172.16.205.129</span>

 <span style="background-color: #ffaaaa">root</span>

 <span style="background-color: #ffaaaa">sh config</span>

 <span style="background-color: #ffaaaa">commit</span>

 <span style="background-color: #ffaaaa">end</span>

<span style="background-color: #ffaaaa">sh route ipv4</span>

<span style="background-color: #ffaaaa">sh route ipv6</span>

\(config\)\# <span style="background-color: #ffaaaa">router static</span>

 <span style="background-color: #ffaaaa">vrf ABC</span>

**RIPv2**

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> int lo0</span>

<span style="background-color: #ffaaaa"> int g0/0/0/0</span>

<span style="background-color: #ffaaaa"> timers basic</span>

<span style="background-color: #ffaaaa"> int g0/0/0/0</span>

<span style="background-color: #ffaaaa">  authentication keychain ABC mode md5</span>

<span style="background-color: #ffaaaa"> root / clear / commit</span>

<span style="background-color: #ffaaaa">end</span>

<span style="background-color: #ffaaaa">sh rip</span>

**EIGRP**

<span style="background-color: #ffaaaa">router eigrp { \<asn\> | \<name\> }</span>

<span style="background-color: #ffaaaa"> address\-familty ipv4</span>

<span style="background-color: #ffaaaa">  autonomous\-system \<asn\></span>

     \-\> in named mode

<span style="background-color: #ffaaaa">  int lo0</span>

<span style="background-color: #ffaaaa">  in g0/0/0/0</span>

<span style="background-color: #ffaaaa"> root</span>

<span style="background-color: #ffaaaa"> sh config</span>

<span style="background-color: #ffaaaa"> commit</span>

<span style="background-color: #ffaaaa">sh eigrp nei</span>

\(config\)\# <span style="background-color: #ffaaaa">router eigrp ABC</span>

<span style="background-color: #ffaaaa"> address\-family ipv4</span>

<span style="background-color: #ffaaaa">  int g0/0/0/0</span>

<span style="background-color: #ffaaaa">   hello\-interval 10</span>

<span style="background-color: #ffaaaa">   hold\-time 30</span>

<span style="background-color: #ffaaaa"> root</span>

<span style="background-color: #ffaaaa"> sh config</span>

<span style="background-color: #ffaaaa"> commit</span>

\(config\)\# <span style="background-color: #ffaaaa">ipv4 access\-list ACL1</span>

 p<span style="background-color: #ffaaaa">ermit ipv4 any any</span>

**OSPF**

 

\(config\)\# <span style="background-color: #ffaaaa">router ospf P1</span>

 <span style="background-color: #ffaaaa">area 0</span>

<span style="background-color: #ffaaaa">  int lo0</span>

<span style="background-color: #ffaaaa">  int g0/0/0/0</span>
