---
layout: post
title: Software Defined Shenanigans
date: '2013-05-04 22:00:00'
tags:
- sdn
---

Software defined anything (SDx) is the new black.

In July of last year VMware acquired Software Defined Networking (SDN) vendor Nicira and suddenly every network vendor had a SDN strategy, they must have reckoned the Google hits alone from people searching for SDN justified a change in vision.

Now VMware (a.o.) is further leading the charge by talking about the Software Defined Data Center (SDDC), wherein anything, the entire data center is now pooled, aggregated, and delivered as software, and managed by intelligent, policy-driven software.

Cloud and XaaS are so last year, SDx is where it’s at, it is the halo effect gone haywire.

A lot of networking and storage companies, both “legacy” and “start-up”, are scurrying around trying to figure out how to squeeze “Software-Defined” into their messaging.

**So what defines software defined?**

Software defined first appeared in the context of networking, traditionally network devices were delivered as a monolithic appliance, but logically you can think of them as consisting out of 3 parts, the data plane, the management plane, and the control plane.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn-logical-1.png" class="kg-image" alt loading="lazy" width="931" height="561" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sdn-logical-1.png 600w, __GHOST_URL__ /content/images/2021/08/sdn-logical-1.png 931w" sizes="(min-width: 720px) 720px"></figure>

The Data plane is relatively straight forward (no pun intended), it is where your data packets travel from point A to point B. When packets and frames arrive on the ingress ports of the network device the forwarding table is what all routers and switches use to dispatch frames and packets to their egress ports.

The Management plane, besides providing management functions such as device access, os updates, etc. also delivers the Forwarding table data from the Control plane towards the Data plane.

The Control plane is more involved, as networks become more sophisticated, (routing) algorithms here can be pretty complex (and complexity often leads to bugs). Algorithms here are not uniform and dynamic because they are expected to support a wide range of use cases and deployment scenarios.

The idea of SDN is to separate these planes, separate the control/management function from the data function to increase flexibility. Now imagine you have moved to the control function to a system that controls other functions in your data center, like creating virtual machines and storage, as well. No longer are you limited by silos of control, you can potentially manage everything that is needed to deploy new applications (vm, network, storage, security, …) from a single point of control (single pane of glass?).

**Is exposing API’s enough?**

It has always been possible to control functions in the network device programmatically, a lot of vendors are merely allowing you to control the existing control plane using API’s, I would argue this is not SDN, at least not in a purist sense, because it lacks scalability. (This point is very debatable I admit).

The aim is not having the control plane in each monolithic device, but rather have the intelligence outside, using OpenFlow for example, or the Big Network Controller from Big Switch Networks, allowing more flexibility and greater uniformity (one can dream).

**Northbound API**

The northbound API on a software-defined networking (SDN) controller enables applications and orchestration systems to program the network and request services from it. This is what the non-network vendors will use to integrate with your SDN, the problem today is that the service is not standardised (yet?), meaning that it is less open than we want it to be.

**Is SDN the same as network virtualisation?**

Network virtualisation adds a layer of abstraction (like all virtualisation) to the network often using tunnelling or an overlay network across the existing physical network. Nicira uses STT, VMware already had VXlan, Microsoft uses NVGRE, … I would argue that network virtualisation often is an underlying part of SDN.

**Software Defined Storage (SDS)**

In the world of storage we are also exposed to software defined, a lot of storage start-ups are using the SDS messaging to combat existing (or legacy as the start-ups would prefer) storage vendors to claim they have something new and improved. This, in my humble opinion, is not always warranted.

If you define SDS as SDN whereby the control plane is separated from the data plane, this is enabled by the lower-level storage systems abstracting their physical resources into software. Same reasons prevail, dynamism flexibility, more control,… These abstracted storage resources are then presented up to a control plane as “software-defined” services. The exposure and management of these services is done through an orchestration layer (like the northbound API in SDN world). The quality and quantity of these services dependents on the virtualisation and automation capabilities of the underlying hardware (is exposing API’s enough?).

Some would argue that because of the existing architectures of legacy storage systems this becomes more cumbersome and less flexible compared to the new start-up SDS players. Like you have new players in SDN (Arista (even though they don’t seem to like the SDN terminology very much), Plexxi,…) baking these technologies in from the ground up, you have the same with storage vendors, but I would argue that the rate of innovation seems to be much higher here. A lot of new storage vendors (ExaBlox, Tintri, PureStorage, Nimble,..) , a lot of new architectures (Fusion-IO, Pernixdata, SanDisk FlashSoft,…), a lot of acquisitions of flash based systems by legacy vendors, etc… make it that I don’t believe “legacy” storage vendors are going the way of the dinosaur just yet. I do however think it will lead to a lot of confusion, like software only storage suddenly being SDS etc.

**Is SDS the same as storage virtualisation?**

Like network virtualisation in SDN, storage virtualisation in SDS can play it’s part. Storage virtualisation is another abstraction between the server and the storage array, one such abstraction can be achieved by implementing a storage hypervisor. The storage hypervisor can aggregate multiple different arrays, of different vendors, of maybe even generic JBODs. The storage hypervisor tends to not, at least not always, use most of the capabilities of the array instead treating them as generic storage. Datacore for example sells a storage hypervisor, so does Virsto which was acquired by VMware.

In a more traditional sense the IBM SVC, NetApp V-series, EMC VPLEX can be considered storage virtualisation, or more accurately storage federation. And then you have logical volume managers, LUNs, RAID sets, all abstraction, all “virtualisation”… so a lot of FUD will be incoming.

**Is it all hype?**

Of course not, some of the messaging might be confusing, and some vendors like to claim they are part of the latest trend without much to show for it, but the industry is moving, fast, adding functionality to legacy systems, building new architectures to deliver (at least partially) on the promise of better. But as always, there is a lot of misinformation about certain capabilities in certain products, maybe a little too much talking and not enough delivering. I expect a great deal of consolidation in the next few years, both of companies and terminology, so look carefully at who is doing what and how this matches with your companies strategy going forward. Exiting times ahead though.

