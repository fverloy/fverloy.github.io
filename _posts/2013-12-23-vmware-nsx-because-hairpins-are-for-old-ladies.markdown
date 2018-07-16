---
layout: post
title:  "VMware NSX – because hairpins are for old ladies"
date:   2013-12-23 14:21:43 +0200
categories: VMware, NSX
permalink: /2013/12/23/vmware-nsx-because-hairpins-are-for-old-ladies/
redirect_from:
  - /2013/12/23/vmware-nsx-because-hairpins-are-for-old-ladies/amp/
---
Server virtualization has increased the amount of server to server network traffic, commonly described as east-west traffic. Let’s assume that you have 2 VM’s living on the same host and both VM’s are in different layer 3 networks, in a traditional network traffic flow would be:

![blog image]({{ "/assets/hairpin1.png" | absolute_url }})

essentially “hair pinning” the traffic , meaning the traffic now goes out the VMware host across the network to end up at the same VMware host.

With the NSX distributed Logical Router (DLR) embedded in the VMware kernel the traffic flow would be:

![blog image]({{ "/assets/hairpin2.png" | absolute_url }})

Essentially freeing the external network and using the kernel to move traffic between VM1 and VM2, so now east-west traffic is not externalized and you end up with less hops, less traffic, and more speed.