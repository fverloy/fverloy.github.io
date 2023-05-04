---
layout: post
title: Networking Field Day 10 – Nuage Networks
date: '2015-09-28 22:00:00'
tags:
- sdn
---

 **Introduction**

Networking Field Day 10 was held from August 19 till 21 2015, in Silicon Valley, NFD is part of the Tech Field Day series of events organized by Stephen Foskett and team, and aims to bring together independent bloggers and IT product vendors to share information and opinions in a presentation format. In this setting demo’s and whiteboard sessions are usually appreciated more than slide-ware and marketing pitches. Assuming you are an independent (e.g. you don’t work for a vendor) blogger/influencer you can ask to become a TFD delegate and when selected get to experience these sessions first hand.

One of the vendors at NFD 10 was Nuage Networks, Nuage has appeared at various TFD events on numerous (10 times by my count) occasions and that’s how I first heard about and got interested in their solutions.

**So what did they show at NFD 10?**

First up was Sunil Khandekar, Founder and CEO, with an introduction to Nuage Networks and an update on the products. He talked about building software defined, programmable, automated data centers and how Nuage through its declarative policy based automation is applicable to all types of workloads, be it bare metal, virtual machines, or containers. Further he mentioned how Nuage is a complete SDN solution seeing that it hits on all the key tenets, namely; abstraction, automation, control, and visibility. Then he went on to give an update on Nuage Networks.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/nfd101.png" class="kg-image" alt loading="lazy" width="938" height="510" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/nfd101.png 600w, __GHOST_URL__ /content/images/2021/08/nfd101.png 938w" sizes="(min-width: 720px) 720px"></figure>

Nuage was started January 2012 with the idea that networking should be as instantaneous and consumable as compute has historically become, the solution, VSP, was launched about a year later in April 2013 delivering on this thesis. Currently Nuage is on its 4th release and has seen great customer traction.

The Virtual Services Platform (VSP) has 3 main components, the Virtual Services Directory (VSD), the Virtual Services Controller (VSC), and the Virtual Routing & Switching engine (VRS). Additionally Nuage also optionally provides a hardware VTEP gateway, the 7850 Virtualized Services Gateway (VSG), to connect legacy networking to VXLAN overlays.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/nfd102.png" class="kg-image" alt loading="lazy" width="938" height="510" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/nfd102.png 600w, __GHOST_URL__ /content/images/2021/08/nfd102.png 938w" sizes="(min-width: 720px) 720px"></figure>

To get a more detailed overview of the solution please see my Nuage Compendium page\*.

Sunil also announced the VSP SDK (VSPK) available on https://github.com/nuagenetworks with the idea of fostering open collaboration around the platform. Through this proposed github collaboration, custom scripts for network automation, control or visibility can be developed and shared by its customer community

It is also important to understand that Nuage takes the concept of network automation beyond the data center and extends it to the WAN (branch office) with Virtual Network Services (VNS).

**Virtual Network Services (VNS) basics**

Next up was Rotem Salomonovitch, head of product management for VNS, talking about how it works in some more detail. The idea of VNS is to make setting up a new branch stupidly easy and independent of the underlying (carrier) technology, essentially SDWAN with a lot of automation. VNS is available as a hardware box, a VM, and as a software only package to install on a bare metal server.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/nfd103.png" class="kg-image" alt loading="lazy" width="946" height="527" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/nfd103.png 600w, __GHOST_URL__ /content/images/2021/08/nfd103.png 946w" sizes="(min-width: 720px) 720px"></figure>

VNS uses the same control plane components from the data center (VSD / VSC) but has a different data plane, e.g. forwarding entity called the Network Services Gateway (NSG), reason being is that data plane’s in branches typically differ from those in data centers where you have GigE, 10GigE, 40GigE and beyond. In the branch you have different interface types and things like encryption in the WAN, additional security requirements, etc. The NSG is meant to be a platform, initial applications of this platform are networking services (routing, QoS, FW,…) but the goal is to go beyond network connectivity and enable application flexibility. (see section below). The appliance has multiple WAN ports and also USB connectivity so you could do things like extend it with external LTE connectivity for example.

Rotem then moved on to talk about different abstractions for different types of audiences, e.g. a developer just wants his application to be “connected” whereas the network design team are concerned with connection points, bandwidth consumption etc. To support this he talked about both vertical abstractions, for multi-tenancy, and horizontal abstractions for different audience types within each tenant. The idea is to expose only those abstractions that a particular audience is interested in (until we break all IT silos and everyone is a unicorn of course).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/nfd104.png" class="kg-image" alt loading="lazy" width="958" height="521" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/nfd104.png 600w, __GHOST_URL__ /content/images/2021/08/nfd104.png 958w" sizes="(min-width: 720px) 720px"></figure>

The idea of abstractions is not to have each team work in their own silo but rather have the system interpret a certain audience’ set of abstractions and translate those to complete the end-to-end policy setup e.g. the application developer uses the Application Designer to create a new app, he only defines the app concepts (e.g. front-end, middle-tier, database) and then the system translates those to subnets, ACLs, etc.

Another example is the VPN designer where you can bring up a new site by linking the device object in the GUI to a location, then, depending on the authentication method, the only thing that needs to happen is physically connecting the WAN and LAN ports on the device at the branch and the device will “bootstrap” automatically and pick up its configuration. The forwarding engine in the device is multi-tenant capable, each subnet is created as a L2 EVPN construct (remember that the idea of VNS is to be independent of the WAN technology), the access ports of the box are piped into those.

**Application Flexibility at the Branch**

The VNS does not only provide network connectivity but also allows you to run containerized workloads on top of it (it’s an Intel Atom based system). The main idea is to automate the attachment of existing container based applications to the branch network. One example would be to enable running external network operations tools to perform local logging of LAN elements and then setup a single encrypted connection over the WAN or do local auditing of running configurations. Another would be to run a user simulation at the branch before go-live to validate user experience and adjust as needed.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/nfd105.png" class="kg-image" alt loading="lazy" width="940" height="526" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/nfd105.png 600w, __GHOST_URL__ /content/images/2021/08/nfd105.png 940w" sizes="(min-width: 720px) 720px"></figure>

Theoretically you can run any containerized app (pull it from docker hub) on VNS (provided that you have enough resources to run it), Nuage takes care of the multi-tenant network aspects of running multiple containers on a single host.

**Boundary-less Wide Area Networking**

Next up was Hussein Khazaal, solution director, talking about extending connectivity from the data center to the branch, to the public cloud, making a VPC your own personal branch office with consistent business policies between all of them.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/nfd106.png" class="kg-image" alt loading="lazy" width="943" height="521" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/nfd106.png 600w, __GHOST_URL__ /content/images/2021/08/nfd106.png 943w" sizes="(min-width: 720px) 720px"></figure>

The way it works is you get (in this case) a virtual NSG from Nuage which comes as an Amazon AMI (Amazon Machine Image) and deploy it as any other instance to your VPC. Now you can use the NSG-v in Amazon just like any other NSG from your application designer / vpn designer. The classic use case would be to load-balance your front-end (web) application between your data center and Amazon in times of increased load, essentially cloud-bursting made real. If you would want to do something like this without Nuage you would need to figure out how to translate your business policies to the available constructs at each public cloud provider, with the NSG-v it consumes the centralized policies from the VSD just like any other forwarding engine.

