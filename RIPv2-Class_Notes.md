# RIPv2 - Class Notes

**\[RIPv2** \(20 Aug 2014\)

 \- UDP port 520

 \- Multicast IP 224.0.0.9

 \- Classless \(not by default\)

     \-\> Cannot have discontiguous networks

 \- Metric is hop count

 \- Metric 16 is infinite metric

 \- Timers

     \-\> Update timer:  30 seconds

     \-\> Invalid timer:  180 seconds

     \-\> Holddown timer:  180 seconds

     \-\> Flush timer:  240 seconds

 \- Supports authentication

     \-\> Plain text

     \-\> MD5

 \- AD is 120

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> version 2</span>

<span style="background-color: #ffaaaa"> no auto\-summary</span>

<span style="background-color: #ffaaaa"> network x.x.x.x</span>

![20141114_174326-1.jpeg](image/20141114_174326-1.jpeg)

Mismatched version updates can cause one way communication between the routers

 \\\- R1

     \-\> Send v1 updates

     \-\> Receive v1 and v2 updates

 \- R2

     \-\> Send and receive v2 updates

**Authentication**

 \- Plain text

 \- MD5

     \-\> Create key chain

     \-\> Apply to an interface

<span style="background-color: #ffaaaa">key chain CISCO</span>

<span style="background-color: #ffaaaa"> key 1</span>

<span style="background-color: #ffaaaa">  key\-string CCIE</span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> ip rip authentication mode { text | md5 }</span>

<span style="background-color: #ffaaaa"> ip rip authentication key\-chain CISCO</span>

<span style="background-color: #ffaaaa">sh ip protocols</span>

 \- Lists the authentication method

<span style="background-color: #ffaaaa">debug ip rip</span>

 \- To troubleshoot authentication problems

     \-\> Such as key mismatch

<span style="background-color: #ffaaaa">sh key chain</span>

 \- To see if a space is present in the key\-string

**Summarization**

![20141114_174332-1.jpeg](image/20141114_174332-1.jpeg)

Summary address 10.0.0.0/22

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip rip summary\-address 10.0.0.0 255.255.252.0</span>

A summary address is installed into the routing table pointing to the NULL interface

 \- Used when one of the more specific networks is not rachable

 \- This prevents the packets from being default routed to 0.0.0.0/0

 \- Causes packets to be dropped

 \- Summary address AS is still 120

RIP does not create a summary route pointing to NULL0

 \- Must be created manually

<span style="background-color: #ffaaaa">ip route 10.0.0.0 255.255.252.0 NULL0</span>

172.16.0.0 /24

172.16.1.0 /24

172.16.2.0 /24

172.16.3.0 /24

     \-\> 172.16.0.0 /22

          \-\> Works with RIPv2

172.0.0.0 /16

172.1.0.0 /16

172.2.0.0 /16

172.3.0.0 /16

     \-\> 172.0.0.0 /14

          \-\> Will not work with RIPv2

          \-\> Error message when trying to configure

RIPv2 summarization is only possible within the limits of a class \(A, B, C\)

     \-\> RIPv2 summarization is not classles

**Default Routing**

![20141114_174343-1.jpeg](image/20141114_174343-1.jpeg)

R1\(config\)\# <span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> default\-information originate</span>

In other routing tables:

R\*  0.0.0.0     \[120|\*\]

     \-\> The metric of the summary route is the least metric among more specific routes

**Conditional Default Routing**

![20141114_174353-1.jpeg](image/20141114_174353-1.jpeg)

In conditional default routing, the exit interface network is checked

 \- If the network is in the routing table, the default network will be injected

 \- If the network is not in the routing table, the default route is not injected

R1\(config\)\# <span style="background-color: #ffaaaa">access\-list 1 permit 200.0.0.0 0.0.0.255</span>

<span style="background-color: #ffaaaa">route\-map DR</span>

<span style="background-color: #ffaaaa"> match ip address 1</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> default\-information originate route\-map DR</span>

This is conditional, but not reliable

 \- Have to make the route\-map false to be reliable

 \- Link may be up, but the connection to the Internet may be down

Reliable Conditional Default Routing

 \- Uses IP SLA

Apply to RIP

 \- IP SLA \(Step 1\) \<\-\-\> Track \(Step 2\) \<\-\- Dummy Static Route \(Step 3\) \<\-\- Access\-list \(Step 4\) \<\-\- Route\-map \(Step 5\)

R1\(config\)\# <span style="background-color: #ffaaaa">ip sla 1</span>

<span style="background-color: #ffaaaa"> icmp\-echo 4.2.2.2</span>

<span style="background-color: #ffaaaa">  timeout 2000</span>

     \-\> In milliseconds

  <span style="background-color: #ffaaaa">frequency 4</span>

     \-\> In seconds

<span style="background-color: #ffaaaa">ip sla schedule 1 start\-time now life forever</span>

<span style="background-color: #ffaaaa">track 1 ip sla 1 reachability</span>

<span style="background-color: #ffaaaa">ip route 169.254.0.0 255.255.0.0 NULL0 track 1</span>

<span style="background-color: #ffaaaa">access\-list 1 permit 169.254.0.0 0.0.255.255</span>

<span style="background-color: #ffaaaa">route\-map ABC</span>

<span style="background-color: #ffaaaa"> match ip address 1</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> default\-information originate route\-map ABC</span>

**RIP Filtering**

![20141114_174402-1.jpeg](image/20141114_174402-1.jpeg)

**Passive Interface**

 \- It stops sending updates out the specified interface

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> passive\-interface { \<interface\> | default }</span>

<span style="background-color: #ffaaaa">passive\-interface default</span>

 \- Can be used if there are a lot of loopback interfaces that you do not want to advertise

**Distribute List**

 \- Which network to filter

 \- Direction \( in | out \)

 \- In | out which interface

     \-\> If not specified, the network will be filtered from all interfaces

 \- Filter is outsourced\!

     \-\> ACL

     \-\> Prefix\-list

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distribute\-list \<acl\> in | out int \<int\></span>

<span style="background-color: #ffaaaa">

</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distribute\-list prefix \<list\> in | out int \<int\></span>

![20141114_174409-1.jpeg](image/20141114_174409-1.jpeg)

**Distribute List \- Standard ACL**

<span style="background-color: #ffaaaa">access\-list 1 deny 1.1.1.1</span>

<span style="background-color: #ffaaaa">access\-list 1 permit any</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distribute\-list 1 out fa0/0</span>

![20141114_174413-1.jpeg](image/20141114_174413-1.jpeg)

Scenario \-\> Filter all even number octets \(in the 3rd octet\) of outgoing network

255.255.11111110.255

     \-\> I don't care about the first 7 bits, I only care about the last bit

     \-\> Subnet mask:  255.255.254.255

     \-\> Wild card mask:  0.0.1.0

10.0.0.0 \- 00000000

10.0.2.0 \- 00000010

     \-\> Last bit doesn't change

     \-\> 0 \- don't care

     \-\> 1 \- do care

<span style="background-color: #ffaaaa">access\-list 1 deny 0.0.0.0 255.255.254.255</span>

<span style="background-color: #ffaaaa">access\-list 1 permit any</span>

     \- or \-

<span style="background-color: #ffaaaa">access\-list 1 permit 0.0.1.0 255.255.254.255</span>

**Distribute List \- Extended ACL**

 

<span style="background-color: #ffaaaa">access\-list \<number\> permit | deny \<protocol\> \<source\> \<destination\></span>

 \- Protocol is ip

 \- Source is update source

 \- Destination is update network

![20141114_174426-1.jpeg](image/20141114_174426-1.jpeg)

Scenario \-\> On R1, filter incoming update for network 50.0.0.0 if it is coming from R3

R1\(config\)\# <span style="background-color: #ffaaaa">access\-list 100 deny ip host 123.0.0.3 host 50.0.0.0</span>

<span style="background-color: #ffaaaa">access\-list 100 permit ip any any</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distribute\-list 100 in</span>

**Prefix Lists**

 \- More flexible

 \- Can match on subnet masks

i<span style="background-color: #ffaaaa">p prefix\-list \<name\> \[seq \<number\] permit | deny \<network/wildcard mask\> \[le | ge \<0 \- 32\>\]</span>

 \- \<network/wildcard mask\> \- prefix

 \- le | ge \<0 \- 32\> \- subnet mask

<span style="background-color: #ffaaaa">access\-list \<number\> permit | deny \<network\> \<wildcard mask\></span>

10.0.0.0 0.255.255.255

10.0.0.0 /22

10.0.0.0 /24

<span style="background-color: #ffaaaa">ip prefix\-list LIST1 deny 10.0.0.0/8 ge 24 le 24</span>

 \- Matches 10.0.0.0 /24

Match any network starting with 172.16.x.x with subnet mask from 255.255.0.0 to 255.255.255.0

255.255.0.0          \-\> /16

255.255.128.0      \-\> /17

255.255.192.0      \-\> /18

255.255.224.0      \-\> /19

255.255.240.0      \-\> /20

255.255.248.0      \-\> /21

255.255.252.0      \-\> /22

255.255.254.0      \-\> /23

255.255.255.0      \-\> /24

<span style="background-color: #ffaaaa">ip prefix\-list ABC deny 172.16.0.0/16 ge 16 le 24</span>

![20141114_174436-1.jpeg](image/20141114_174436-1.jpeg)

1. 

 \- Wildcard \-\>16

 \- Range \-\> 16 \- 24

2.

 \- Wildcard \-\> 16

 \- Range \-\> 18 \- 32

     \-\> If on the exam, upper boundary is not specific, assume it's 32

3.

 \- Wildcard \-\> 16

 \- Range \-\> 18 \- 24

<span style="background-color: #ffaaaa">ip prefix\-list ABC deny 172.16.0.0/16 ge 18</span>

<span style="background-color: #ffaaaa">ip prefix\-list ABC deny 172.16.0.0/16 ge 18 le 24</span>

Drawback of using prefix\-lists

 \- ge cannot be lower than the wildcard mask

Scenario \-\> Match any network starting with 172.16.0.0 and subnet mask between 8 and 24

<span style="background-color: #ffaaaa">ip prefix\-list ABC deny 172.16.0.0/16 ge 8 le 24</span>

     \-\> **Will not work**

     \-\> ge cannot be lower than the wildcard mask

If a single subnet mask is to be matched and it happens to be equal to the wildcard mask, then ge and le can be skipped

 \- Compare first octet of 10.0.0.0 and subnet mask must be 255.0.0.0

<span style="background-color: #ffaaaa">ip prefix\-list permit 10.0.0.0/8</span>

![20141114_174443-1.jpeg](image/20141114_174443-1.jpeg)

Filter 1.1.1.1 /32 from R2

R1\(config\)\# <span style="background-color: #ffaaaa">ip prefix\-list ABC deny 1.1.1.1/32</span>

<span style="background-color: #ffaaaa"> ip prefix\-list ABC permit 0.0.0.0/0 le 32</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distribute\-list prefix ABC out fa0/0</span>

**Filter from a specific source**

![20141114_174452-1.jpeg](image/20141114_174452-1.jpeg)

Scenario \-\> Filter any network coming from R3 and accept all networks from R2

 \- In this scenario, two prefix lists will be used

     \-\> 1. To identify which networks will be filtered

     \-\> 2. To identify the source

**Distribute List**

<span style="background-color: #ffaaaa">distribute\-list prefix \<list1\> gateway \<list2\> in | out \[\<interface\>\]</span>

R1\(config\)\# <span style="background-color: #ffaaaa">ip prefix\-list LIST1 permit 50.0.0.0/8</span>

<span style="background-color: #ffaaaa"> ip prefix\-list LIST2 deny 123.0.0.3/32</span>

<span style="background-color: #ffaaaa"> ip prefix\-list LIST2 permit 123.0.0.2/32</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distribute\-list prefix LIST1 gateway LIST2 in fa0/0</span>

During the lab, always use extended ACLs unless using prefix\-lists is spcified

<span style="background-color: #ffaaaa">sh ip protocols</span>

 \- Displays the distribute\-lists applied

**Offset\-list**

 \- This is used to add an offset number to the metric value when updates are sent or received

![20141114_174500-1.jpeg](image/20141114_174500-1.jpeg)

Scenario \-\> R1 should always use R2 as next\-hop to reach network x \(50.0.0.0\)

 \- If connection to R2 goes down, R1 should start using R3 as next\-hop for network x

<span style="background-color: #ffaaaa">offset\-list \<acl\> in | out \<offset\-number\> \[\<interface\>\]</span>

 \- Standard ACL is used

<span style="background-color: #ffaaaa">access\-list 1 permit 50.0.0.0</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> offset\-list 1 in 2 s0/1</span>

For filtering purposes, offset number 16 can be used

Scenario \-\> Filter all network from R3

<span style="background-color: #ffaaaa">access\-list 1 permit any</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> offset 1 in 16 s0/1</span>

     \- or \-

\! No ACL needed

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> offset\-list 0 in 16 s0/1</span>

**Filtering By Manipulating AD**

 \- AD 255

     \-\> Unreachable

     \-\> Route deleted from the routing table

Scenario \-\> Filter network 50.0.0.0 from any router

<span style="background-color: #ffaaaa">access\-list 1 permit 50.0.0.0</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distance 255 0.0.0.0 255.255.255.255 1</span>

     \-\> 0.0.0.0 255.255.255.255 \- the source

     \-\> 1 \- ACL

Scenario \-\> Filter 50.0.0.0 from R3 \(123.0.0.3\)

<span style="background-color: #ffaaaa">access\-list 1 permit 50.0.0.0</span>

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> distance 255 123.0.0.3 0.0.0.0 1</span>

**RIP Miscellaneous Topics**

 \- Change timers

 \- Unicast updates

 \- Triggered updates

 \- Send / receive version

**Changing Timers**

 \- rip configation

 \- inside interface

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> timers basic \<update\> \<invalid\> \<holddown\> \<flush\></span>

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip rip advertise \<sec\></span>

![20141114_174512-1.jpeg](image/20141114_174512-1.jpeg)

Scenario \-\> Change the RIP timers to 20, 90, 90, 120, but keep the update time to 30 seconds for fa0/0

R1\(config\)\# <span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> timers basic 20 90 90 120</span>

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip rip advertise 30</span>

**Unicast Updates**

![20141114_174518-1.jpeg](image/20141114_174518-1.jpeg)

To switch to unicast updates

 \- Stop sending multicast updates

 \- Start sending unicast updates

On R1 / R2:

<span style="background-color: #ffaaaa">router rip</span>

<span style="background-color: #ffaaaa"> passive\-interface fa0/0</span>

<span style="background-color: #ffaaaa"> neighbor 12.0.0.x</span>

**Triggered Updates**

 \- Any serial \(point\-to\-point\) interface the periodic updates can be disabled and made triggered

![20141114_174525-1.jpeg](image/20141114_174525-1.jpeg)

On R1 / R2:

<span style="background-color: #ffaaaa">int s0/0</span>

<span style="background-color: #ffaaaa"> ip rip triggered</span>

<span style="background-color: #ffaaaa">debug ip rip</span>

<span style="background-color: #ffaaaa">sh ip protocols</span>

Send | Receive Version

 \- By default if the version command is not used, then all interfaces

     \-\> Send v1 updates

     \-\> Receive v1 and v2 updates

 \- If the version command is used, the interfaces will send and receive the version specified

 \- The impact of the version command can be overridden by using interface specific commands

<span style="background-color: #ffaaaa">int fa0/0</span>

<span style="background-color: #ffaaaa"> ip rip send version { 1 | 2 }</span>

<span style="background-color: #ffaaaa"> ip rip receive version { 1 | 2 }</span>

<span style="background-color: #ffaaaa">sh ip protocols</span>

router rip will not display in show run if a network statement is not configured

ip rip advertise 30 will not show up in the sh run config because it is the default configuration

 \- Use sh ip route to verify the proper networks are received on the interface
