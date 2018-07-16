---
layout: post
title:  "VMware Integrated OpenStack (VIO)"
date:   2014-08-25 14:21:43 +0200
categories: VMware, VIO, OpenStack
permalink: /2014/08/25/vmware-integrated-openstack-vio/
redirect_from:
  - /2014/08/25/vmware-integrated-openstack-vio/amp/
---
![blog image]({{ "/assets/openstack-logo.jpg" | absolute_url }})

VMware just announced the VMware Integrated OpenStack (VIO) solution at VMworld.

**What is it?**

VIO is not a VMware OpenStack distribution, it is a fully validated (and supported) architecture to use VMware components in OpenStack Programs, it provides pre-packaged VMware integration with OpenStack components to allow for easy deployment and management. In other words if you want to use OpenStack with VMware this is the easiest way to do it. The basic integration with OpenStack existed before the VIO packaging, VIO just makes it easier to consume and provides a supported implementation option from VMware.

![blog image]({{ "/assets/vio1.jpeg" | absolute_url }})

If you think about how you can consume OpenStack today you basically end up with 3 choices, either you go with the DIY approach and start from the OpenStack sourcecode to build your own solution (a bit like building your own Linux distro from source code, far fetched and not really a path a lot of people tend to walk down), or you use a well known distro from companies like Canonical, Mirantis, SUSE,… or you use an pre-integrated product like VIO.

VMware will allow you to leverage the VMware components in either the VIO product (tightly-integrated/fully validated) or by using the components of your choice (including VMware and non-VMware ones) to build your own (loosely-integrated) stack. That is after all the beauty of the OpenStack APIs, choice AND standardisation.

![blog image]({{ "/assets/vio2.jpeg" | absolute_url }})

**What VMware components are we talking about?**

Initially the focus is on vSphere (vCenter) for compute, NSX for networking, VSAN for storage, and vCenter Operations Manager for monitoring. Like mentioned above the integration was already there before, looking at the picture below you see with which OpenStack components we integrate. Components in red are OpenStack programs, components in blue are VMware’s.

![blog image]({{ "/assets/vio3.png" | absolute_url }})

For the integration VMware provides opensource drivers, the vCenter driver for Nova, the NSX plugin for Neutron and the vCenter VMDK driver for Cinder and Glance.
Note: the ESX VMDK driver has been deprecated since the Icehouse release.

The OpenStack documentation provides a good starting point to see how the integration actually takes place.

**OpenStack Nova – vSphere Integration**

OpenStack Compute (Nova) supports the VMware vSphere product family and enables access to advanced features such as vMotion, High Availability, and Dynamic Resource Scheduling (DRS). The VMware vCenter driver enables the nova-compute service to communicate with a VMware vCenter server that manages one or more ESX host clusters. The driver aggregates the ESX hosts in each cluster to present one large hypervisor entity for each cluster to the Compute scheduler. Because individual ESX hosts are not exposed to the scheduler, Compute schedules to the granularity of clusters and vCenter uses DRS to select the actual ESX host within the cluster. When a virtual machine makes its way into a vCenter cluster, it can use all vSphere features. The VMware vCenter driver also interacts with the OpenStack Image Service to copy VMDK images from the Image Service back end store.

VMware driver architecture

![blog image]({{ "/assets/vio4.jpg" | absolute_url }})

OpenStack – vSphere integration documentation

**OpenStack Cinder – VMDK driver Integration**

The VMware VMDK driver enables management of OpenStack Block Storage volumes on vCenter-managed datastores. Volumes are backed by VMDK files on datastores using any VMware compatible storage technology (such as, NFS, iSCSI, FiberChannel, and VSAN).

OpenStack – VMDK driver integration documentation

**OpenStack Neutron – NSX Integration**

OpenStack Networking uses the NSX plugin for Networking to integrate with an existing VMware vCenter deployment. When installed on the network nodes, the NSX plugin enables a NSX controller to centrally manage configuration settings and push them to managed network nodes. Network nodes are considered managed when they’re added as hypervisors to the NSX controller.

The diagram below depicts an example NSX deployment and illustrates the route inter-VM traffic takes between separate Compute nodes. Note the placement of the VMware NSX plugin and the neutron-server service on the network node. The NSX controller features centrally with a green line to the network node to indicate the management relationship:

![blog image]({{ "/assets/vio5.png" | absolute_url }})

OpenStack – NSX plug-in configuration

**VIO**

As mentioned before the VMware supported VIO solution will allow you to use VMware’s infrastructure components without needing to do the all the required configuration manually. The VIO beta release will initially be available as a private beta for which you can signup here.

![blog image]({{ "/assets/vio6.png" | absolute_url }})

