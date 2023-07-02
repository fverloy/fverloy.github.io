---
layout: post
title: VMware Integrated OpenStack (VIO)
date: '2014-08-24 22:00:00'
tags:
- openstack
- vmware
permalink: /vmware-integrated-openstack-vio/
---

VMware just announced the VMware Integrated OpenStack (VIO) solution at VMworld.

**What is it?**

VIO is not a VMware OpenStack distribution, it is a fully validated (and supported) architecture to use VMware components in OpenStack Programs, it provides pre-packaged VMware integration with OpenStack components to allow for easy deployment and management. In other words if you want to use OpenStack with VMware this is the easiest way to do it. The basic integration with OpenStack existed before the VIO packaging, VIO just makes it easier to consume and provides a supported implementation option from VMware.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vio1.jpeg" class="kg-image" alt loading="lazy" width="1024" height="768" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vio1.jpeg 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vio1.jpeg 1000w, __GHOST_URL__ /content/images/2021/08/vio1.jpeg 1024w" sizes="(min-width: 720px) 720px"></figure>

If you think about how you can consume OpenStack today you basically end up with 3 choices, either you go with the DIY approach and start from the OpenStack sourcecode to build your own solution (a bit like building your own Linux distro from source code, far fetched and not really a path a lot of people tend to walk down), or you use a well known distro from companies like Canonical, Mirantis, SUSE,… or you use an pre-integrated product like VIO.

VMware will allow you to leverage the VMware components in either the VIO product (tightly-integrated/fully validated) or by using the components of your choice (including VMware and non-VMware ones) to build your own (loosely-integrated) stack. That is after all the beauty of the OpenStack APIs, choice AND standardisation.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vio2.jpeg" class="kg-image" alt loading="lazy" width="1024" height="768" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vio2.jpeg 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vio2.jpeg 1000w, __GHOST_URL__ /content/images/2021/08/vio2.jpeg 1024w" sizes="(min-width: 720px) 720px"></figure>

**Which VMware components are we talking about?**

Initially the focus is on vSphere (vCenter) for compute, NSX for networking, VSAN for storage, and vCenter Operations Manager for monitoring. Like mentioned above the integration was already there before, looking at the picture below you see with which OpenStack components we integrate. Components in red are OpenStack programs, components in blue are VMware’s.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vio3.png" class="kg-image" alt loading="lazy" width="1024" height="234" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vio3.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vio3.png 1000w, __GHOST_URL__ /content/images/2021/08/vio3.png 1024w" sizes="(min-width: 720px) 720px"></figure>

For the integration VMware provides opensource drivers, the vCenter driver for Nova, the NSX plugin for Neutron and the vCenter VMDK driver for Cinder and Glance. Note: the ESX VMDK driver has been deprecated since the Icehouse release.

The OpenStack documentation provides a good starting point to see how the integration actually takes place.

**OpenStack Nova – vSphere Integration**

OpenStack Compute (Nova) supports the VMware vSphere product family and enables access to advanced features such as vMotion, High Availability, and Dynamic Resource Scheduling (DRS). The VMware vCenter driver enables the nova-compute service to communicate with a VMware vCenter server that manages one or more ESX host clusters. The driver aggregates the ESX hosts in each cluster to present one large hypervisor entity for each cluster to the Compute scheduler. Because individual ESX hosts are not exposed to the scheduler, Compute schedules to the granularity of clusters and vCenter uses DRS to select the actual ESX host within the cluster. When a virtual machine makes its way into a vCenter cluster, it can use all vSphere features. The VMware vCenter driver also interacts with the OpenStack Image Service to copy VMDK images from the Image Service back end store.

VMware driver architecture

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vio4.jpg" class="kg-image" alt loading="lazy" width="720" height="540" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vio4.jpg 600w, __GHOST_URL__ /content/images/2021/08/vio4.jpg 720w" sizes="(min-width: 720px) 720px"></figure>

OpenStack – vSphere integration documentation

**OpenStack Cinder – VMDK driver Integration**

The VMware VMDK driver enables management of OpenStack Block Storage volumes on vCenter-managed datastores. Volumes are backed by VMDK files on datastores using any VMware compatible storage technology (such as, NFS, iSCSI, FiberChannel, and VSAN).

OpenStack – VMDK driver integration documentation

**OpenStack Neutron – NSX Integration**

OpenStack Networking uses the NSX plugin for Networking to integrate with an existing VMware vCenter deployment. When installed on the network nodes, the NSX plugin enables a NSX controller to centrally manage configuration settings and push them to managed network nodes. Network nodes are considered managed when they’re added as hypervisors to the NSX controller.

The diagram below depicts an example NSX deployment and illustrates the route inter-VM traffic takes between separate Compute nodes. Note the placement of the VMware NSX plugin and the neutron-server service on the network node. The NSX controller features centrally with a green line to the network node to indicate the management relationship:

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vio5.png" class="kg-image" alt loading="lazy" width="660" height="399" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vio5.png 600w, __GHOST_URL__ /content/images/2021/08/vio5.png 660w"></figure>

**VIO**

As mentioned before the VMware supported VIO solution will allow you to use VMware’s infrastructure components without needing to do the all the required configuration manually. The VIO beta release will initially be available as a private beta for which you can signup here.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vio6.png" class="kg-image" alt loading="lazy" width="568" height="369"></figure>