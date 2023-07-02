---
layout: post
title: Using Windows Azure Cloud Storage with Riverbed Whitewater
date: '2012-08-10 22:00:00'
tags:
- azure
- riverbed
- backup
permalink: /using-windows-azure-cloud-storage-with-riverbed-whitewater/
---

Riverbed Whitewater allows you to connect your on-premise backup and archive environment to a public cloud storage provider like Microsoft’s Windows Azure.

Note that at this stage Whitewater is not meant to be a gateway for primary storage in the cloud, but rather provides an optimized (using network optimization, de-duplication and encryption) way to store your backup and archive data in the public cloud.

It connects your existing backup mechanism (Riverbed supports most backup vendors as seen in the picture below) with a number of cloud storage systems.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwazure1.png" class="kg-image" alt loading="lazy" width="490" height="295"></figure>

Also note that the Windows Azure Platform is a much broader public cloud platform than just cloud storage and provides a Platform-as-a-Service (PaaS) for customers to run applications (web apps, middle-tier/worker apps, and stand alone VMs) in the public cloud.

**Windows Azure Platform**

Windows Azure is made up of different building blocks namely:

- Windows Azure Compute (RDFE, Fabric Controller, Networking)
- Windows Azure Storage, consisting of BLOBs, Tables, and Queues (more on this later)
- Windows Azure CDN (more on this later)
- SQL Azure
- AppFabric PaaS Middleware Services (AppFabric Caching, Access Control Server, Service Bus)
<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwazure2.jpg" class="kg-image" alt loading="lazy" width="811" height="449" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwazure2.jpg 600w, __GHOST_URL__ /content/images/2021/08/wwazure2.jpg 811w" sizes="(min-width: 720px) 720px"></figure>

We are going to focus on the storage piece, if you want to learn more about the Windows Azure Platform I highly recommend watching some of Mark Russinovich’s Azure sessions, you can find these recordings on channel9 (just Bing for Azure).

The advantages of using Windows Azure storage are:

- Fault-tolerance: Windows Azure Blobs, Tables and Queues stored on Windows Azure are replicated three times in the same data centre for resiliency against hardware failure. No matter which storage service you use, your data will be replicated across different fault domains to increase availability
- Geo-replication: Windows Azure Blobs and Tables are also geo-replicated between two data centres 100s of miles apart from each other on the same continent, to provide additional data durability in the case of a major disaster, at no additional cost.
- REST and availability: In addition to using Storage services for your applications running on Windows Azure, your data is accessible from virtually anywhere.
- Content Delivery Network: With one-click, the Windows Azure CDN (Content Delivery Network) dramatically boosts performance by automatically caching content near your customers or users.
- Price: No brainer

**Windows Azure BLOBs, Tables, and Queues**

Blobs and Tables reside within a Windows Azure Storage account. A single Windows Azure Storage account can hold up to 100 TB of data. If you need to store more data, then you can create additional storage accounts. Within a storage account, you can store data in Blobs and Tables. A storage account can also contain Queues. Here is a brief description for Blobs, Tables, and Queues:

- Blobs (Binary Large Objects). Blobs are for storing individual data items (like files) which can be large in size. A single Blob typically contains a document (xml, docx, pptx, xlxs, pdf, etc.), a picture, a song or a video.
- Tables. Tables contain large collections of property-bag state (called entities) such as customer information, order data, news feed items. Tables sort their entities and can return a filtered subset of the entities.
- Queues. Windows Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS Whitewater uses the Binary Large Object (BLOB) Service, an easy way to store text or binary data within Windows Azure, we connect to the BLOB storage service using REST APIs over port 443.
<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwazure3.jpg" class="kg-image" alt loading="lazy" width="635" height="315" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwazure3.jpg 600w, __GHOST_URL__ /content/images/2021/08/wwazure3.jpg 635w"></figure>

**Public Cloud Data Security**

One of the key security tactics you want to employ when storing your data in a public cloud (where everything is shared to take advantage of economies of scale) is making sure your data remains private by using encryption, the data we send to Windows Azure is encrypted in flight and at rest by using SSLv3 and AES 256.

**Riverbed Whitewater CSG**

The Riverbed Whitewater Cloud Storage Gateway presents itself to your existing backup infrastructure as either CIFS or NFS, so integrating it into your environment should be a snap.

Whitewater dedupes the date inline (using Riverbed’s very efficient and effective network optimization and storage deduplication method) , we expect a dedupe ratio between 10 and 30x for backup data, meaning that a single Whitewater CSG can represent up to 1PB of cloud storage. Whitewater appliances use inline, variable-length, byte-level deduplication that maximizes the ability to identify duplicate data and reduce storage requirements.

The advantages of using Riverbed Whitewater CSG are:

- Cost savings: Whitewater gateways reduce the management overhead, network bandwidth, and storage that are required for data protection.
- Simplicity: Whitewater allows you to replace tape based infrastructures so you have a full lights-out solution requiring no ongoing tape management overhead, in addition to improved recovery point objectives (RPOs) and recovery time objectives (RTOs).
- Drop-in DR: Because data is stored in the cloud, there is no need to provision and manage a second site for DR purposes. Should the main datacentre go down, a virtual or physical Whitewater can be deployed to a third-party site and, once the encryption key and cloud account information is loaded, restores can commence immediately. In addition, the virtual Whitewater could be spun up in the cloud itself, where the cloud could be used as the DR site.
- Cloud effects: Whitewater also unlocks all the benefits of the cloud for IT administrators in one simple-to deploy appliance, including pay-as-you grow cost management, instant capacity scaling to accommodate demand fluctuations, greatly simplified capital budgeting, and access-anywhere flexibility to maximize IT efficiency.

The Whitewater appliance also uses a local disk cache to store to most recent backups, seeing that most restores require the latest backup data this significantly speeds up recovery time. At the same time we also store this local cache data on Windows Azure storage for data protection purposes. The amount of local cache, the ingest ratio (how much data we can take in at the LAN side) and supported cloud storage amount depend on the model of Whitewater appliance.

_Azure: of or having a light, purplish shade of blue, like that of a clear and unclouded sky._ _Whitewater: of or moving over or through rapids._

