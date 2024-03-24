```
# CCIE Lab Recommendations

**CCIE Lab Recommendations**

**The test is a reading comprehension test.**

**Troubleshooting Section:**

 - "There is no troubleshooting section.  There is a misconfiguration section." - Narbik

      -> "There is no spoon." - The Matrix

 - "Troubleshoot as if you were configuring the protocol." - Narbik

    -> Review Narbik's steps for configuring protocols (repeatedly)

    -> Go through the Cisco360 Student Guide flow charts for configuring protocols (repeatedly)

    -> Go through the Cisco360 Student Guide charts on troubleshooting pre-existing configurations (repeatedly)

    -> Some protocols are easy to configure, but can be broken in many different ways

       -> MPLS

       -> L3VPN
```
Â - You donâ€™t troubleshoot 30 devices at the same time

Â  Â  Â -> You only have to troubleshoot 4 to 5 devices for any given ticket

Â - In the Cisco360 Troubleshooting assessments, the routers tended to be configured with â€œ**no logging console**.â€ Â This is to mask the symptoms and log messages that would help you troubleshoot.

Â  Â  Â -> First thing to do when connecting to a new device, configure â€œ**logging console**."

Â - Read all of the tickets before trying to fix any of them

Â  Â  Â -> From the Cisco360 Advanced Workshop 1 Student Guide:

Â  Â  Â  Â  Â  -> â€œSome tickets depend on other tickets. Â Most do not."

Â  Â  Â -> Look for tickets that are interrelated or overlap

Â  Â  Â  Â  Â  -> Core network

Â  Â  Â  Â  Â  Â  Â  Â -> Switching

Â  Â  Â  Â  Â  Â  Â  Â -> DMVPN

Â  Â  Â  Â  Â  -> Redistribution

Â  Â  Â -> Look for devices that are involved in multiple tickets

Â  Â  Â  Â  Â  -> May rule out resolution options

Â  Â  Â  Â  Â  -> A solution for one ticket may break the solution for another ticket

**[JP Cedeno, #47408](https://routergods.slack.com/team/jpcedeno47408)**Â 

95% of the TS section you are going to be asked to configure... SOOOO if you struggle with DMVPN, take the working config from TS and copy it into notepad.Â 

you can use that template for your config sectionÂ 

you can save that notepad file on the desktop during the exam

**Diagnostic Section:**

Â - "Something to do while your lab environment is changed from troubleshooting to configuration.â€ - donâ€™t rememberâ€¦.

Â -Â â€œDiagnostics, this is the worst thing Cisco has ever done. Â Itâ€™s for kids. Â Itâ€™s a mini-written in the lab. Â If you dump the lab, youâ€™ll have problems with diagnostics." - Narbik

Â - The challenge is being able to go through and process a large amount of show commands and being able to spot any errors and problems.

Â - Look at the answers to help led you in the right direction. - Some post on IOEC

**Configuration Section:**

**Read the entire lab:**

**Â - The test is a reading comprehension test.**

Â  Â  Â -> The Cisco360 assessment labs do a good job at pointing this out

Â -Â If you do the test in the order as written, you will fail.

Â  Â  Â -> Youâ€™ll run out of time when you have to go back of reconfigured devices

Â  Â  Â  Â  Â  -> Potentially introducing configuration errors

Â - Look for:

Â  Â  Â -> Tasks needed to reach Golden Moment

Â  Â  Â -> Interdependencies between tasks

Â  Â  Â  Â  Â  -> Authentication required for IGPs, but mentioned with security tasks and not when the IGP is configured

Â  Â  Â  Â  Â  -> Authentication method may force named mode configuration of an IGP, but IGP configurationÂ method is not mentioned

Â  Â  Â  Â  Â  -> EIGRP hmac-sha-256

Â  Â  Â -> Layer 2 configuration tasks later in the lab

Â  Â  Â -> â€œUse ACL 1â€¦.â€ later in the lab, when you already used ACL 1 for something else

Â  Â  Â -> Restrictions about removing certain configurations

Â  Â  Â  Â  Â  -> â€œDo not remove access-list 111â€¦."

Â - Pay very close attention to the following words:

Â  Â  Â -> They rule out potential configuration options

Â Â Â Â Â Â  Â Â  -> **not**

Â Â Â Â Â Â  Â Â  -> **only**

Â Â Â Â Â Â  Â Â  -> **without**

Â - Lab is based on Ciscoâ€™s internal version of IOU, not the one you can find on the Internet

**Things to look for in the lab diagram:**

Â - Loops

Â - Stubs

Â - Count the VLANs

Â - Look for important routers

Â  Â  Â -> OSPF ABR

Â  Â  Â -> OSPF ASBR

Â  Â  Â -> BGP RR

Â  Â  Â -> extra loopbacks

Â - Look for important switches

Â - Interfaces in two protocols

Â - Discontiguous OSPF area 0

Â  Â  Â -> Virtual links

Â - Multicast across DMVPN

Â - Redistribution points

Â  Â  Â -> Watch out for mutual redistribution

Â  Â  Â  Â  Â  -> Use Narbikâ€™s method of tagging routes with the admin distance of the originating routing protocol:

**route-map RIPtoEIGRP deny 10**

**Â match tag 90**

**route-map RIPtoEIGRP permit 20**

**Â set tag 120**

Â  Â  Â -> If route is tagged with 90, originated from EIGRP, deny so they will not be redistributed back into EIGRP

Â  Â  Â -> RIP originated routes will be tagged with 120

**route-map EIGRPtoRIP deny 10**

**Â match tag 120**

**route-map EIGRPtoRIP**

**Â set tag 90**

Â  Â  Â -> If route is tagged with 120, originated from RIP, deny so they will not be redistributed back into RIP

Â  Â  Â -> EIGRP originated routes will be tagged with 90

**router rip**

**Â redistribute eigrp 1 route-map EIGRPtoRIP metric 1**

**router eigrp 1**

**Â redistribute rip route-map RIPtoEIGRP metric 100000 1 255 1 1500**

Â  Â  Â -> Donâ€™t use a metric of â€œ1 1 1 1 1â€ with wide metrics

Â  Â  Â  Â  Â  -> The resulting composite metric will be too large and will not be advertised

**Make copies of default configurations**

**Â  -Â** When logging into any device for the first time

Â - Note:Â  IOL images may not have a flash:, copy to nvram: or unix:

Â  Â  -> **copy run nvram:run.orig**

Â - Remember to go back through and delete the copies

Â  Â  -> **delete nvram:run.orig**

**Existing pre-configuration may not be correct**

Â - â€œTrust, but verify.â€ - Reagan

Â - Verify connectivity between devices

Â  Â  Â ->Â **ping 255.255.255.255 rep 2**

Â - Be very careful when defaulting an interface (default int s1/0) Â or removing a router section configuration (no router rip).

Â  Â  -> Check to see if there is any pre-exisiting configuration.

**Drawing Diagrams**

Â - Only create your own layer 2 diagram when there are more than 2 switches

Â  Â  Â -> Useful for visualizing spanning-tree tasks

Â  Â  Â  Â  Â  -> Root bridges

Â  Â  Â  Â  Â  -> Ports in blocking state

Â - There is not enough time to draw the entire topology

Â - Use Cisco360â€™s second method (not the colored pencil method)

Â  Â  Â -> Access ports

Â  Â  Â  Â  Â  -> A + VLAN number

Â  Â  Â  Â  Â  -> â€œA20"

Â  Â  Â -> Trunk ports

Â  Â  Â  Â  Â  -> â€œT"

Â  Â  Â  Â  Â  -> Allowed VLANs, write next to the â€œT"

Â Â Â Â Â Â Â Â Â  -> "T 10, 20, 30"

Â  Â  Â -> Routed ports

Â  Â  Â  Â  Â  -> â€œR"

Â  Â  Â -> Port-channels

Â  Â  Â  Â  Â  -> Circle or oval over the links

Â  Â  Â -> SVI

Â  Â  Â  Â  Â  -> Write inside the box that represents the switch

Â  Â  Â -> Disabled ports

Â  Â  Â  Â  Â  -> â€œX"
![Screen_Shot_2015-08-30_at_1-17-38_PM.png](image/Screen_Shot_2015-08-30_at_1-17-38_PM.png)

**Watch for land mind questions**

Â - Easy 2 point questionsÂ that can break 20 points from somewhere else

Â - Donâ€™t chase the rabbit down the rabbit hole

**Never touch layer 2 in the last 30 minutes of the lab**

Â - Read the test and identify all layer 2 items

**Itâ€™s possible to pass all of the sections and still fail**

Â - Because you donâ€™t make the cut score

Â - It is possible now to fail a section (just barely)Â and still pass (if you did awesome in another section).

- From Narbik:

Â Â Â Â  -> You may pass each section individual.Â  Each sectionÂ  may have cut off scores such as:

Â Â Â Â Â Â Â Â Â  -> Troubleshooting:Â  70

Â Â Â Â Â Â Â Â Â  -> Diagnostics:Â  50

Â Â Â Â Â Â Â Â Â  -> Configuration:Â  70

Â Â Â Â  -> But if you don't meet or exceed the overall cutoff score, say 80, you'll fail the lab.

Â Â Â Â  -> Itâ€™s unknown what the cutoff score is for each section, how the sections are weighted, or what the overall cutoff score is.

~~- From another source ( have heard from several sources this is not true ):~~

~~Â  Â  Â -> You will be graded against all of the other people taking the test that day~~

~~Â  Â  Â -> 10 people may pass, but only 8 are allowed to pass for the day.~~

**Save text files to the desktop.**

**Cisco360â€™s Golden Moment:**

Â - â€œAll IPv4 and IPv6 addresses are reachable from all devices."

Â - Per link configuration

Â - Per IGP configuration

Â - Inter-IGP configuration (redistribution)

Â - BGP (sometimes)

Â - It is recommend by Cisco360 to skip any filtering and summarization to reach the Golden Moment

Â  Â  Â -> Me: not sure I want to do thisÂ 

Â - tclsh

Â  Â  Â -> **WARNING!!**

Â  Â  Â  Â  Â  -> The tcl script had a habit of locking up the switches during the Cisco360 assessment labs

Â  Â  Â  Â  Â  -> Make sure to save the configuration of all devices before using the tcl script

Â  Â  Â  Â  Â  Â  Â  Â -> If you havenâ€™t been configuring with notepadâ€¦.

Â  Â  Â  Â  Â  -> If the device locks up, you can reload the device and not have to reconfigure the device from scratch. Â Just paste the configuration back into the device.

Â  Â  Â  Â  Â  -> **Donâ€™t use it.**

# tchsh

Â foreach address {

Â 12.1.1.1

Â 12.1.1.2

} {ping $address}

Â  Â  Â -> Instead, list â€œping <ip address>â€ separately on each line.

ping 12.1.1.1

ping 12.1.1.2

Â . . . .

**Cisco360â€™s Configuration Order**

Â - Data Link Layer

Â - IGPs

Â - Redistribution

Â - BGP

Â  Â  Â -> Should reach Golden Moment

Â - VPN Technologies

Â - Multicast

Â - QoS

Â - IP Services

Â - Cisco IOS Software features

Â  Â  Â -> Exception, if there are VRFs, configure those first

Â  Â  Â  Â  Â  -> Should be MPLS, VRF, MP-BGP

Â Â Â Â Â Â Â Â Â  -> L2 may need to be configured before VRF / MPLS / MP-BGP connectivity is possible.

Â  Â  Â  Â  Â  -> The fact that you may be doing a CE-PE protocol configuration will probably not be pointed out by the tasks

Â  Â  Â  Â  Â  Â  Â  Â -> "Configure IGP between routersâ€¦.."

Â  Â  Â  Â  Â  Â  Â  Â -> If you follow the lab tasks in order, youâ€™ll end up having to reconfigure the PE routers for VRF later (waste time, potentially introduce configuration errors)

Â  Â  Â  Â  Â  -> â€œConfigure the core first, whether it's MPLS or DMVPN.Â  Then attach each site to that core.â€ - Narbik

Â - So, modified configuration order:

Â  Â  Â -> Layer 2

Â  Â  Â -> DMVPN (core)

Â  Â  Â -> MPLS (core, especially if there are VRFs)

Â  Â  Â -> IGPs

Â  Â  Â -> Redistribution

Â  Â  Â -> BGP

Â  Â  Â -> VPN Technologies ( whatever tasks are left for DMVPN and MPLS / L3VPN )

Â  Â  Â -> QoS

Â  Â  Â -> IP Services

Â  Â  Â -> Cisco IOS Software features

**DOC-CD:**

Â - â€œIf you have to go to the DOC-CD more than once, you are going to run out of time.â€ - Narbik

Â - My mind map of lab exam topics to DOC-CD locations

Â  Â  Â [http://feralpacket.org/?p=286](http://feralpacket.org/?p=286)

**Access-list, route-map, prefix-list naming convention:**

Â - Use task numbers if possible

Â  Â  Â -> Helps avoid numbering conflicts

Â  Â  Â -> Helps you remember or lookup why it was configured in the first place

Â  Â  Â access-list 21 permit host 1.1.1.1

Â - Be careful when using punctuation in names

**Â  Â  Â -> In fact, do not use any punctuation**

**Â  Â  Â ->** **This is a restriction that is listed in the lab ( mentioned by many people )**

Â  Â  Â route-map **TASK2.1** permit 10

Â  Â  Â  Â  Â  . . .

Â  Â  Â router bgp 100

Â  Â  Â  Â  Â  nei 12.1.1.2 route-map **TASK2,1** in

Â  Â  Â  Â  Â  Â  Â  Â -> Youâ€™d have to run debug to catch it if you cannot spot the typo (comma vs. period)

Â - Otherwise, use something meaningful

Â  Â  Â -> List the protocol and the direction with redistribution

Â Â Â Â Â Â  Â Â  route-map OSPF->RIP deny 10

Â  Â  Â -> Use NET plus the network portion of the IP address for prefix-lists

Â Â Â Â Â Â  Â Â  ip prefix-list NET3 permit 3.0.0.0/8

**Notepad:**

Â - Very helpful with XR router configuration on the SP lab

Â  Â  Â -> XR router configuration can be long, but is mostly repeatitive

Â - Very helpful if you happen to lock up a device and it needs to be reset or reloaded

Â - Be aware of self-inflected configuration errors

Â  Â  Â -> Copy and paste errors

Â  Â  Â  Â  Â  -> Pasting configuration into the wrong device

Â  Â  Â  Â  Â  -> Not copying the entire configuration

Â  Â  Â -> Not changing router-ids

Â  Â  Â  Â  Â  -> Duplicate router-ids

Â  Â  Â -> Misconfigured network statements

Â  Â  Â -> Misconfigured interfaces

Â  Â  Â -> Extra interfaces

Â  Â  Â  Â  Â  -> loopback101 is on both R1 and R2, but R2 should only have loopback102

Â  Â  Â -> Configuration not completely copied to the devices

Â  Â  Â  Â  Â  -> Did you verify?

Â  Â  Â  Â  Â  -> Just because you didnâ€™t see any errors when you pasted the configuration, doesnâ€™t mean that the configuration is correct

Â  Â  Â -> Making configuration changes on the devices, but not updating the configuration in notepad

**When you get stuck:**

Â - Redistribution

Â Â Â Â Â -> Idea to get around redistribution if you are having problems, use a static route and loose points here, but maintain connectivity. Â Count points later. Â If you have enough, leave it.

Â Â Â Â Â -> Donâ€™t actually use a static route, you can redistribute an individual route to establish connectivity.

Â - DMPVN

Â  Â  Â -> So many other protocols ride over DMVPN. Â If you are not able to make it work as the task specified, use whatever ugly configuration you can to get reachability

Â  Â  Â -> At least this way, you may be able to get points with the other protocols and tasks.

**Example config:**

!Â R1

!Â TaskÂ 1

routerÂ ospfÂ 1

Â router-idÂ 0.0.0.1

intÂ s1/0

Â ipÂ ospfÂ 1Â areaÂ 0

intÂ lo0

Â ipÂ ospfÂ 1Â areaÂ 0

!Â TaskÂ 2

ipÂ vrfÂ TST

Â rdÂ 1:10

Â route-targetÂ bothÂ 34:34

intÂ s1/1

Â ipÂ vrfÂ forwardingÂ TST

Â ipÂ addÂ 13.1.1.1Â 255.255.255.0

routerÂ eigrpÂ asdf

Â address-familyÂ ipv4Â vrfÂ TSTÂ autoÂ 100

Â  netwÂ 13.1.1.1Â 0.0.0.0

mplsÂ ldpÂ router-idÂ lo0

mplsÂ labelÂ protocolÂ ldp

intÂ s1/0

mplsÂ ip

routerÂ bgpÂ 65001

Â noÂ bgpÂ defaultÂ ipv4-unicast

Â neiÂ 2.2.2.2Â remote-asÂ 65001

Â neiÂ 2.2.2.2Â upÂ lo0

Â address-familyÂ vpnv4Â unicast

Â  neiÂ 2.2.2.2Â activate

Â  neiÂ 2.2.2.2Â send-communityÂ extended

!

routerÂ eigrpÂ asdf

Â address-familyÂ ipv4Â vrfÂ TSTÂ autoÂ 100

Â  topologyÂ base

Â  Â noÂ redistributeÂ bgpÂ 65001Â metricÂ 1Â 1Â 1Â 1Â 1

Â  Â redistributeÂ bgpÂ 65001Â metricÂ 1000000Â 1Â 255Â 1Â 1500

routerÂ bgpÂ 65001

Â address-familyÂ ipv4Â vrfÂ TST

Â  redistributeÂ eigrpÂ 100

!Â TaskÂ 3

route-mapÂ SOOÂ permitÂ 10

Â setÂ extcommunityÂ sooÂ 1:111

intÂ s1/1

Â ipÂ vrfÂ sitemapÂ SOO

!

route-mapÂ SOOÂ permitÂ 10

Â noÂ setÂ extcommunityÂ sooÂ 1:111

Â setÂ extcommunityÂ sooÂ 1:121

!Â R2

!Â TaskÂ 1

routerÂ ospfÂ 1

Â router-idÂ 0.0.0.2

intÂ s1/0

Â ipÂ ospfÂ 1Â areaÂ 0

intÂ lo0

Â ipÂ ospfÂ 1Â areaÂ 0

!Â TaskÂ 2

ipÂ vrfÂ TST

Â rdÂ 1:20

Â route-targetÂ bothÂ 34:34

intÂ s1/2

Â ipÂ vrfÂ forwardingÂ TST

Â ipÂ addÂ 24.1.1.2Â 255.255.255.0

routerÂ eigrpÂ asdf

Â address-familyÂ ipv4Â vrfÂ TSTÂ autoÂ 100

Â  netwÂ 24.1.1.2Â 0.0.0.0

!

mplsÂ ldpÂ router-idÂ lo0

mplsÂ labelÂ protocolÂ ldp

intÂ s1/0

mplsÂ ip

!

routerÂ bgpÂ 65001

Â noÂ bgpÂ defaultÂ ipv4-unicast

Â neiÂ 1.1.1.1Â remote-asÂ 65001

Â neiÂ 1.1.1.1Â upÂ lo0

Â address-familyÂ vpnv4Â unicast

Â  neiÂ 1.1.1.1Â activate

Â  neiÂ 1.1.1.1Â send-communityÂ extended

!

routerÂ eigrpÂ asdf

Â address-familyÂ ipv4Â vrfÂ TSTÂ autoÂ 100

Â  topologyÂ base

Â  Â noÂ redistributeÂ bgpÂ 65001Â metricÂ 1Â 1Â 1Â 1Â 1

Â  Â redistributeÂ bgpÂ 65001Â metricÂ 1000000Â 1Â 255Â 1Â 1500

routerÂ bgpÂ 65001

Â address-familyÂ ipv4Â vrfÂ TST

Â  redistributeÂ eigrpÂ 100

!Â TaskÂ 3

route-mapÂ SOOÂ permitÂ 10

Â setÂ extcommunityÂ sooÂ 2:222

intÂ s1/2

Â ipÂ vrfÂ sitemapÂ SOO

!

route-mapÂ SOOÂ permitÂ 10

Â noÂ setÂ extcommunityÂ sooÂ 2:222

Â setÂ extcommunityÂ sooÂ 1:121

!Â TaskÂ 4

route-mapÂ SOOÂ permitÂ 10

Â noÂ setÂ extcommunityÂ sooÂ 1:121

Â setÂ extcommunityÂ sooÂ 2:222

!

route-mapÂ SOOÂ permitÂ 10

Â noÂ setÂ extcommunityÂ sooÂ 2:222

Â setÂ extcommunityÂ sooÂ 1:121

ipÂ slaÂ 1

Â icmp-echoÂ 34.1.1.4Â source-interfaceÂ s1/2

Â thresholdÂ 500

Â timeoutÂ 500

Â freqÂ 5

Â vrfÂ TST

ipÂ slaÂ scheduleÂ 1Â lifeÂ foreverÂ start-timeÂ now

trackÂ 2Â ipÂ slaÂ 1Â reachability

eventÂ managerÂ appletÂ DOWN

Â eventÂ trackÂ 2Â stateÂ down

Â actionÂ 1.1Â cliÂ commandÂ â€œenableâ€

Â actionÂ 1.2Â cliÂ commandÂ â€œconfigÂ 1â€

Â actionÂ 1.3Â cliÂ commandÂ â€œroute-mapÂ SOOÂ permitÂ 10â€

Â actionÂ 1.4Â cliÂ commandÂ â€œnoÂ setÂ extcommunityÂ sooÂ 1:121â€

Â actionÂ 1.5Â cliÂ commandÂ â€œsetÂ extcommunityÂ sooÂ 2:222â€

eventÂ managerÂ appletÂ UP

Â eventÂ traceÂ 2Â stateÂ up

Â actionÂ 1.1Â cliÂ commandÂ â€œenableâ€

Â actionÂ 1.2Â cliÂ commandÂ â€œconfigÂ tâ€

Â actionÂ 1.3Â cliÂ commandÂ â€œroute-mapÂ SOOÂ permitÂ 10â€

Â actionÂ 1.4Â cliÂ commandÂ â€œnoÂ setÂ extcommunityÂ sooÂ 2:222â€

Â actionÂ 1.5Â cliÂ commandÂ â€œsetÂ extcommunityÂ sooÂ 1:121â€Â possible now to fail a section (just barely)Â and still pass (if you did awesome in another section).

**Recommendations that only work with a printed copy of the network diagram:**

**Things to look for in the lab diagram:**

Â - Loops

Â  Â  Â -> Circle them

Â - Stubs

Â  Â  Â -> Bracket them

Â - Count the VLANs

Â  Â  Â -> Slash through them

**Draw a big red â€œXâ€ on interfaces that have the following applied:**

Â - ACL

Â - Port-security
