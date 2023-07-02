---
layout: post
title: On SDN, NFV, and OpenFlow
date: '2014-03-16 23:00:00'
tags:
- sdn
permalink: /on-sdn-nfv-and-openflow/
---

 **Introduction**

Network-, and network function virtualisation are the talk of the town at the moment, the sheer amount of vendor solutions is staggering, and the projected market opportunity is enough to make anyone’s head spin. I’ve been working in ‘traditional’ networking all my career and it looks like there finally is something new and exciting, dare I say sexy, to play with.

Because of all the buzz, and because the topic reaches far beyond pure networking folks, there does seem to be a lot of confusion around some of the terminology and what piece of technology is applicable where. In this, what I hope to be an introductory post, I wanted to touch upon the high level differences between SDN and NFV and what they represent.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn_nfv.png" class="kg-image" alt loading="lazy" width="335" height="242"></figure>

**History**

Let’s start with a little history but feel free to skip to the topic sections below.

I’m going to choose the early work being done at Stanford as the starting point, you can argue that the history of network virtualisation goes back way further, and in theory you would be right, but in order not to muddy the water too much I’ll start at Stanford.

2007 Stanford graduate Martin Casado, now at VMware, is generally credited with starting the software defined networking revolution, himself and co-conspirators Nick McKeown (Stanford), Teemu Koponen (VMware), and Scott Schenker (Berkeley) built on earlier work that attempted to separate the control plane from the data plane, but SDN more or less started there.

Martin Casado together with his colleagues created Ethane, which was intended as a way of centrally managing global policy, it used a “flow-based network and controller with a focus on network security”

The Ethane abstract reads:

_Intended as a new network architecture for the enterprise. Ethane allows managers to define a single networkwide fine-grain policy, and then enforces it directly. Ethane couples extremely simple flow-based Ethernet switches with a centralized controller that manages the admittance and routing of flows.”_

If you look at Aruba and Meraki in the wireless space for instance you can see that they also have this model of separating control- and data plane. The networking models that companies like Yahoo!, Google, and Amazon had started to use are also very closely related to this concept of SDN.

The “official” starting point of SDN is when the NOX + OpenFlow specification emerged in 2008, but again, it’s legacy goes back a lot further.

**The human middleware problem.**

If you look at a traditional network setup, you most likely encounter a 3 tier network architecture, with the Core network at the top, the Distribution network in the middle, and the Access network close to the end-users and the applications. This model is highly biased towards North-South traffic, since Layer 3 decisions are made higher up instead of close to the access network. Port security and VLANs are applied at the Access layer, Policy and ACL’s are applied in the Distribution layer, etc. all this has some redundancy and scalability limits due to operational complexity. Furthermore you cannot use all the capacity of the links because of the design (spanning tree etc.).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn_nfv2.png" class="kg-image" alt loading="lazy" width="550" height="544"></figure>

Now if you look the practical aspects of packet forwarding, you have packets that come in, those are evaluated against a table, the table determines what happens to the packet, i.e. does the packet get dropped, forwarded, replicated, etc., and then it goes out the other end. Now these tables against which the pakets get evaluated are populated in one of two ways, some tables (L2 and L3 table) are populated using dynamic (routing) algorithms, these algorithms run on every network node, when there is a change in the network they calculate state and have to communicate with all the other nodes and populate the tables. This is actually rather functional, not a lot of human interaction, should be very stable and able to dynamically react to changes.

But there are other tables, like ACLs, QoS, port groups, etc. and these tables are populated using very different methods like human beings, scripts. etc. these are slow, and error prone, what happens if your network admin doesn’t show up Monday..?

Now there are management APIs and SDKs to programatically deal with these tables, but not really a standardised way, and certainly not to do it at scale. i.e. the APIs expose functions of the hardware chip’s and these provide a fixed function. Moreover there are limited consistency semantics in the APIs, i.e. you can not be sure that if you programmed state it would get consistently implemented on all the network devices.

This problem led to the creation of OpenFlow, in order to programmatically manage all of the “datapath” state in the network.

**OpenFlow**

If you look a networking devices today you generally see 3 levels of abstraction (3 planes), the Data plane, where fast packet forwarding takes place (ASICs live here) based on the information located in the forwarding table. To get state information the forwarding table this has to be populated from the Control plane. The Control plane is where your routing protocols live, like OSPF and BGP, who figure out the topology of the network by exchanging information with neighbouring devices in the network. A subset of the “best” forwarding (IP routing) table in the Control gets pushed/programmed into the forwarding table in the Data plane. On top of both the Data and Control plane we have the Management plane. This is where the configuration happens through a command line interface, GUI, or API.

In an OpenFlow based network there is a physical separation between the Control plane and the Data plane. Usually the Management plane is a dedicated controller that establishes a TCP connection (OpenFlow runs over TCP) with each networking device, the controller runs topology discovery (not the Control plane). The controller computes the forwarding information that needs to get pushed/programmed to all networking devices and uses the OpenFlow protocol to distribute this information to the flow tables of the devices.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn_nfv3.png" class="kg-image" alt loading="lazy" width="460" height="300"></figure>

The OpenFlow protocol is published by the Open Networking Foundation, and it somewhat depends on the switch version what version of OpenFlow they implement (because some feel the rate of innovation is too high 😉 ) . i.e. not all OpenFlow capable devices have implemented the same version and so interoperability with features of later versions is not always guaranteed.

OpenFlow is an interface to your existing switches, it does not really enable features that are not already there.

So why do we have OpenFlow then?

OpenFlow provides you with the ability to innovate faster within the network, allowing you to get a competitive advantage. It also moves the industry towards standards, which creates more market efficiency. And if you decouple layers, they can evolve independently, fixed monolithic systems are slower to evolve.

**Software Defined Networking (SDN)**

Obviously SDN and OpenFlow are closely related. You can think of OpenFlow as an enabler of SDN since SDN requires a method for the Control plane to talk to the Data plane, OpenFlow is one such mechanism.

In SDN we also talk about decoupling the Control and Data plane but SDN also looks at what happens above the Control plane, i.e. how do we take the information we get from the application space and use that as input to dynamically program the network below it.

Usually the Control plane functionality is handled by a controller cluster that is implemented as a virtual machine cluster running on top of a hypervisor for maximum flexibility.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn_nfv4.png" class="kg-image" alt loading="lazy" width="527" height="340"></figure>

We also use the concept of overlay networking (called encapsulation protocol in the picture above) to dynamically create connections between switches (or application tiers, or users,…) independent of the physical network. This way we can link applications using the most optimum path independent of the physical topology.

As a crazy high level example let’s look at Hadoop , generally speaking there are 2 cycles in the Hadoop process, you have the mapping phase and you have the reduce phase.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn_nfv5.gif" class="kg-image" alt loading="lazy" width="1108" height="637"></figure>

What if the flow the traffic takes between these 2 phases is best served by another path, i.e. the actual compute nodes with different capabilities are located on different parts of the network and we want traffic to run over the shortest path possible. In a physical world it would be very difficult to accomplish this dynamic reorganisation of the network but in SDN land you potentially could.

Or a more traditional example would be, I need to spin up a new virtual machine, this VM requires network connectivity, so I ask the network team to place the physical port of the switch in the right VLAN so the VM can start to communicate. In most organisations this could take some time so what if the application itself, based on the SDN policies set in place (once) was able to direct this action.

Another benefit most SDN solutions implement is the distribution of routing (and potentially other services) to the hypervisor layer, either with an Agent (virtual machine) or integrated in the kernel or virtual switch. This means that the traditional north-south bias of a physical 3 tier network architecture is no longer hairpinning traffic, if you can route layer 3 traffic from within the hypervisor layer you can potentially move traffic across the server “backplane” between 2 virtual machines living on separate layer 3 networks but on the same host. Or between hosts across the overlay network. Efficiently moving east-west traffic in the data center.

Now of course once the network exists as a software construct, you can just as easily manipulate it as a virtual machine, i.e. snapshot it, roll it back, clone it, etc.

If you have a background in VMware administrator you understand that normally you would apply configuration in vCenter and have that distributed to all you ESXi hosts, you don’t need to log into each host individually to configure them, the same thing applies to SDN, you set policy at the controller layer and it feeds down to the switches.

In terms of physical architecture the traditional 3 tier (core, aggregation, and access) design is being replaced by a more flatter network like leaf-spine whereby all the links are being utilised for more bandwidth and resiliency.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sdn_nfv6.png" class="kg-image" alt loading="lazy" width="445" height="231"></figure>

In this type of architecture spanning tree is being replaced by things like TRILL and SPB to enable all links to forward traffic, eliminate loops and create meshes to let traffic take the shortest path between switches.

**Network Function Virtualisation (NFV)**

While SDN has grown out of the enterprise networking space, NFV has a background in the telco and service provider world. It was CSPs looking to avoid the need to implement more hardware middle-boxes when rolling out more functionality in the network. i.e. specialised load-balancers, firewalls, caches, DNS, etc.

So taking a page out of the server virtualisation book, what if some of those higher layer services could be implemented as virtual machines, what would that mean in terms of flexibility and cost improvement (reduced CAPEX, OPEX, space and power consumption)?

Again SDN and NFV are closely related. NFV builds virtualised network functions, but how the network is built (the interconnectivity) is the job of SDN.

These virtualised building blocks can then be connected (chained) together to create services.

Like I mentioned in the beginning of the post, I wanted to provide a high level, abstracted overview, and I will attempt to go into some more detail on each of these in subsequent articles.

