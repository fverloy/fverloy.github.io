---
layout: post
title: VMware NSX and Palo Alto NGFW
date: '2015-05-01 22:00:00'
tags:
- vmware
- sdn
- security
---

VMware NSX is a platform for network and security virtualization, and as such it has the capability to integrate onto it’s platform certain functionalities that are not delivered by VMware itself. One such integration point is with Palo Alto Networks’s Next-Generation Firewall.

VMware NSX has built-in L2-4 stateful firewall capabilities both in the distributed firewall running directly in the ESXi hypervisor for east-west traffic, and in the Edge Services Gateway VM for north-south traffic. If L2-4 is not sufficient for your specific use case we can use VMware’s NSX Service Composer to steer traffic towards a third party solution provider for additional inspection.

At a high level the solution requires 3 components, VMware NSX, The Palo Alto Networks VM-series VM-1000-HV, and Palo Alto Networks’ central management system, Panorama.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan1.png" class="kg-image" alt loading="lazy" width="1024" height="405" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/pan1.png 1000w, __GHOST_URL__ /content/images/2021/08/pan1.png 1024w" sizes="(min-width: 720px) 720px"></figure>

Currently the VM-1000-HV supports 250.000 sessions (8000 new sessions per second) and 1Gbps firewall throughput (with App-ID enabled). The VM-series firewall is installed on each host of the cluster where you want to protect virtual machines with Palo Alto’s NGFW. Each VM-series firewall takes 2 vCPU’s and 5GB RAM.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan2.png" class="kg-image" alt loading="lazy" width="257" height="123"></figure>

If you look (summarize-dvfilter) at each ESXi host after installation you should see the VM-series show up in the dvfilter slowpath section.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan3.png" class="kg-image" alt loading="lazy" width="919" height="232" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan3.png 600w, __GHOST_URL__ /content/images/2021/08/pan3.png 919w" sizes="(min-width: 720px) 720px"></figure>

We can also look at the Panorama central management console and verify that our VM-series are listed under managed devices.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan4.png" class="kg-image" alt loading="lazy" width="982" height="335" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan4.png 600w, __GHOST_URL__ /content/images/2021/08/pan4.png 982w" sizes="(min-width: 720px) 720px"></figure>

Deciding which traffic to pass to the VM-series is configured using the Service Composer in NSX. The Service Composer provides a framework that allows you to dictate what you want to protect by creating security groups, and then deciding how to protect the members of this group by creating and linking security policies.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan5.png" class="kg-image" alt loading="lazy" width="1024" height="405" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/pan5.png 1000w, __GHOST_URL__ /content/images/2021/08/pan5.png 1024w" sizes="(min-width: 720px) 720px"></figure>

It is perfectly feasible to use security policy to first enable NSX’s distributed firewall to deal with certain type of traffic (up to layer 4) and only steer other “interesting” traffic towards the Palo Alto VM-series, this way you can simultaneously benefit from the distributed throughput of the DFW and the higher level capabilities of Palo Alto Networks NGFW.

Using the Service Composer, we create a security policy and use the Network Introspection Service to select which external 3rd party service that we want to steer traffic to. In this case we select the Palo Alto Networks NGFW and can further select the source, destination, and specific traffic (protocol/port) that we want to have handled by the VM-series.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan6.png" class="kg-image" alt loading="lazy" width="1018" height="711" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan6.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/pan6.png 1000w, __GHOST_URL__ /content/images/2021/08/pan6.png 1018w" sizes="(min-width: 720px) 720px"></figure>

Today only the traffic is passed to the external service but it is feasible to pass on more metadata that additionally could be acted upon by the third party provider. For example what if we could pass along that the VM we are protecting is running Windows Server 2003 and thus needs to have certain additional security measures applied.

So now that we have a policy that redirects traffic to the VM-series we need to apply this to a specific group. The power of combining NSX with Palo Alto Networks lies in the fact that we can use dynamic groups (both on NSX and in Panorama) and that members of the dynamic groups are sync’ed (about every 60 seconds) between both solutions. This means that if we add or remove VM’s from groups, the firewall rules are automatically updated. No more dealing with large lists of outdated firewall rules relating to decommissioned applications that nobody is willing to risk deleting because no one is sure what the impact would be.

For example we could create a security group using dynamic membership based on a security tag, this security tag could easily be applied as metadata by a cloud management platform (vRealize Automation for example) at the time of creation of the VM. (or you can manually add/remove security tags using the vSphere Web Client).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan7.png" class="kg-image" alt loading="lazy" width="974" height="362" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan7.png 600w, __GHOST_URL__ /content/images/2021/08/pan7.png 974w" sizes="(min-width: 720px) 720px"></figure>

In Panorama we also have this concept of dynamic address groups, these are linked in a one-to-one fashion with security groups in NSX.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan8.png" class="kg-image" alt loading="lazy" width="865" height="570" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan8.png 600w, __GHOST_URL__ /content/images/2021/08/pan8.png 865w" sizes="(min-width: 720px) 720px"></figure>

If we look a the group membership of the address groups in Panorama we will see the IP address of the VM, this can then be leveraged to apply firewall rules in Palo Alto Networks.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan9.png" class="kg-image" alt loading="lazy" width="418" height="130"></figure>

NOTE: if I would remove the VM from the security group in NSX about 60 seconds later the IP address in Panorama would disappear.

Traffic is redirected by using the filtering, and traffic redirection module that are running between the VM and the vNIC. The filtering module is an extension of the NSX distributed firewall, the traffic redirection module defines which traffic is steered to the third party services VM (VM-series VM in our case).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan10.png" class="kg-image" alt loading="lazy" width="980" height="1025" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan10.png 600w, __GHOST_URL__ /content/images/2021/08/pan10.png 980w" sizes="(min-width: 720px) 720px"></figure>

If we use the same dvfilter command (summarize-dvfilter) on the ESXi host as before we can see which slots are occupied;

Slot 0 : implements vDS Access Lists. Slot 1: Switch Security module (swsec) capture DHCP Ack and ARP messages, this info then forwarded to the NSX Controller. Slot 2: NSX Distributed Firewall. Slot 4: Palo Alto Networks VM-series

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan11.png" class="kg-image" alt loading="lazy" width="676" height="542" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan11.png 600w, __GHOST_URL__ /content/images/2021/08/pan11.png 676w"></figure>

So as we are now able to steer traffic towards the Palo Alto Networks NGFW we can apply security policies, as an example we have built some firewall rules blocking ICMP and allowing SSH between two security groups.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan12.png" class="kg-image" alt loading="lazy" width="839" height="124" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan12.png 600w, __GHOST_URL__ /content/images/2021/08/pan12.png 839w" sizes="(min-width: 720px) 720px"></figure>

As you could see from the picture earlier the VM in the SG-PAN-WEB group has IP address 172.16.10.11 (matching the member IP seen above in the dynamic group DAG-WEB in Panorama).

We are not allowed to ping a member of the dynamic group DAG-APP as dictated by the firewall rules on the VM-series firewall.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan13.png" class="kg-image" alt loading="lazy" width="603" height="363" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan13.png 600w, __GHOST_URL__ /content/images/2021/08/pan13.png 603w"></figure>

Since SSH is allowed we can test this by trying to connect to a VM in the DAG-APP group.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan14.png" class="kg-image" alt loading="lazy" width="604" height="317" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan14.png 600w, __GHOST_URL__ /content/images/2021/08/pan14.png 604w"></figure>

We can also verify if this session shows up on the VM-series firewall by opening the console on the vSphere web client.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan15.png" class="kg-image" alt loading="lazy" width="766" height="209" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan15.png 600w, __GHOST_URL__ /content/images/2021/08/pan15.png 766w" sizes="(min-width: 720px) 720px"></figure>

And finally if we look at the monitoring tab on Panorama we can verify that our firewall rules are working as expected.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan16.png" class="kg-image" alt loading="lazy" width="1024" height="122" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan16.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/pan16.png 1000w, __GHOST_URL__ /content/images/2021/08/pan16.png 1024w" sizes="(min-width: 720px) 720px"></figure>

So that’s it for this brief overview of using Palo Alto Networks NGFW in combination with VMware NSX. As you can see from the screenshot below, NSX allows for a broad list of third party solutions to be integrated, so the solution is very extensible and true to it’s goal of being a network and security platform for the next generation data center.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/pan17.png" class="kg-image" alt loading="lazy" width="836" height="391" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/pan17.png 600w, __GHOST_URL__ /content/images/2021/08/pan17.png 836w" sizes="(min-width: 720px) 720px"></figure>