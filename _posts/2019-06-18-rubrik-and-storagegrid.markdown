---
layout: post
title:  "Rubrik and NetApp StorageGRID"
date:   2019-06-18 07:00:00 +0200
categories: Blog, Rubrik, NetApp
permalink: /2019/06/18/rubrik-and-storagegrid/
---
![blog image]({{ "/assets/baconandeggs.jpeg" | absolute_url }})

Rubrik and NetApp recently announced support for NetApp StorageGRID as an Archive target. Some things, like bacon and eggs, are just better together which is what I wanted to focus on in this blogpost. Additionally I wanted to provide some background and a deeper look at the NetApp StorageGRID solution.

You can find the joint data sheet [here](https://www.netapp.com/us/media/sb-3990.pdf)

**Rubrik Archive Target**

First things first, what do we mean when we talk about an Archive target at Rubrik? Rubrik delivers a software fabric that can run in and across multiple different locations, could be on-premises on our own hardware or OEM hardware, in the branch as a virtual appliance, in the public cloud,... When we backup data this gets stored on our filesystem called Atlas. The retention on Atlas is determined by SLA policies, these also determine after which period data gets offloaded to another storage target, this target is considered an Archive location from Rubrik's point of view. The location in this case is NetApp StorageGRID.

![blog image]({{ "/assets/rubrikstoragegrid.png" | absolute_url }})

**NetApp StorageGRID**

