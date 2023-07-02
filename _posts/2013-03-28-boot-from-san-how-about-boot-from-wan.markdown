---
layout: post
title: Boot from SAN? How about boot from WAN!
date: '2013-03-28 23:00:00'
tags:
- riverbed
- storage
- networking
permalink: /boot-from-san-how-about-boot-from-wan/
---

If you ask an IT administrator to draw his/her ideal IT architecture you’ll probably get a picture of a big consolidated datacenter with all remote branches connecting to it without any locally installed servers/storage.

Or not, since this would not deliver the local performance needed to keep your branch office users happy, and if the WAN connection went down nothing would work anymore.

Cue Riverbed Granite.

With Granite we can use your consolidated infrastructure in the datacenter to power your branch remotely while still delivering on centralized management and provisioning.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite1.jpg" class="kg-image" alt loading="lazy" width="800" height="321" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite1.jpg 600w, __GHOST_URL__ /content/images/2021/08/granite1.jpg 800w" sizes="(min-width: 720px) 720px"></figure>

The Granite Core appliance in the datacenter will “project” a iSCSI LUN from your centralized SAN to the Steelhead EX (with Granite license) in the branch office making data, applications, and virtual machines available locally.

There are many use cases for this deployment model but in this blogpost I wanted to call out booting a virtual server across a high latency, low bandwith WAN link.

The problem when wanting to boot an entire virtual windows server from the datacenter across this limited bandwith link with substantial latency is that you simply would need to transfer too much data (i.e. how big is your VM of windows server 2008?), it would either take too long, or time out altogether.

What Granite is able to do is make a correlation between the file system and the block based storage (supported filesystems today are NTFS and VMFS) so we can predicatively transfer the blocks of storage from the SAN to power the filesystem in the branch without needing to transfer the entire image. (i.e. what blocks do we need to boot windows server 2008). Combined with WAN optimization using the Steelhead appliances this results in being able to comfortably boot a server and use it locally.

In this case I will use an external ESXi host in the branch and make an iSCSI connection to the “projected” LUN on the Steelhead EX appliance. It is also possible to use ESXi on top of the Steelhead EX appliance so all functionality is residing in one box (the branch office box if you will).

In terms of iSCSI functionality, the Steelhead EX appliance will act as the iSCSI initiator towards the Granite Core which will act as the iSCSI target (the local LUN in the datacenter), then the Steelhead EX will provide the iSCSI target (the “projected” LUN) towards the iSCSI initiator from the ESXi host.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite2.png" class="kg-image" alt loading="lazy" width="787" height="516" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite2.png 600w, __GHOST_URL__ /content/images/2021/08/granite2.png 787w" sizes="(min-width: 720px) 720px"></figure>

On the Granite Core appliance I have a LUN called CDrive which contains the VM image of my Windows Server 2008, the LUN has been exposed to the IQN of the ESXi host.

I then need to connect my Granite Edge (Steelhead EX) to the Granite core.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite3.png" class="kg-image" alt loading="lazy" width="891" height="463" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite3.png 600w, __GHOST_URL__ /content/images/2021/08/granite3.png 891w" sizes="(min-width: 720px) 720px"></figure>

Once the connection is complete the GUI will show both ends connected and healthy

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite4.png" class="kg-image" alt loading="lazy" width="620" height="393" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite4.png 600w, __GHOST_URL__ /content/images/2021/08/granite4.png 620w"></figure>

Next I will bring the CDrive LUN online on the core so it can be used from the edge.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite5.png" class="kg-image" alt loading="lazy" width="751" height="517" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite5.png 600w, __GHOST_URL__ /content/images/2021/08/granite5.png 751w" sizes="(min-width: 720px) 720px"></figure>

Then from my ESXi host in the branch office I need to connect to the iSCSI LUN on the Granite Edge appliance (make sure the IQN from the ESXI host matches the configured initiator on the Edge).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite6.png" class="kg-image" alt loading="lazy" width="499" height="581"></figure>

Then rescan the HBA on the ESXi host and simply add the storage to the host.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite7.png" class="kg-image" alt loading="lazy" width="739" height="195" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite7.png 600w, __GHOST_URL__ /content/images/2021/08/granite7.png 739w" sizes="(min-width: 720px) 720px"></figure>

You can then browse the datastore and add the Windows Server VM to the inventory and place it on the host.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite8.png" class="kg-image" alt loading="lazy" width="959" height="418" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite8.png 600w, __GHOST_URL__ /content/images/2021/08/granite8.png 959w" sizes="(min-width: 720px) 720px"></figure>

Now we are ready to boot our Windows Server VM across the WAN (in this case the Core is in San Francisco and the ESXi host is in Antwerp).

The report on the Edge will give some insight into what amount of data is being pulled across the WAN to actually boot the server. (what data is already locally available vs what data needs to be pushed over the WAN).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite9.png" class="kg-image" alt loading="lazy" width="1108" height="588" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite9.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/granite9.png 1000w, __GHOST_URL__ /content/images/2021/08/granite9.png 1108w" sizes="(min-width: 720px) 720px"></figure>

In this case it took about 5 minutes to boot (I have 166ms latency and getting around 512Kbps throughput) the Windows Server 2008 VM.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite10.png" class="kg-image" alt loading="lazy" width="952" height="481" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite10.png 600w, __GHOST_URL__ /content/images/2021/08/granite10.png 952w" sizes="(min-width: 720px) 720px"></figure>

All operations (write new data etc.) you perform inside the VM are now committed locally to the Granite Edge (LAN speed read/write) and will be asynchronously synced back to the Core. You can track the amount of uncommitted data at the edge in the reports.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/granite11.png" class="kg-image" alt loading="lazy" width="1108" height="595" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/granite11.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/granite11.png 1000w, __GHOST_URL__ /content/images/2021/08/granite11.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Because of the integration with the SAN we will first commit all data back to the core before allowing manipulation in the datacenter. I.e. you take a snapshot on your SAN of the “projected” LUN, we will first transfer the data from the Edge and then allow the snapshot to go through.

