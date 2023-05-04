---
layout: post
title: Rubrik and NetApp StorageGRID
date: '2019-06-17 22:00:00'
tags:
- backup
- rubrik
- netapp
---

Rubrik and NetApp recently announced support for NetApp StorageGRID as an Archive target. Some things, like bacon and eggs, are just better together which is what I wanted to focus on in this blogpost. Additionally I wanted to provide some background and a deeper look at the NetApp StorageGRID solution.

You can find the joint data sheet [here](https://www.netapp.com/us/media/sb-3990.pdf)

**Rubrik Archive Target**

First things first, what do we mean when we talk about an Archive target at Rubrik? Rubrik delivers a software fabric that can run in and across multiple different locations, could be on-premises on our own hardware or OEM hardware, in the branch as a virtual appliance, in the public cloud,… When we backup data this gets stored on our filesystem called Atlas. The retention on Atlas is determined by SLA policies, these also determine after which period data gets offloaded to another storage target, this target is considered an Archive location from Rubrik’s point of view. The location in this case is NetApp StorageGRID.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/rubrikstoragegrid.png" class="kg-image" alt loading="lazy" width="1956" height="1198" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/rubrikstoragegrid.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/rubrikstoragegrid.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/rubrikstoragegrid.png 1600w, __GHOST_URL__ /content/images/2021/08/rubrikstoragegrid.png 1956w" sizes="(min-width: 720px) 720px"></figure>

NetApp StorageGRID supports S3 as a way of interfacing with their platform, Rubrik will leverage this to connect to the Archive location and have it’s intelligent policy engine tier data off to NetApp StorageGRID to optimize for cost and redundancy.

(Note: StorageGRID also supports NFS as an interface via what they call a NAS Bridge)

**NetApp StorageGRID**

The StorageGRID solution is based on an acquisition NetApp did of Bycast in 2010, Bycast was founded in 1999. The solution is a Scale-Out Object Storage system, it is software-defined in that you can run it as Virtual Machines, as Docker Containers, or on NetApp Engineered Appliances. It presents the data in a single-namespace and can stretch that single-namespace across 16 (geographically dispersed) locations, potentially having certain locations served by VMs and other as Appliances, etc.

Architecturally the solution runs 2 Admin Nodes (when these are not available the Grid continues to function however), the nodes also provide GUI access (consuming the APIs of the Grid itself), capacity and performance is provided through the Storage Nodes, scale-out across GEOs can be asymmetric, meaning you don’t need a exact similar amount of Nodes in every location. and everything is IP interconnected.

When an object is PUT into the Grid the system automatically makes 2 copies of the data and 3 copies of the metadata (in a distributed NoSQL Database) before acknowledging the write has occurred. Once this operation is done it’s Policy Engine (called ILM or Information lifecycle management) determines where this data needs to go and how it needs to be protected (replicas vs erasure coding). The system however guarantees read after write consistency for that data across sites by redirecting (if needed) the metadata lookup to a location that already has it.

The ILM Policy Engine can be used to set object granular policies, it allows operations like changing the redundancy of data over time (remove a copy from a certain site for example), tier to another location (to cloud storage using a NetApp StorageGRID Archive Node for example) etc. Since the Storage Nodes do not need to be homogeneous this also means you can tightly control performance this way. The policy targets an internal construct called a Pool, a group of Storage Nodes in the same physical location can be a member of the same pool, but a Storage Node can also belong to multiple pools, so if you have high performance storage nodes as part of a pool those can be targeted differently for some policies as well.

**Rubrik and NetApp StorageGRID for Direct NAS Archiving**

Besides providing automated data lifecycle management through Rubrik’s control plane while using StorageGRID as a scale-out object-based archive target we can also combine both using Rubrik’ Direct NAS Archive capabilities. Rubrik has no interest in being a storage company, partners like NetApp have already solved this, so when we manage and protect large NAS environments we do not land the data on Rubrik, instead we ingest and index the data, wrap it with metadata, and directly pass it through to a storage layer, in this case NetApp StorageGRID. This gives you Rubrik as a data control plane (sic) and NetApp as a data plane in an ideal scenario for both management, cost optimization and redundancy.

