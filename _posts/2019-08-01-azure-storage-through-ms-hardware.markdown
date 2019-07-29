---
layout: post
title:  "Azure Storage through the lens of Microsoft's own hardware"
date:   2019-08-01 07:00:00 +0200
categories: Blog, Microsoft, Azure
permalink: /2019/08/01/azure-storage-through-ms-hardware/
---
![blog image]({{ "/assets/msgfx.jpg" | absolute_url }})

Microsoft Azure provides very compelling cloud storage services, Azure Storage offers a massively scalable object store for data objects (Blob), a file system service for the cloud (Files), a messaging store for reliable messaging (Queues), and a NoSQL store (Tables). Consuming these services from within the public cloud is pretty straightforward and Microsoft can leverage it's high performance backend network to connect these services with your workloads. If you want to consume these services from a hybrid cloud perspective you potentially need to also worry about bandwidth and latency implications, there are of course options like ExpressRoute providing a private connection to Azure directly to your WAN network with bandwidth up to 100 Gbps and lower latency than across the Internet. There are also alternative (or suplementative) ways of working with Azure Storage from within your private datacenter environment or remote office. Microsoft has acquired a lot of companies over the years and some of these companies provided storage related hardware that makes interacting with Azure Storage more straight-forward, some solutions have also been grown organically within Microsoft itself. I wanted to look at 3 of these solutions in this blog post and provide an overview of what they do and how they work.

**StorSimple**

Microsoft acquired StorSimple in 2012, situated in Santa Clare at the time, it was founded in 2009 by former Cisco Systems and Brocade Communications Systems executives Ursheet Parikh and Guru Pangal. StorSimple created a cloud storage gateway appliance called Cloud-integrated Storage (CiS) with the idea of integrating the public cloud as a tier together with on-premises block (iSCSI) storage. The appliance provided tiered storage in the appliance itself as well by having SATA disks fronted by SSD, it would dedupe and compress data and use public cloud blob storage as a colder tier.

![blog image]({{ "/assets/storsimple.png" | absolute_url }})

Today the solution is available from Microsoft as a combination of a virtual or physical appliance, Azure Blob storage, and access and data transfer charges depending on use. Now the problem is that data is stored in StorSimple format, making it not readily consumable by other Azure services and applications. To overcome this Microsoft provides something called the StorSimple Data Manager, a gateway after your StorSimple gateway if you will, the Data Manager service provides APIs to extract the StorSimple format data and transform it into other formats such as Azure blobs and Azure Files. There are some limitations and security considerations to take into account, namely the StorSimple Data Manager currently does not work with volumes that are bitlocker encrypted, and some metadata of files (including ACLs) will not be retained in the transformed data, it also only works with NTFS volumes. In terms security considerations you need to take into account that the StorSimple Data Manager needs the service data encryption key to transform from StorSimple format to native format.

**Avere Systems - Azure FXT Edge Filer**

Microsoft acquired Avere Systems in 2018