---
layout: post
title:  "Azure storage through the lens of Microsoft Hardware"
date:   2019-08-01 07:00:00 +0200
categories: Blog, Microsoft, Azure
permalink: /2019/08/01/azure-storage-through-ms-hardware/
---
![blog image]({{ "/assets/msgfx.jpeg" | absolute_url }})

Microsoft Azure provides very compelling cloud storage services, Azure Storage offers a massively scalable object store for data objects (Blob), a file system service for the cloud (Files), a messaging store for reliable messaging (Queues), and a NoSQL store (Tables). Consuming these services from within the public cloud is pretty straightforward and Microsoft can leverage it's high performance backend network to connect these services with your workloads. If you want to consume these services from a hybrid cloud perspective you potentially need to also worry about bandwidth and latency implications, there are of course options like ExpressRoute providing a private connection to Azure directly to your WAN network with bandwidth up to 100 Gbps and lower latency than across the Internet. There are also alternative (or suplementative) ways of working with Azure Storage from within your private datacenter environment or remote office. Microsoft has acquired a lot of companies over the years and some of these companies provided storage related hardware that makes interacting with Azure Storage more straight-forward, some solutions have also been grown organically within Microsoft itself. I wanted to look at 3 of these solutions in this blog post and provide an overview of what they do and how they work.

