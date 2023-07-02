---
layout: post
title: SAP HANA on vSphere 5.5
date: '2014-05-07 22:00:00'
tags:
- vmware
permalink: /sap-hana-on-vsphere-5-5/
---

As announced during EMC World 2014 SAP now supports running production SAP HANA on vSphere 5.5 further delivering on VMware’s “Apps love vSphere” premise unveiled at last year’s VMworld conference.

**What is SAP HANA?**

SAP HANA, short for High-Performance Analytic Appliance, is an in-memory computing platform which takes advantage of the latest hardware technologies by combining columnar data storage, massively parallel processing (MPP), and in-memory computing.

It is the next step up from traditional analytics, making sense of all data your business has. Traditional analytics, would take a copy of your data into a operational data store and than load the data into a data warehouse using ETL (extract, transform, and load), which is a set of processes that extract data from your various sources, transforms it to fit your needs (i.e. get quality up to par), and then loads it into the target data warehouse. Here it gets aggregated (to make it manageable/consumable) and indexed (to make it searchable), from here your business users can query the data, and usually the query results are then passed to a calculation engine and displayed into several usable formats/dashboards (duplication of data).

It is in this methodology that the need for an in-memory platform arises, since you need to aggregate data you lose granularity (it is a compromise between speed and data detail), furthermore this typically takes place on spinning disk which usually translates to more hardware for better performance. Because of the effort required updating the data warehouse is less frequent than would be ideal. (latency between data creation and analytics).

So SAP HANA moves this process in-memory which negates the drawbacks previously discussed. Another piece of the puzzle is that HANA support row (like a stack of papers) and column (like a filing cabinet) based data storage. Columnar tables allow for faster queries. Because of it’s in-memory architecture you can load all data into HANA , it also allows for real-time loading (replication) of the data (updating of the data warehouse), the data is also compressed so you need less storage space. You don’t need aggregate tables (although they are supported ofc) nor non-materialized views (no data duplication).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hana1.png" class="kg-image" alt loading="lazy" width="300" height="193"></figure>

**How does vSphere 5.5 provide a solid platform for SAP HANA?**

Since 2012 vSphere 5.1 has had support from SAP to run SAP HANA for non-production scenarios. Now in vSphere 5.5 that support is expanded to include production workloads. Today customers can run and scale single node SAP HANA databases up to 1TB and 32 physical cores (64 virtual cores) per VMware vSphere instance are supported in a virtual environment. The benefit of doing this on vSphere 5.5 is that you can use DRS (automatically balance and manage SAP HANA virtual machines without service disruption), vMotion (live migrate SAP HANA databases from one host to another without downtime), HA (monitor SAP HANA hosts and virtual machines to detect hardware and guest operating system failures and restart on another host if required), etc. to achieve high availability and improved Quality of Service for your mission critical workloads with near native performance. This however does still require you run on SAP certified storage.

**What else what announced at EMC World regarding SAP HANA?**

EMC announced it has acquired DSSD, a start-up that was in stealth-mode, DSSD is working on a server-side flash storage architecture (top of rack, pooled server-memory flash) for in-memory workloads such as SAP HANA. According to USENIX, DSSD is focused on creating the fastest and most reliable storage possible. The idea is to overcome (sic) the performance barriers of PCI-based flash for these new types of workloads. Not much is known yet in terms of products but a small “lifting of the veil” can be found in Chad Sakac’s blog post on the [subject](http://virtualgeek.typepad.com/virtual_geek/2014/05/a-new-5th-branch-in-the-storage-tree-of-life.html?cid=6a00e552e53bd2883301a511b19b84970c).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hana2.jpg" class="kg-image" alt loading="lazy" width="597" height="356"></figure>

DSSD was formed by an amazing all-star team including Bill Moore (previously Chief Engineer of Storage at Sun Microsystems), Jeff Bonwick (team lead of ZFS and Sun Microsystems Fellow), and Andy Bechtolsheim (co-founder of Sun Microsystems and currently also involved in Arista Networks).

You can get some more insights into DSSD’s origins from this EMC World presentation (starting at 32:15) from EMC TV

http://www.emcworld.com/emctv.htm?id=3547506466001

I will be following the DSSD developments with great interest.

