---
layout: post
title: VMware’s Storage Portfolio
date: '2013-10-18 22:00:00'
tags:
- vmware
- storage
permalink: /vmwares-storage-portfolio/
---

I recently joined VMware as an SE, one of the reasons that motivated and influenced my decision to join is a thirst for technology, at VMware you get to work on such a broad and interesting technology stack that anyone is bound to find one or more things that are deeply interesting.

One (out of many) of those things that I find compelling is storage and as such my first blog post as an employee on storage seemed fitting.

In this post I’ll briefly cover VSAN, vFRC, Virsto, and vVols.

I won’t cover vSphere data services like VMFS, VAAI, VASA, Storage vMotion, Storage DRS, VADP, and vSphere Replication here.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmware-sds.png" class="kg-image" alt loading="lazy" width="960" height="720" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmware-sds.png 600w, __GHOST_URL__ /content/images/2021/08/vmware-sds.png 960w" sizes="(min-width: 720px) 720px"></figure>

**VSAN**

VSAN is a software based distributed strorage solution that is integrated directly into the hypervisor (in contrast to running as a virtual appliance, like the vSphere Storage Appliance (VSA)). It uses directly connected storage (a combination of SSD for performance and spinning disks for capacity) and creates a distributed storage layer across multiple (up to 8 hosts in the beta, rumored 16 hosts at GA in 2014) ESXi hosts.

One of the most obvious uses cases for VSAN will be virtual desktops, in fact VSAN will be a part of the new [Horizon View 5.3](https://blogs.vmware.com/euc/2013/10/read-all-about-it-vmware-horizon-view-5-3.html)

For more information on VSAN I suggest heading over to Cormac Hogan’s blog for an [entire series on VSAN](http://cormachogan.com/vsan/), or (AND!) [Duncan Epping’s blog](http://www.yellow-bricks.com/2013/08/26/introduction-vmware-vsphere-virtual-san/)

**vFRC**

vSphere Flash Read Cache takes your direct attached flash resources (SSD and/or PCIe flash devices) on your vSphere host and allows you to utilize them as a dedicated (per VM) read cache for your virtual workloads. Like VSAN this solution is integrated into the hypervisor itself.

For more information on vFRC I recommend heading over to the [Punching Clouds blog](http://www.punchingclouds.com/2013/09/23/vsphere-flash-read-cache-vfrc-action/)

**VVols**

VVols or virtual volumes is in technology preview at the moment (i.e. not available), the idea behind VVols is to make storage VM (VMDK) aware in contrast to VMFS aware (usually LUN or Volume based) for things like clones, snapshots, and replication.

For more information on Virtual Volumes I recommend reading [Duncan Epping’s blog post.](http://www.yellow-bricks.com/2012/08/07/vmware-vstorage-apis-for-vm-and-application-granular-data-management/)

You can also see a (tech preview) demo of vVols by NetApp [here.](https://www.youtube.com/watch?v=jnwYEIpzthE)

**Virsto**

Vmware acquired Virsto earlier this year in an effort to optimize external block storage (i.e. external SAN only) performance and space utilization. It is no secret that block based storage performs best with sequential I/O, but when you start using virtualization you end up with [mostly random I/O](https://vimeo.com/59272921) (if you have more than 1 VM that is). Virsto tries to solve the random I/O issue by intercepting the random I/O, writing it to a serialized log file and later de-staging it to your SAN storage resulting in mostly sequantial data. Virsto is installed as a (set of) virtual appliance. Virsto [presented at Storage Field day 2](http://techfieldday.com/appearance/virsto-presents-at-storage-field-day-2/) before they were acquired by VMware.

