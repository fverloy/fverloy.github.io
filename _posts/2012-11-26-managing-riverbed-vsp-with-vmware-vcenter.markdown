---
layout: post
title: Managing Riverbed VSP with VMware vCenter
date: '2012-11-26 23:00:00'
tags:
- riverbed
- vmware
---

Riverbed has recently released version 2 of the EX platform software, this includes RiOS 8 and Virtual Services Platform v2. VSPv2 runs VMware ESXi 5 as it’s hypervisor layer and as such can be managed by VMware vCenter.

In this post I’ll first cover how to install EX 2 on your existing Riverbed Steelheads and then we’ll look at managing the hypervisor with VMware vCenter.

First thing you need is the new EX 2 firmware which can be downloaded from our support website (http://support.riverbed.com)

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp1.png" class="kg-image" alt loading="lazy" width="735" height="274" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp1.png 600w, __GHOST_URL__ /content/images/2021/08/vsp1.png 735w" sizes="(min-width: 720px) 720px"></figure>

Install the new firmware just like any regular update and reboot the appliance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp2.png" class="kg-image" alt loading="lazy" width="666" height="600" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp2.png 600w, __GHOST_URL__ /content/images/2021/08/vsp2.png 666w"></figure>

After the appliance has rebooted you will notice a new menu option under Configure, called Virtualization. Here you can install the VSP platform and also migrate any legacy VSPv1 packages you have installed.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp3.png" class="kg-image" alt loading="lazy" width="390" height="274"></figure>

Before you install ESXi, it is recommended you select the disk layout you need, this will allocate the internal disks on your Steelhead EX platform to your required setup (i.e. will you use the appliance only for Granite, only for VSP, or for a mix of both) by going to the Virtual Services Platform page.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp4.png" class="kg-image" alt loading="lazy" width="567" height="192"></figure>

After you have made your selection you can go ahead and launch the ESXi installation wizard.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp5.png" class="kg-image" alt loading="lazy" width="671" height="237" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp5.png 600w, __GHOST_URL__ /content/images/2021/08/vsp5.png 671w"></figure>

As you can see the ESXi installation wizard uses a familiar colour scheme to VMware engineers.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp6.png" class="kg-image" alt loading="lazy" width="652" height="627" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp6.png 600w, __GHOST_URL__ /content/images/2021/08/vsp6.png 652w"></figure>

The Wizard is pretty self explanatory. Start by giving ESXi a management IP, this can be placed on either or both our Primary and AUX interface.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp7.png" class="kg-image" alt loading="lazy" width="591" height="454"></figure>

Enter the ESXi credentials in order to manage ESXi using vCenter. (or standalone).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp8.png" class="kg-image" alt loading="lazy" width="587" height="449"></figure>

If you want you can enter VNC credentials so you can have access to the ESXi console.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp9.png" class="kg-image" alt loading="lazy" width="586" height="212"></figure>

After verifying your settings click next to install and configure ESXi.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp10.png" class="kg-image" alt loading="lazy" width="612" height="173" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp10.png 600w, __GHOST_URL__ /content/images/2021/08/vsp10.png 612w"></figure>

After the installation has finished you can manage the VSP platform by going to Configure, Virtualization, Virtual Services Platform.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp11.png" class="kg-image" alt loading="lazy" width="653" height="568" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp11.png 600w, __GHOST_URL__ /content/images/2021/08/vsp11.png 653w"></figure>

Here you can see the resources currently allocated to the vSphere hypervisor, notice that at the moment we allocate 1 socket (with 2 cores – on the EX760 appliance) to the hypervisor, this is important for VMware licensing, should you choose to do so, if not you can keep running the free version (called embedded license) of the hypervisor by managing each EX appliance separately.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp12.png" class="kg-image" alt loading="lazy" width="652" height="65" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp12.png 600w, __GHOST_URL__ /content/images/2021/08/vsp12.png 652w"></figure>

Connect to your vCenter server using the vSphere Client (or Webclient) and add the Steelhead appliance (using the ESXi management address) to vCenter.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp13.png" class="kg-image" alt loading="lazy" width="849" height="624" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp13.png 600w, __GHOST_URL__ /content/images/2021/08/vsp13.png 849w" sizes="(min-width: 720px) 720px"></figure>

At this point you can choose to add a license.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp14.png" class="kg-image" alt loading="lazy" width="668" height="518" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp14.png 600w, __GHOST_URL__ /content/images/2021/08/vsp14.png 668w"></figure>

If you change the license, this is reflected on the management console (web interface) of the Steelhead appliance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp15.png" class="kg-image" alt loading="lazy" width="652" height="94" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vsp15.png 600w, __GHOST_URL__ /content/images/2021/08/vsp15.png 652w"></figure>

After adding the Steelhead appliance to vCenter you can manage it like any other vSphere server.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vsp16.png" class="kg-image" alt loading="lazy" width="511" height="284"></figure>

So there you have it, Steelhead EX version 2, managed by VMware vCenter 5.1. Happy consolidating!

