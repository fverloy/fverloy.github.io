---
layout: post
title:  "Managing Riverbed VSP with VMware vCenter"
date:   2012-11-27 14:21:43 +0200
categories: Riverbed, VMware
permalink: /2012/11/27/managing-riverbed-vsp-with-vmware-vcenter/
---
Riverbed has recently released version 2 of the EX platform software, this includes RiOS 8 and Virtual Services Platform v2. VSPv2 runs VMware ESXi 5 as it’s hypervisor layer and as such can be managed by VMware vCenter.

In this post I’ll first cover how to install EX 2 on your existing Riverbed Steelheads and then we’ll look at managing the hypervisor with VMware vCenter.

First thing you need is the new EX 2 firmware which can be downloaded from our support website (http://support.riverbed.com)

![blog image]({{ "/assets/vsp1.png" | absolute_url }})

Install the new firmware just like any regular update and reboot the appliance.

![blog image]({{ "/assets/vsp2.png" | absolute_url }})

After the appliance has rebooted you will notice a new menu option under Configure, called Virtualization. Here you can install the VSP platform and also migrate any legacy VSPv1 packages you have installed.

![blog image]({{ "/assets/vsp3.png" | absolute_url }})

Before you install ESXi, it is recommended you select the disk layout you need, this will allocate the internal disks on your Steelhead EX platform to your required setup (i.e. will you use the appliance only for Granite, only for VSP, or for a mix of both) by going to the Virtual Services Platform page.

![blog image]({{ "/assets/vsp4.png" | absolute_url }})

After you have made your selection you can go ahead and launch the ESXi installation wizard.

![blog image]({{ "/assets/vsp5.png" | absolute_url }})

As you can see the ESXi installation wizard uses a familiar colour scheme to VMware engineers.

![blog image]({{ "/assets/vsp6.png" | absolute_url }})

The Wizard is pretty self explanatory.
Start by giving ESXi a management IP, this can be placed on either or both our Primary and AUX interface.

![blog image]({{ "/assets/vsp7.png" | absolute_url }})

Enter the ESXi credentials in order to manage ESXi using vCenter. (or standalone).

![blog image]({{ "/assets/vsp8.png" | absolute_url }})

If you want you can enter VNC credentials so you can have access to the ESXi console.

![blog image]({{ "/assets/vsp9.png" | absolute_url }})

After verifying your settings click next to install and configure ESXi.

![blog image]({{ "/assets/vsp10.png" | absolute_url }})

After the installation has finished you can manage the VSP platform by going to Configure, Virtualization, Virtual Services Platform.

![blog image]({{ "/assets/vsp11.png" | absolute_url }})

Here you can see the resources currently allocated to the vSphere hypervisor, notice that at the moment we allocate 1 socket (with 2 cores – on the EX760 appliance) to the hypervisor, this is important for VMware licensing, should you choose to do so, if not you can keep running the free version (called embedded license) of the hypervisor by managing each EX appliance separately.

![blog image]({{ "/assets/vsp12.png" | absolute_url }})

Connect to your vCenter server using the vSphere Client (or Webclient) and add the Steelhead appliance (using the ESXi management address) to vCenter.

![blog image]({{ "/assets/vsp13.png" | absolute_url }})

At this point you can choose to add a license.

![blog image]({{ "/assets/vsp14.png" | absolute_url }})

If you change the license, this is reflected on the management console (web interface) of the Steelhead appliance.

![blog image]({{ "/assets/vsp15.png" | absolute_url }})

After adding the Steelhead appliance to vCenter you can manage it like any other vSphere server.

![blog image]({{ "/assets/vsp16.png" | absolute_url }})

So there you have it, Steelhead EX version 2, managed by VMware vCenter 5.1.
Happy consolidating!