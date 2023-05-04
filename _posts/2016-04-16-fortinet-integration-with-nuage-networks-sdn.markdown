---
layout: post
title: Fortinet integration with Nuage Networks SDN
date: '2016-04-16 22:00:00'
tags:
- networking
- sdn
---

 **Introduction**

Nuage Networks VSP, with the emphasis on P for Platform provides many integration points for 3rd party network and security providers (a.o.) this way the customer can leverage the SDN platform and build end-to-end automated services in support of his/her application needs.

One of the integration partners is Fortinet whereby we can integrate with the FortiGate Virtual Appliances to provide NGFW services and automated FW management via FortiManager.

**Integration example**

In the example setup below we are using OpenStack as the Cloud Management System and KVM as the OS Compute hosts. We have the FortiGate Virtual Appliance connected to a management network (orange), a untrusted interface (red), and a trusted/internal interface (purple). On the untrusted network we have a couple of virtual machines connected, and on the internal network.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/overview.png" class="kg-image" alt loading="lazy" width="1108" height="737" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/overview.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/overview.png 1000w, __GHOST_URL__ /content/images/2021/08/overview.png 1108w" sizes="(min-width: 720px) 720px"></figure>

**Dynamic group membership**

Since we are working in cloud environment where new workloads are spun up and down at a regular pace resulting in a dynamic allocation of IP addresses, we need to make sure that we can keep the firewall policy intact. To do this we use dynamic group membership that adds and deletes the IP addresses of the virtual machines based on group membership on both platforms. The added benefit of this is that security policy does not go stale, when workloads are decommissioned in the cloud environment it’s address information is automatically removed from the security policies resulting in a more secure and stable environment overall.

Looking at the FortiGate below, we are using dynamic address groups to define source and destination policy rules. The membership of the address groups is synchronised between the Nuage VSP environment and Fortinet.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortigate1.png" class="kg-image" alt loading="lazy" width="1108" height="255" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortigate1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortigate1.png 1000w, __GHOST_URL__ /content/images/2021/08/fortigate1.png 1108w" sizes="(min-width: 720px) 720px"></figure>

If we look at group membership in Fortinet we can see the virtual machines that are currently a member of the group. As you can see in the image below currently the group “PG1 – Address group 1 – Internal” has 1 virtual machine member.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortinet001.png" class="kg-image" alt loading="lazy" width="1108" height="405" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortinet001.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortinet001.png 1000w, __GHOST_URL__ /content/images/2021/08/fortinet001.png 1108w" sizes="(min-width: 720px) 720px"></figure>

If we now create a new virtual machine instance in OpenStack and make that instance a member of the corresponding Policy Group in Nuage Networks VSP it will be automatically synced back to Fortinet.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/instance001.png" class="kg-image" alt loading="lazy" width="1108" height="862" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/instance001.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/instance001.png 1000w, __GHOST_URL__ /content/images/2021/08/instance001.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Looking at Nuage Networks VSP we can see the new OpenStack virtual machine is a member of the Policy Group “PG1 – Address Group 1 – Internal”

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortinet002.png" class="kg-image" alt loading="lazy" width="1108" height="389" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortinet002.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortinet002.png 1000w, __GHOST_URL__ /content/images/2021/08/fortinet002.png 1108w" sizes="(min-width: 720px) 720px"></figure>

If we now go back to our FortiGate we can see the membership of the corresponding group has been updated.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortinet003.png" class="kg-image" alt loading="lazy" width="1108" height="409" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortinet003.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortinet003.png 1000w, __GHOST_URL__ /content/images/2021/08/fortinet003.png 1108w" sizes="(min-width: 720px) 720px"></figure>

**Traffic Redirection**

Now that we have dynamic address groups we can use these to create dynamic security policy, in order to selectively forward traffic from Nuage Networks VSP to FortiGate we need to create a Forward Policy in VSP.

In the picture below we define a new redirection target pointing to the FortiGate Virtual Appliance, in this case we opted for L3 service insertion, this could also be virtual wire based.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortinet004.png" class="kg-image" alt loading="lazy" width="1108" height="931" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortinet004.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortinet004.png 1000w, __GHOST_URL__ /content/images/2021/08/fortinet004.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Now we need to define which traffic to classify as “interesting” and forward to FortiGate, because Nuage Networks VSP has a built-in distributed stateful L4 firewall we can create a security baseline that handles common east-west traffic locally and only forwards traffic that demands a higher level inspection to the FortiGate virtual appliance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortinet005.png" class="kg-image" alt loading="lazy" width="1108" height="748" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortinet005.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortinet005.png 1000w, __GHOST_URL__ /content/images/2021/08/fortinet005.png 1108w" sizes="(min-width: 720px) 720px"></figure>

In the picture above we can select the Protocol, in this case I’m forwarding all traffic, but we could just as easily select for example TCP and define interesting traffic based on source and destination ports. We need to select the origin and destination network, in this case we use the dynamic address groups that are synced with Fortinet, this could also be based on more static network information. Finally we select the forward action and point to the Fortinet Virtual Appliance.

We have a couple of policies defined, as you could see in the picture at the top of the post we are allowing ICMP traffic between the untrusted network and the internal network. In the picture below I’ve logged on to one of the untrusted clients and am pinging the internal server.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fn1.png" class="kg-image" alt loading="lazy" width="1108" height="541" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fn1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fn1.png 1000w, __GHOST_URL__ /content/images/2021/08/fn1.png 1108w" sizes="(min-width: 720px) 720px"></figure><figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fn2.png" class="kg-image" alt loading="lazy" width="1108" height="558" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fn2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fn2.png 1000w, __GHOST_URL__ /content/images/2021/08/fn2.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Since this traffic mathes the ACL redirect rule we have configured in Nuage Networks VSP we can see a flow redirection at the Open vSwitch level pointing to the FortiGate virtual appliance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/ovs1.png" class="kg-image" alt loading="lazy" width="1108" height="92" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/ovs1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/ovs1.png 1000w, __GHOST_URL__ /content/images/2021/08/ovs1.png 1108w" sizes="(min-width: 720px) 720px"></figure>

We can also see that the Forward statics counters in VSP are increasing and furthermore determine that the traffic is logged for referential purposes

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/stats1.png" class="kg-image" alt loading="lazy" width="1108" height="631" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/stats1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/stats1.png 1000w, __GHOST_URL__ /content/images/2021/08/stats1.png 1108w" sizes="(min-width: 720px) 720px"></figure>

If we look at FortiGate we can see that the traffic is indeed received and allowed by the virtual appliance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortinet006.png" class="kg-image" alt loading="lazy" width="1108" height="128" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortinet006.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortinet006.png 1000w, __GHOST_URL__ /content/images/2021/08/fortinet006.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Same quick test with SSH which is blocked by our ForiGate security policy.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/ssh1.png" class="kg-image" alt loading="lazy" width="1108" height="89" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/ssh1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/ssh1.png 1000w, __GHOST_URL__ /content/images/2021/08/ssh1.png 1108w" sizes="(min-width: 720px) 720px"></figure><figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/fortigate4.png" class="kg-image" alt loading="lazy" width="1108" height="206" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/fortigate4.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/fortigate4.png 1000w, __GHOST_URL__ /content/images/2021/08/fortigate4.png 1108w" sizes="(min-width: 720px) 720px"></figure>

So as you can see a very solid integration between the Nuage Networks datacenter SDN solution and Fortinet to secure dynamic cloud environments in an automated way.

