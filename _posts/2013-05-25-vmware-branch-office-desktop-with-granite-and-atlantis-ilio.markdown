---
layout: post
title: VMware Branch Office Desktop with Granite and Atlantis ILIO
date: '2013-05-25 22:00:00'
tags:
- vmware
---

When using VMware View, or any other VDI based solution for that matter, across a Wide Area Network you need to think about certain limitations inherit in this setup that can potentially limit the user experience for your remote users.

**Running your virtual desktops in the data center and connecting over the WAN.**

If you decide to keep the virtual desktops in the data center and let your users connect remotely, the user experience will be impacted by the amount of bandwidth, the latency, the type of application, and the remoting protocol. In the case of VMware View we are using PCoIP\* across the WAN. With Riverbed Steelhead you can use WAN optimization technology to optimize PCoIP, for example Riverbed Steelhead can optimize printer mappings, drive mappings and USB redirection between the branch office and the data center.

Riverbed Steelhead also enables QoS for PCoIP giving fine bandwidth control and latency prioritization for virtual channels within a PCoIP stream, enabling fine-tuning of traffic including voice, video and display rendering.

**Running your virtual desktops in the branch office.**

To get round the bandwidth and latency issue you could also decide to host the virtual desktop vm’s in the branch office, Riverbed Granite allows you to host the VM’s remotely while the central management components still remain in the data center.

The net result is that you only need bandwidth for PCoIP in the branch office, where it is readily available and is not impacted by latency, and that the SAN hosting the virtual desktops vm’s is less impacted by the IOPS requirement when booting and running the vm’s since they are now running on local blockstore of the Granite appliance in the branch office. All while maintaining central management from the data center.

Now depending on the amount of virtual desktops you need to run in the branch office you could be impacted by the amount of IOPS required, the Steelhead EX appliance in the branch which runs the Granite Edge component has a certain amount of internal disks (HDD for Granite Blockstore, SSD for Steelhead Datastore) which translates to a maximum amount of IOPS available for you virtual desktops. The total amount of IOPS we can serve from Steelhead EX depends on the model.

Let’s assume you are running a big branch office and you require a large amount of IOPS to keep user experience optimal, again you have several options, you could run multiple Steelhead EX + Granite appliances (Riverbed supports up to 80 branch offices connected to a single Granite Core in the data center), or you could use a solution like Atlantis Computing ILIO to leverage your server’s RAM to satisfy your IOPS requirements. Steelhead EX has a certain amount of memory depending on the model or you can use an external VMware host chock full of RAM and connect that to the Granite component in the branch office.

**So how much IOPS do you need to run your virtual desktops?**

A lot has been written about the IOPS requirements for VDI, there are numerous whitepapers and VDI storage calculators out there that will give you some idea of the amount of IOPS you should expect, just be careful with steady state numbers vs booting the vm vs starting an application (application virtualization also helps reduce IOPS requirements here), the idea is that you want to provide a user experience that is at least as good compared to working locally so your users wont revolt. In general the Windows OS will consume as much disk IO or throughput to the hard drive as is available, additionally Windows desktop workloads are write heavy (70-80% writes, 20-30% reads).

VMware itself has also provided a way to alleviate IOPS requirements with its View Storage Accelerator introduced in VMware View 5.1, this is a great addition to limit read IOPS (20-30% in Windows virtual desktops) but as such still leaves us with the write IOPS requirement.

Atlantis ILIO is a virtual machine that is hosted on the same host running the virtual desktops (in our case the Steelhead EX or an external VMware host), it essentially presents the RAM of the host as a datastore where the virtual desktops are run from, providing IOPS from RAM (nanosecond latency as compared to microsecond latency when using flash based arrays or PCI based flash cards).

By using inline deduplication you further limit the amount of IOPS needed from the backend storage (in our case the Granite Edge) since less blocks are being transferred and also limits the amount of RAM required to run the virtual desktops.

**A closer look at Atlantis ILIO.**

First we need to differentiate bewteen persistent and non-persistent VDI desktops. For non-persistent desktops ILIO has had a solution for some time to just run these desktops from RAM without needing persistent storage, when the server dies you just reboot the virtual desktop on another host and start working from there.

With persistent desktops there is a need to write data to persistent storage so your users adjustments aren’t lost after a reboot, with the release of ILO 4.0 this is now possible. I’ll further explore the persistent desktop use case since this is the most interesting one and has the bigger IOPS requirements.

The VM you install on the host is called the session host, this session host hosts the virtual desktops, it exposes the RAM of the host as a NFS or iSCSI mountpoint to which you attach the VMware datastore.

Once data needs to be written to the datastore ILIO (In Line Image Optimization) performs inline deduplication and compression, and Windows I/O optimization. (potentially fixing the I/O blender issue by optimizing random 4K blocks into sequential 64K blocks).

The Replication Host stores the persistent data of multiple Session Hosts and can further deduplicate data (across multiple hosts this time) and as such further reduces the storage requirements of the SAN/NAS. The replication host is responsible for making sure that any changes made to the desktop are saved to a persistent storage device, either SAN or NAS. In order to get the data from the RAM to the Replication host Atlantis uses Fast Replication. (you can run session and replication hosts on the same server if you want). So once you need to restart the virtual desktop on another host, the persistent state of the desktop is retrieved by the replication host from persistent storage. With all these features in the I/O path, Atlantis estimates that they only need around 3GB of persistent storage space per desktop.

See below for a demo of the Atlantis ILIO Persistent VDI 4 user experience

<figure class="kg-card kg-embed-card"><iframe width="200" height="113" src="https://www.youtube.com/embed/ADvL5CoC9ho?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></figure>

Of course this is not the only way to deal with user experience requirements for branch office VDI and a lot of options exist out there but as far as I am concerned this is definitely one of the coolest.

\*PCoIP is not the only option you have as a remoting protocol, you can also use RDP or the HTML5 Blast “client”.

