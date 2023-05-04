---
layout: post
title: Intel and Micron 3D XPoint
date: '2016-02-12 23:00:00'
tags:
- intel
- storage
---

 **Introduction**

My day job is in networking but I do consider myself (on the journey to) a full stack engineer and like to dabble in lot’s of different technologies like, I’m assuming, most of us geeks do. Intel and Micron have been working on a seeming breakthrough that combines memory and storage in one non-volatile device that is cheaper than DRAM (typically computer memory) and faster than NAND (typically a SSD drive).

**3D Xpoint**

3D Xpoint, as the name implies, is a crosspoint structure, meaning 2 wires crossing each other, with “some material\*” in between, it does not use transistors (like DRAM does) which makes it easier to stack (hence the 3D) —\> for every 3 lines of metal you get 2 layers of this memory.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/3dxpoint.png" class="kg-image" alt loading="lazy" width="745" height="671" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/3dxpoint.png 600w, __GHOST_URL__ /content/images/2021/08/3dxpoint.png 745w" sizes="(min-width: 720px) 720px"></figure>

The columns contain a memory cell (the green section in the picture above) and a selector (the yellow section in the picture above), connected by perpendicular wires (the greyish sections in the picture above), allowing you to address each column individually by using one wire at the top and one wire at the bottom. These grids can be stacked 3 dimensionally to maximise density. The memory can be accessed/modified by sending varied voltage to each selector, in contrast DRAM requires a transistor at each memory cell to access or modify it, this results in 3D XPoint being 10x more dense that DRAM and 1000x faster than NAND (at the array level, not at the individual device level).

3D XPoint can be connected via PCIe NVMe and has little wear effect over it’s lifetime compared to NAND. Intel will commercialise this in it’s Optane range both as an SSD disk and as DIMMS. (The difference between Optane and 3D XPoint is that 3D XPoint refers to the type of memory and Optane includes the memory and a controller package).

**1000x faster, really?**

In reality Intel is getting 7x performance compared to a NAND MLC SSD (on NVMe) today (at 4kB read), that is because of the inefficiencies of the storage stack we have today.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/speed.png" class="kg-image" alt loading="lazy" width="1108" height="616" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/speed.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/speed.png 1000w, __GHOST_URL__ /content/images/2021/08/speed.png 1108w" sizes="(min-width: 720px) 720px"></figure>

The I/O passes through the filesystem, storage stack, driver, bus/platform link (transfer and protocol i.e. PCIe/NVMe), controller firmware, controller hardware (ASIC), transfer from NAND to the buffers inside the SSD, etc. So 1000x is a theoretical number (and will show up on a lot of vendor marketing slides no doubt) but reality is a bit different.

So focus is and has been on reducing latency, for example work that has been done by moving to NVMe already reduced the controller latency by roughly 20 microseconds (no HBA latency and the command set is much simpler).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/iops.png" class="kg-image" alt loading="lazy" width="1108" height="670" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/iops.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/iops.png 1000w, __GHOST_URL__ /content/images/2021/08/iops.png 1108w" sizes="(min-width: 720px) 720px"></figure>

The picture above shows the impact of the bus technology, on the left side you see AHCI (SATA) and on the right NVMe, as you see there is a significant latency difference between the two. NVMe also provides a lot more bandwidth compared to SATA (about 6x more on PCIe NVMe Gen3 and more than 10x on Gen4).

Another thing that is hindering the speed improvements of 3D XPoint is replication latency across nodes (it’s storage so you typically want redundancy). To address this issue work is underway on things like “NVMe over Fabrics” to develop a standard for low overhead replication. Other improvements in the pipe are work on optimising the storage stack, mostly on the OS and driver level. For example, because the paging algorithms today were not designed with SSD in mind they try to optimise for seek time reduction etc, things that are irrelevant here so reducing paging overhead is a possibility.

They are also exploring “Partial synchronous completion”, 3D XPoint is so fast that doing an asynchronous return, i.e. setting up for an interrupt and then waiting for interrupt completion takes more time than polling for data. (we have to ignore queue depth i.e. assume that it will be 1 here).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/polling.png" class="kg-image" alt loading="lazy" width="1108" height="572" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/polling.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/polling.png 1000w, __GHOST_URL__ /content/images/2021/08/polling.png 1108w" sizes="(min-width: 720px) 720px"></figure>

**Persistent memory**

One way to overcome this “it’s only 7x faster problem”, altogether is to move to persistent memory. In other words you skip over they storage stack latency by using 3D XPoint as DIMMs, i.e. for your typical reads and writes there is no software involved, what little latency remains is now caused entirely by the memory and the controller itself.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/latency3dx.png" class="kg-image" alt loading="lazy" width="1108" height="727" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/latency3dx.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/latency3dx.png 1000w, __GHOST_URL__ /content/images/2021/08/latency3dx.png 1108w" sizes="(min-width: 720px) 720px"></figure>

To enable this “storage class memory” you need to change/enable some things, like a new programming model, new libraries, new instructions etc. So that’s a little further away but it’s being worked on. What will probably ship this year is the SSD model (the 7x improvement) which is already pretty cool I think.

- It’s not really clear right now what those materials entail exactly which is part of it’s allure I guess 😉
