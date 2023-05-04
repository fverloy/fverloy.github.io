---
layout: post
title: VMware NSX – because hairpins are for old ladies
date: '2013-12-22 23:00:00'
tags:
- sdn
- vmware
---

Server virtualization has increased the amount of server to server network traffic, commonly described as east-west traffic. Let’s assume that you have 2 VM’s living on the same host and both VM’s are in different layer 3 networks, in a traditional network traffic flow would be:

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hairpin1.png" class="kg-image" alt loading="lazy" width="198" height="273"></figure>

essentially “hair pinning” the traffic , meaning the traffic now goes out the VMware host across the network to end up at the same VMware host.

With the NSX distributed Logical Router (DLR) embedded in the VMware kernel the traffic flow would be:

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hairpin2.png" class="kg-image" alt loading="lazy" width="447" height="273"></figure>

Essentially freeing the external network and using the kernel to move traffic between VM1 and VM2, so now east-west traffic is not externalized and you end up with less hops, less traffic, and more speed.

