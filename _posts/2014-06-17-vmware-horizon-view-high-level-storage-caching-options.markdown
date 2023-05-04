---
layout: post
title: VMware Horizon View – High Level Storage Caching Options
date: '2014-06-17 22:00:00'
tags:
- vmware
- storage
---

 **What are we trying to solve?**

Probably the most quoted statement about VDI is that solving the storage problem is hard. Why?

Replacing your users’ physical desktops, which have local disks, with a virtual machine in the datacenter requires you to think about cost (enterprise storage is probably more expensive than consumer grade disks in a pc) and performance (if you have 10.000 PC’s being migrated to VDI are you going to have 10.000 HDD’s in your datacenter storage box?).

**How much IOPS do you really need?**

If you look at the IOPS a single HDD in a PC can deliver you should expect something in the 75 to 100 IOPS range for a single 7.200 rpm SATA drive

**But what if there are more IOPS available?**

The figure below looks at what Windows would request if it had unlimited IOPS available. As you can see a desktop search reaches up to 900 read IOPS and opening a 5MB PowerPoint file pushes 500 and more read IOPS.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon1.png" class="kg-image" alt loading="lazy" width="1024" height="568" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/horizon1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/horizon1.png 1000w, __GHOST_URL__ /content/images/2021/08/horizon1.png 1024w" sizes="(min-width: 720px) 720px"></figure>

In order to limit this (expected) behavior we sometimes disable certain services/functionality in a virtual desktop environment, this then becomes a trade-off between user experience versus cost of the back-end infrastructure. Seeing that a typical desktop only has about 100 IOPS available you can of course argue that a user is waiting on the spinning disk in a physical environment anyway so he doesn’t expect it to be faster. (not really the best design principle imho).

The picture above also indicates what causes IOPS spikes (index-searching, opening applications, AV-scanning,…) so we should incorporate these in our VDI design.

**Read / Write ratio**

A typical pattern (it all depends on use cases of course) we see, after user logon, is around 70 to 80% write IOPS and 20 to 30% read IOPS for a virtual desktop. Because we have multiple virtual machines accessing the same back end storage we also run into the storage IO blender issue which causes the write IOs to be random and typically 4K in block size. When we are using a storage array we also need to take into account the RAID write-penalty, i.e. you have multiple writes taking place for each block depending on the protection level, the table below gives an overview.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon2.png" class="kg-image" alt loading="lazy" width="1024" height="332" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/horizon2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/horizon2.png 1000w, __GHOST_URL__ /content/images/2021/08/horizon2.png 1024w" sizes="(min-width: 720px) 720px"></figure>

**Storage Caching options**

Note that the feasibility of any of these techniques depends on the type of VDI environment you are architecting, and these are some examples of what is possible, the list is no way exhaustive.

VMware itself has some options to alleviate some of the issues related to storage in a VDI environment like CBRC/View Storage Accelerator and VSAN.

**Memory based read IOPS caching**

_VMware View Storage Accelerator_

The vSphere host Content Based Read Cache (CBRC) feature is known as View Storage Accelerator in Horizon View. It addresses a.o. things the initial IO required during booting of the virtual desktops by caching, completely transparent to the virtual desktop, the most common blocks in memory on the host, the maximum amount of memory that can be allocated to CBRC is 2GB per host.

For a Windows 7 guest single vSphere host boot storm the effect of CBRC looks something like the picture below.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon3.jpg" class="kg-image" alt loading="lazy" width="1024" height="292" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/horizon3.jpg 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/horizon3.jpg 1000w, __GHOST_URL__ /content/images/2021/08/horizon3.jpg 1024w" sizes="(min-width: 720px) 720px"></figure>

Memory based read/write IOPS caching

_Atlantis ILIO_

Atlantis Computing takes the memory caching approach a step further by having the entire virtual desktop run in memory on the host, this way both reads and writes are served by the vSphere host greatly improving performance of the guest. To get round the limited capacity of memory they use in-line deduplication. Persistent storage is achieved through fast replication to a separate replication host. Atlantis claim 80% IOPS reduction on the back-end storage while only needing 5% of the original capacity. The solution can be coupled with vSphere CBRC to further increase performance acceleration.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon4.png" class="kg-image" alt loading="lazy" width="300" height="168"></figure>

_Liquidware Labs Flex-IO_

Flex-IO provides IOPS acceleration for non-persistent desktops, like Atlantis ILIO it leverages the memory of the vSphere host for R/W operations and uses compression to alleviate capacity requirements.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon5.png" class="kg-image" alt loading="lazy" width="300" height="181"></figure>

Flex-IO can also be coupled with vSphere CBRC to increase the performance gains.

_Infinio_

Infinio Accelerator’s distributed storage architecture uses two vCPUs and 8 GB of RAM from each server to provide a distributed, de-duped cache. This can then also be leveraged for VDI workloads. As far as I know Infinio exposes NAS services only at the moment. The product is aimed at easy install and easy removal without reconfiguration or any downtime, it connects to the vMotion network and optimizes NAS traffic (assuming it runs over the same vmkernel port) on the fly.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon6.png" class="kg-image" alt loading="lazy" width="300" height="171"></figure>

Infinio recenlty hired the former EUC CTO from VMware Scott Davis.

**Host (server) based flash caching**

_VMware vFlash Read Cache_

At the moment of writing Horizon View does not support VMware Flash Read Cache.

_Proximal Data_

Proximal Data Autocache is a vSphere works as a vSphere attached solution where it inspects all I/O from all virtual machines and places hot I/O into a local PCIe flash card or solid-state disk (SSD). By design, AutoCache is a read cache with write through and write around semantics. It’s algorithm supplies hot reads back to the VMs that request them, without requiring any sysadmin configuration to modify the deployed storage or VM infrastructure. AutoCache creates a universal cache for all VMs that adapts automatically to the changing workloads of the environment, shifting cache resources on the fly to VMs that most need them. Cold data is then moved to the storage array.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon7.png" class="kg-image" alt loading="lazy" width="215" height="299"></figure>

_Pernixdata FVP_

FVP virtualizes flash and RAM across servers to create a clustered pool of high-speed resources that accelerate reads and writes to shared storage. This should decrease Horizon View latency by up to 80%.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/horizon8.png" class="kg-image" alt loading="lazy" width="1024" height="489" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/horizon8.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/horizon8.png 1000w, __GHOST_URL__ /content/images/2021/08/horizon8.png 1024w" sizes="(min-width: 720px) 720px"></figure>

FVP can also be couple with vSphere CBRC to further accelerate performance.

**PCIe Flash based host storage**

_Virident/Fusion-IO/XtermSF/…_

In these types of setups the vSphere host is populated with Flash cards that are used to offload , this is usually limited to non-persistent desktops requiring replica’s of the golden image to be placed on all local flash datastores.

**Storage based caching options for VDI**

With the advent of All Flash Arrays using in-line deduplication putting persistent desktops on an AFA has become a reality from a cost perspective.

VMware itself has recently GA’ed VSAN as an option for Horizon View giving you a mix of performance (SSD based R/W cache, no actual persistent data is stored on the SSD) and price conscious capacity (HDD) for a virtual desktop environment.

Other storage vendor options include, like on the server side, various caching options to front-end spinning disk, or have a tiered solution that intelligently moves data based on performance requirements.

**Conclusion**

Depending on your use case and budget you can pick and choose the best option for your specific environment. Remember that different parts of your user population can require a different setup with different capabilities, a sure-fire way to fail any VDI deployment is by treating all your end-users the same.

