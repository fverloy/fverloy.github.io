---
layout: post
title: NSX Service Chaining
date: '2014-05-11 22:00:00'
tags:
- vmware
- sdn
permalink: /nsx-service-chaining/
---

When NSX was introduced to the public at large during VMworld 2013, there was a logo slide including all the initial NSX partners that had a working demo together with NSX. Now in order to have both solutions work together in a meaningful way we often resort to service insertions or service chaining.

**What is service chaining?**

Some different definitions exist which is somewhat expected in what is a very active (SDN) market right now, I’ll liken it to when you, in a traditional network with lots of middle boxes (load-balancer, WAN accelerator, Firewall, etc.) you have to cable up these appliances in your network and then manually configure the traffic flow to “route” via these boxes for a specific service. i.e. you want your external website to be load-balanced across multiple front-end servers so you point your webclients towards the load-balancer whom then spreads the traffic across multiple servers. In the SDN/NFV space this is somewhat similar but more rapid and dynamic (on-demand), i.e. you can create policies that govern when certain services are required and these (usually through VM’s) are then put into the data path if and when needed. Since it should be fast to provision a new VM/application, provision network services should be equally fast, and we are not limited to the standard network plumbing (L3-4) either, which is quite important. In the world of Virtualization where Virtual Machines can move around physical hosts and physical network devices it is imperative that service chaining or service insertion can deal with these events, this is somewhat trickier in purely physical world.

**Some service chaining/service insertion solution examples;**

**Security with Palo Alto Networks**

If and when the added security capabilities of the virtual Palo Alto security appliance are needed traffic from the VM is transparently (you don’t need to do network configuration) inserted in the path of the virtual security appliance. Context is shared between NSX and Palo Alto Panorama (centralized management) to keep track of VM movement and make sure that security policies can still be enforced.

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/w1TS7CZQ1sY?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></figure>

**WAN optimisation with Silver Peak**

The Silver Peak integration (Agility for VMware Software Defined Data Centers / point-and-click workload optimization) provides traffic redirection for workload optimization, meaning that if WAN optimization is required you can enable redirection for a specific application/VM toward the Silver Peak VX VM which then does the optimization across the WAN.

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/5P6HecWGEBE?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></figure>

**ADC services with F5 Networks**

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/ybUWal6KgjU?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></figure>

**Trend Micro Deep Security**

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/R_KBd05l1w0?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></figure>

Many more examples, in various stages of development, exist.

