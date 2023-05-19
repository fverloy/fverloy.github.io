---
layout: post
title: Rubrik and Microsoft, rest assured.
date: '2017-09-05 22:00:00'
tags:
- microsoft
- rubrik
---

Rubrik has been working with Microsoft’s solutions in various ways since version 2.3 with the initial support of Microsoft Azure as an archive location for long-term retention data. You can even argue the relationship started earlier than that with support for application consistent backups for Microsoft applications through our own VSS provider and requester (for Windows OS’s on Virtual Machines) since the beginning.

But for this overview, I’ll show the Microsoft specifics from version 2.3 till version 4.0, which is currently GA.

**Azure Blob storage as an archive location**

At the heart of Rubrik sit our SLA policies that govern how we handle your data end-to-end, as part of that SLA policy you can define archival settings, these will determine when we need to start moving data of the local Rubrik cluster and onto Azure Blob storage.

Azure Blob storage is a massively-scalable object storage location for unstructured data, the nice thing about using it as an archive with Rubrik is that because we index all data we can search for specific files/folders in this unstructured data, and only pull back those specific objects instead of needing to re-hydrate the entire fileset. Furthermore the data we sent to Azure Blob storage is encrypted.

<img src="/assets/img/ms1.png">

**Support for physical Windows and Microsoft SQL Server**

In Rubrik CDM version 3.0 we added support for physical Windows and MS SQL server via the use of the backup service. The backup service is a very lightweight agent that you install on your physical Windows server, it runs in user-space and doesn’t require a reboot, once installed it is managed throughout it’s lifecycle via the Rubrik cluster itself. In other words once the backup service is installed you don’t need to manage it manually, it is also valid for all applications, meaning that it provides filesystem backup but also SQL backup capabilities.

<img src="/assets/img/ms2.png">

Rubrik also provides protection and data management of SQL Server databases that are installed across nodes in a Windows Server Failover Clustering, additionally we support Always-On availability groups as wel, Rubrik detects that a database is part of an availability group. In the event of a failover, protection will be transferred to another availability database in the same availability group. When protection is transferred, the Rubrik cluster transfers the existing metadata for history and data protection to the replacement database.

**Rubrik Cloud Cluster in Microsoft Azure**

Since Rubrik CDM v3.2 we support running a 4-node (minimum) cluster in Microsoft Azure. We use the DSv2-series VM sizes, which gives us 4 vCPU, 14GiB RAM, 400 GiB SSD, and a max of 8 HDDs per VM.

<img src="/assets/img/ms3.png">

Through the Rubrik backup service we are able to support native Azure workloads running either Windows or Linux, and Native SQL. Since Cloud Cluster in Azure has the same capabilities we can also archive to another Azure Blob storage location within Azure for long term retention data (i.e. move backup data of the the DSv2 based instances and unto more cost-effective Blob storage), and even replicate to another Rubrik Cluster, either in Azure or another Public Cloud provider, or even to a physical Rubrik cluster on-premises.

**Microsoft SQL Server Live Mount**

One of the coolest, in my humble opinion, new features in version 4.0 of Rubrik CDM is the ability to Live Mount SQL databases. Rubrik will use the SSD tier to rapidly materialize any point-in-time copy of the SQL database and then expose this, fully writetable DB to the SQL server as a new DB. In other words you are not consuming any space on your production storage system to do this.

<img src="/assets/img/ms4.png">

As a SQL DBA you can now easily restore individual objects between the original and copy. The speed of recovery is greatly reduced, and the original backup copy is still maintained as an immutable object on Rubrik safeguarding it against ransomware.

**Microsoft Hyper-V**

Last, but not least, is our support for Microsoft Hyper-V based workloads which we achieve by integrating directly with the Hyper-V 2016 WMI based APIs, so this works independent of the underlying storage layer for the broadest support.

<img src="/assets/img/ms5.png">

We leverage Hyper-V’s Resilient Change Tracking (RCT) to perform incremental forever backups. Older versions of Hyper-V are also supported through the use of the Rubrik backup service.

Independent of the source, be it virtual or physical, we can leverage the same SLA policy based system to avoid having to set individual, and manual backup policies.

