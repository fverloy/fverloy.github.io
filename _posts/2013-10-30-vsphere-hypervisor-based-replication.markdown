---
layout: post
title: vSphere hypervisor-based replication
date: '2013-10-30 23:00:00'
tags:
- vmware
permalink: /vsphere-hypervisor-based-replication/
---

vSphere Replication is VMware’s hypervisor (as opposed to storage array) asynchronous (minimum 15 minutes) based replication solution that works at the virtual machine (VMDK) level whereas storage array replication usually works at the datastore (VMFS) level. (One of the reasons why vVols will be interesting going forward).

Since vSphere replication happens at the host level it is storage agnostic meaning that you can replicate between between different storage arrays, for instance from your enterprise level array in production to a cheaper array in the DR site, or even from a storage array to local disk. Obviously VSAN could also be a good replication partner use case.

Within the RPO you as an admin specify (again 15 minutes is the minimum here, and you can set it per VMDK) the vSphere replication engine looks at the changed blocks that need to be sent over the network to the other site.

Now sending (potentially) lot’s of data over a network can be tricky depending on bandwidth and latency. vSphere replication uses CUBIC TCP to better cope with high bandwidth links where you potentially have lot’s of bandwidth but the round trip time is high. It is mainly about optimizing the congestion control algorithm in order to avoid low link utilization because of the way TCP generally works (see my post on TCP throughput for more info).

In terms of bandwidth requirements you need to take into account the way vSphere replication works on a per VM model, it will not consume all the bandwidth that is available but depending on the data set, change rate and your RPO will intelligently decide what to replicate and when. There is a [KB article](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2037268) that goes into detail about calculating the bandwidth requirements.

It is included with vSphere Essentials Plus or higher. You need to download a single virtual appliance that performs a dual role. (the vSphere Replication Management Server (VRMS) and the vSphere Replication Server(VRS)).

You can only have one VRMS per site but you can have multiple VRS’s (up to 10). The VRMS performs configuration management and the VRS manages replica instances.

Initially you select the source and target location and VR will scan the entire disk on both sites (you could already have a copy of the VM sitting in the target site if you want), and generate a checksum for each block, it then compares both and figures out what needs to be initially replicated to get both sites in sync. This initial sync happens over TCP port 3103 and is called a full sync.

After the initial sync the VR agent (VRA – inside the ESXi kernel) will (together with a with a passive virtual SCSI filter) track all I/O and keep an in-memory bitmap of all changed blocks, this bitmap is backed up with a persistent state file (psf) in the home directory of the VM. The psf file contains pointers to the changed blocks. Now the replication engine figures out based on your RPO when the time is right (you cannot yourself schedule this) to send the changed blocks to the replication site. This “ongoing” replication happens over TCP port 44046 initiated from the vmkernel management NICs.

At the recovery site you have deployed your VRS to which the changed blocks will be sent. After the VRS has received all changed blocks for that replication cycle it will pass those off to the ESXi’s network file copy (NFC) service to write the blocks to its target storage. This means that the vSphere replication process is completely abstracted from the underlying storage and as such gives you flexibility in terms of underlying hardware being different at both ends.

