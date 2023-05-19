---
layout: post
title: Azure Storage through the lens of Microsoft's own hardware
date: '2019-08-01 10:57:00'
tags:
- microsoft
- storage
---

Microsoft Azure provides very compelling cloud storage services, Azure Storage offers a massively scalable object store for data objects (Blob), a file system service for the cloud (Files), a messaging store for reliable messaging (Queues), and a NoSQL store (Tables). Consuming these services from within the public cloud is pretty straightforward and Microsoft can leverage it’s high performance backend network to connect these services with your workloads. If you want to consume these services from a hybrid cloud perspective you potentially need to also worry about bandwidth and latency implications, there are of course options like ExpressRoute providing a private connection to Azure directly to your WAN network with bandwidth up to 100 Gbps and lower latency than across the Internet. There are also alternative (or suplementative) ways of working with Azure Storage from within your private datacenter environment or remote office. Microsoft has acquired a lot of companies over the years and some of these companies provided storage related hardware that makes interacting with Azure Storage more straight-forward, some solutions have also been grown organically within Microsoft itself. I wanted to look at 3 of these solutions in this blog post and provide an overview of what they do and how they work.

**StorSimple**

Microsoft acquired StorSimple in 2012, situated in Santa Clare at the time, it was founded in 2009 by former Cisco Systems and Brocade Communications Systems executives Ursheet Parikh and Guru Pangal. StorSimple created a cloud storage gateway appliance called Cloud-integrated Storage (CiS) with the idea of integrating the public cloud as a tier together with on-premises block (iSCSI) storage. The appliance provided tiered storage in the appliance itself as well by having SATA disks fronted by SSD, it would dedupe and compress data and use public cloud blob storage as a colder tier.

<img src="/assets/img/storsimple.png">

Today the solution is available from Microsoft as a combination of a virtual or physical appliance, Azure Blob storage, and access and data transfer charges depending on use. Now the problem is that data is stored in StorSimple format, making it not readily consumable by other Azure services and applications. To overcome this Microsoft provides something called the StorSimple Data Manager, a gateway after your StorSimple gateway if you will, the Data Manager service provides APIs to extract the StorSimple format data and transform it into other formats such as Azure blobs and Azure Files. There are some limitations and security considerations to take into account, namely the StorSimple Data Manager currently does not work with volumes that are bitlocker encrypted, and some metadata of files (including ACLs) will not be retained in the transformed data, it also only works with NTFS volumes. In terms security considerations you need to take into account that the StorSimple Data Manager needs the service data encryption key to transform from StorSimple format to native format.

**Avere Systems - Azure FXT Edge Filer**

Microsoft acquired Avere Systems in 2018, the company was based in Pittsburgh, Pennsylvania and founded in 2008 by Ronald Bianchini, Michael Kazar, and Dan Nydick. They released their first FXT Series storage appliances in November 2009. The FCT Edge filer also focuses on providing cloud-integrated hybrid storage that is designed to work with your existing network-attached storage (NAS) system living on-premises. The Edge Filer (caching appliance, available in a performance or capacity model) sits on-premises, it provides a combination of software and hardware to deliver high throughput and low latency for hybrid storage infrastructure supporting high-performance computing (HPC) workloads.

<img src="/assets/img/model.png">

Like the StorSimple appliance the FXT also has built-in tiering, but in this case it is provided by DRAM (up to 1.5TB) and NVMe SSD to support big data workloads. It presents itself as a single mountpoint, spanning heterogeneous storage, to your workloads using multuple protocols (including NFSv3, SMBv2, Azure Blob Storage, and Amazon S3). It is sold on a subscription basis.

**Azure Data Box family**

The Data Box solutions are available for both off-line and on-line data transfer into Azure Storage. If you want to move large amounts of data to Azure and you are limited by time, network availability or costs you can look at the Data Box for off-line transfer. You can use tools like Robocopy to get your data onto either the Data Box Disk (8TB SSD, comes in packs of 5, e.g. 40TB), the Data Box, a ruggedised device, with 100 TB of capacity, or the Data Box Heavy, a ruggedised, self-contained device designed to lift 1 PB of data to the cloud. There is typically an data import fee and shipping fee after wich you start consuming Azure Storage as as service.

<img src="/assets/img/databox.png">

For online operations you have the choice of the Data Box Edge, which, again is a on-premises physical appliance that transfers data to and from Azure. It analyses, processes and transforms your on-premises data before uploading it to the cloud using AI-enabled edge compute capabilities or a virtual appliance called the Data Box Gateway. In terms of cost this is again based on a subscription fee and in case of the Edge also a shipping fee of course.

<img src="/assets/img/diagram-databox.svg">

So Microsoft provides a couple of options to either get data into Azure Storage or have Azure Storage function as a storage tier for certain applications running in a Hybrid scenario proving that you cannot cheat your way around the speed of light.

