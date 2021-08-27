# CCIE Service Provider Intro - Class Notes

**CCIE Service Provider** \(8 Sept 2014\) \- Version 3

 \- 8 hour exam

     \-\> Pure configuration\! \(almost\)

     \-\> 4 sections

          \-\> Section 1

               \- IGP

                    \-\> OSPF

                    \-\> IS\-IS

               \- BGP

               \- MPLS basic

               \- Basic multicast

          \-\> Section 2

               \- Smallest Edge Networking Level

                    \-\> IS\-IS

               \- Stub\-area OSPF

          \-\> Section 3

               \- MPLS L3VPN

                    \-\> VPNv4

                    \-\> VPNv6

                    \-\> Inter\-site PE \- CE

                    \-\> Inter\-site VPNv4

                    \-\> Inter\-site VPNv6

                    \-\> CSC

                    \-\> Advanced Multicast

          \-\> Section 4

               \- MPLS L2VPN

 \- Devices

     \-\> XR Routers \(4 routers\)

     \-\> 7200 / 7600 IOS Routes \(20 routers\)

          \-\> Emulation OS 12.2SR

     \-\> ME3400 switches \(3 switches\)

2 weeks of theory

 \- Phase 1

     \-\> IGPs

          \-\> RIP

          \-\> EIGRP

          \-\> OSPF

          \-\> IS\-IS

     \-\> BGP

     \-\> MPLS\-LDP

     \-\> MPLS\-TE

     \-\> Multicast

 \- Phase 2

     \-\> MPLS L3VPN

          \-\> IPv4

          \-\> IPv6

     \-\> MPLS Inter\-AS

          \-\> VPNv4

          \-\> VPNv6

     \-\> Carrier Support Carrier \(CSC\)

     \-\> Advanced Multicast

     \-\> L2VPN \- P2P VPN

          \-\> Interworking

          \-\> VPLS

Racks

 \- Pod 1     \- Jeff

 \- Pod 2     \- Brian

 \- Pod 3     \- John

 \- Pod 4     \- Lukeman

 \- Pod 5     \- Ted

 \- Pod 6     \- Ricky

 \- Pod 7     \- Jahn

 \- Pod 8     \- Daniel

First 2 weeks

 \- 8 routers only

     \-\> XR Routers

          \-\> R1

          \-\> R2

          \-\> R3

          \-\> R4

     \-\> IOS Routers

          \-\> R5

          \-\> R6

          \-\> R7

          \-\> R8

Cisco VPN Client

 \- Server IP:  171.99.171.117

24 Routers

     \-\> 4 Routers R1 \- R4 \-\> XR Routers

          \-\> One VMWare server

     \-\> 20 Routers R 5 \- R20

          \-\> GNS Server

Pod 1

 \- XR Routers

     \-\> 172.16.100.25: 9011 \(R1\)

     \-\> 172.16.100.25: 9012 \(R2\)

     \-\> 172.16.100.25: 9013 \(R3\)

     \-\> 172.16.100.25: 9014 \(R4\)

 \- IOS Routers

     \-\> 172.16.100.11: 2005 \(R5\)

     \-\> 172.16.100.11: 2006 \(R6\)

          . . . 

          . . .

     \-\> 172.16.100.11: 2024 \(R24\)

VPN Client

 \- 172.99.171.117

 \- Group name: isol1

 \- Password: isoler1

 \- Username: jeff

 \- Password: jeff

All links 

 \- IPv4

     \-\>9.9.xy.z

     \-\> xy \- Link for routers

     \-\> z \- router\-id

 \- IPv6

     \-\> 2002:9:9:xy::z /64

R1 \- R2

 \- 9.9.12.1

 \- 9.9.12.2

 \- 2002:9:9:12::1

 \- 2002:9:9:12::2

Loopback 

 \- 9.9.0.x

 \- 2002:9:9::x /128

Reseting routers to blank configs

 \- XR Routers \(R1 \- R4\)

     \-\> \(config\)\# commit replace

 \- IOS Routers \(R5 \- R20\)

     \-\> mstsc \(Remote Desktop\) into 172.16.100.13

     \-\> Password:  cciesp

     \-\> In GNS Server

          \-\> Just restart the routers

          \-\> Don't close GNS3
