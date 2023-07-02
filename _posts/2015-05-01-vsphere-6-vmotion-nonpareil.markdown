---
layout: post
title: vSphere 6 – vMotion nonpareil
date: '2015-05-01 22:00:00'
tags:
- vmware
permalink: /vsphere-6-vmotion-nonpareil/
---

I remember when I was first asked to install ESX 2.x for a small internal project, must have been somewhere around 2004-2005, I was working at a local systems integrator and got the task because no one else was around at the time. When researching what this thing was and how it actually worked I got sucked in, then some time later when I saw VMotion(sic) for the first time I was hooked.

Sure I’ve implemented some competing hypervisors over the years, and even worked for the competition for a brief period of time (in my defence, I was in the application networking team 😉 ). But somehow, at the time, it was like driving a kit car when you knew the real deal was parked just around the corner.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmotion1.jpg" class="kg-image" alt loading="lazy" width="706" height="493" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmotion1.jpg 600w, __GHOST_URL__ /content/images/2021/08/vmotion1.jpg 706w"></figure>

So today VMware announced the upcoming release of vSphere 6, arguably the most recognised product in the VMware stable and the foundation to many of it’s other solutions.

A lot of new and improved features are available but I wanted to focus specifically on the feature that impressed me the most soo many years ago, vMotion.

In terms of vMotion in vSphere 6 we now have;

Cross vSwicth vMotion Cross vCenter vMotion Long Distance vMotion Increased vMotion network flexibility

**Cross vSwitch vMotion**

Cross vSwitch vMotion allows you to seamlessly migrate a VM across different virtual switches while performing a vMotion operation, this means that you are no longer restricted by the network you created on the vSwitches in order to vMotion a virtual machine. It also works across a mix of standard and distributed virtual switches. Previously, you could only vMotion from vSS to vSS or within a single vDS.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmotion2.png" class="kg-image" alt loading="lazy" width="1024" height="995" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmotion2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vmotion2.png 1000w, __GHOST_URL__ /content/images/2021/08/vmotion2.png 1024w" sizes="(min-width: 720px) 720px"></figure>

The following Cross vSwitch vMotion migrations are possible:

vSS to vSS vSS to vDS vDS to vDS (including metadata, i.e. statistics) vDS to vSS is not possible

The main use case for this is data center migrations whereby you can migrate VMs to a new vSphere cluster with a new virtual switch without disruption. It does require the source and destination portgroups to share the same L2. The IP address within the VM will not change.

**Cross vCenter vMotion**

Expanding on the Cross vSwitch vMotion enhancement, vSphere 6 also introduces support for Cross vCenter vMotion.

vMotion can now perform the following changes simultaneously:

Change compute (vMotion) – Performs the migration of virtual machines across compute hosts. Change storage (Storage vMotion) – Performs the migration of the virtual machine disks across datastores. Change network (Cross vSwitch vMotion) – Performs the migration of a VM across different virtual switches. and finally…

Change vCenter (Cross vCenter vMotion) – Performs the migration of the vCenter which manages the VM.

Like with vSwitch vMotion, Cross vCenter vMotion requires L2 network connectiviy since the IP of the VM will not be changed. This functionality builds upon Enhanced vMotion and shared storage is not required.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmotion3.png" class="kg-image" alt loading="lazy" width="1024" height="960" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmotion3.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vmotion3.png 1000w, __GHOST_URL__ /content/images/2021/08/vmotion3.png 1024w" sizes="(min-width: 720px) 720px"></figure>

The use cases are migration of Windows based vCenter to the vCenter Server Appliance (also take a look at the scale improvement of vCSA in vSphere 6) i.e. no more Windows and SQL license needed. Replacement of vCenters without disruption and the possibility to migrate VMs across local, metro, and cross-continental distances.

**Long Distance vMotion**

Long Distance vMotion is an extension of Cross vCenter vMotion but targeted to environments where vCenter servers are spread across large geographic distances and where the latency across sites is 150ms or less.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmotion4.png" class="kg-image" alt loading="lazy" width="1024" height="566" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmotion4.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vmotion4.png 1000w, __GHOST_URL__ /content/images/2021/08/vmotion4.png 1024w" sizes="(min-width: 720px) 720px"></figure>

The requirements for Long Distance vMotion are the same as Cross vCenter vMotion, except with the addition of the maximum latency between the source and destination sites must be 150 ms or less, and there is 250 Mbps of available bandwidth.

The operation is serialized, i.e. it is a VM per VM operation requiring 250 Mbps.

The VM network will need to be a stretched L2 because the IP of the guest OS will not change. If the destination portgroup is not in the same L2 domain as the source, you will lose network connectivity to the guest OS. This means that in some topologies, such as metro or cross-continental, you will need a stretched L2 technology in place. The stretched L2 technologies are not specified (VXLAN is an option here, as are NSX L2 gateway services). Any technology that can present the L2 network to the vSphere hosts will work, as it’s unbeknown to ESX how the physical network is configured.

**Increased vMotion network flexibility**

ESXi 6 will have multiple TCP/IP stacks, this enables vSphere to improve scalability and offers flexibility by isolating vSphere services to their own stack. This also allows vMotion to work over a dedicated Layer 3 network since it can now have it’s own memory heap, ARP and routing tables, and default gateway.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmotion5.png" class="kg-image" alt loading="lazy" width="1024" height="459" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmotion5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vmotion5.png 1000w, __GHOST_URL__ /content/images/2021/08/vmotion5.png 1024w" sizes="(min-width: 720px) 720px"></figure>

In other words the VM network still needs L2 connectivity since the virtual machine has to retain its IP. vMotion, management and NFC networks can all be L3 networks.

