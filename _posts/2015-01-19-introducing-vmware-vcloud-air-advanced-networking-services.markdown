---
layout: post
title: Introducing VMware vCloud Air Advanced Networking Services
date: '2015-01-19 23:00:00'
tags:
- vmware
- cloud
- networking
---

VMware just announced some additions to it’s public cloud service, vCloud Air, one of the additions is advanced networking services powered by VMware NSX. Today the networking capabilities of vCloud Air are based on vCNS features, moving forward these will be provided by NSX.

If you look at the connectivity options from your Data Center towards vCloud Air today you have:

Direct connect which is a private circuit such as MPLS or Metro Ethernet. IPSec VPN or via the WAN to a public IP address (think webhosting)

By switching from vCNS Edge devices to NSX Edge devices vCloud Air is adding SSL VPN connectivity from client devices to the vCloud Air Edge.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/air2.png" class="kg-image" alt loading="lazy" width="1024" height="476" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/air2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/air2.png 1000w, __GHOST_URL__ /content/images/2021/08/air2.png 1024w" sizes="(min-width: 720px) 720px"></figure>

VMware is adding, by using the NSX Edge Gateway, dynamic routing support (OSPF, BGP), and a full fledged L4/7 load-balancer (based on HA Proxy), that also provides SSL offloading. As mentioned before SSL VPN to the vCloud Air network, for which clients are available for Mac OS X, Windows, and Linux is also available. Furthermore the number of available interfaces have also been greatly increased from 9 to 200 sub-interfaces, and the system now also provides distributed firewall (firewall policy linked to the virtual NIC of the VM) capabilities.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/air3.png" class="kg-image" alt loading="lazy" width="1024" height="791" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/air3.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/air3.png 1000w, __GHOST_URL__ /content/images/2021/08/air3.png 1024w" sizes="(min-width: 720px) 720px"></figure>

The NSX Edge can use BGP to exchange routes between vCloud Air and your on-premises equipment over Direct Connect. NSX Edge can also use OSPF to exchange internal routes between NSX edges or a L3 virtual network appliance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/air4.png" class="kg-image" alt loading="lazy" width="1024" height="477" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/air4.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/air4.png 1000w, __GHOST_URL__ /content/images/2021/08/air4.png 1024w" sizes="(min-width: 720px) 720px"></figure>

NSX also introduces the concept of Micro-Segmentation to vCloud Air. This allows implementation of firewall policy at the virtual NIC level. In the example below we have a typical 3-tier application with each tier placed on its own L2 subnet.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/air5.png" class="kg-image" alt loading="lazy" width="1024" height="605" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/air5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/air5.png 1000w, __GHOST_URL__ /content/images/2021/08/air5.png 1024w" sizes="(min-width: 720px) 720px"></figure>

With micro-segmentation you can easily restrict traffic to the application- and database-tier while allowing traffic to the web-tier, even though, again just as an example all these VM’s sit on the same host. Assuming that at some point you will move these VM’s from one host to another host, security policy will follow the VM without the need for reconfiguration in the network. Or you can implement policy that does not allow VM’s to talk to each other even though they sit on the same L2 segment.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/air6.png" class="kg-image" alt loading="lazy" width="1024" height="746" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/air6.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/air6.png 1000w, __GHOST_URL__ /content/images/2021/08/air6.png 1024w" sizes="(min-width: 720px) 720px"></figure>

If you combine this with security policy capabilities in the NSX Edge Gateway you can easily implement firewall rules for both North-South and East-West traffic. The system will also allow you to build a “container” by grouping certain VM’s together and applying policies to the group as a whole. For example you could create 2 applications, each consisting of 1 VM from each tier (web, app, db), and set policies in a granular level. As a service provider you can very easily create a system that supports multiple tenants in a secure fashion. Furthermore this would also allow you to set policies and move VM’s from on-premises to vCloud Air while still retaining network and security configurations.

