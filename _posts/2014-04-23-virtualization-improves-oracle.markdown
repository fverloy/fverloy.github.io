---
layout: post
title: Virtualization improves Oracle
date: '2014-04-23 22:00:00'
tags:
- vmware
- oracle
permalink: /virtualization-improves-oracle/
---

By virtualizing the Oracle Database on vSphere we get significant scalability, availability, and performance benefits. But equally important it does not change the DBA responsibilities or required skill set, so although natural, this DBA turf protection is mostly unnecessary.

Like any other workload virtualizing database workloads on vSphere significantly reduces the number of physical systems your organization requires, while also achieving more effective utilization of datacenter resources. You can realize tangible savings from this consolidation along with operational cost savings from reduced datacenter floor space, power, and cooling requirements.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/oracle1.jpg" class="kg-image" alt loading="lazy" width="250" height="235"></figure>

_Yeah, but what about performance?_

You can achieve combined cost-effectiveness and performance because vSphere provides;

Memory oversubscription

More efficient use of physical RAM by reclaiming unused physical memory and consolidating identical memory pages among virtual machines on a host. DBA will need to work with the VMware admins to set a memory reservation corresponding to the total amount of shared memory used by the GOS Oracle instance(s).

High performance “gang” scheduler

Can answer the CPU and I/O needs of Oracle virtual machines by dynamically allocating more resources and larger processor time slices to the virtual machines.

DRS

Ability to load-balance, on server level, the different server VM’s across different nodes in your cluster, this allows to run at higher utilization levels while still being able to meet your business SLA’s.

Direct driver model

ESXi enables very high I/O throughput and can handle the I/O requirements for more virtual machines simultaneously requesting hardware resources.

Support for large memory pages

Support for large memory pages (called Huge Pages in Linux) and nested/extended page tables. Optimized memory access can provide substantial performance benefits for mission-critical, memory-intensive applications and can reduce CPU resource consumption by up to 15%.

The basic value of VMware for virtualization your Oracle environment can be summarized into 2 main fields;

1. Resource management capabilities;

- DRS – allowing you to load-balance, on server level, the different server VM’s across different nodes in your cluster.
- The ability to hot-add CPU resources to a running VM and allow the running application, Oracle, to take advantage of that.

1. vSphere feature-set allowing you to satisfy your SLA’s for High Availability;

- HA will restart the VM on an available node in the cluster, not necessarily a pre-designated backup target (hint hint), but intelligent fan-out fail-over whereby the “system” determines the best node for that specific VM.

Some say (stig?) that a majority of Oracle databases running on RAC (horizontal scalability model) in customer environments can be run on VMware vSphere and still satisfy the HA SLA requirements without RAC. Of course you can run RAC on vSphere as well (do you need that 10 second failover, and at what price?).

Also a stretched RAC deployment using EMC VPLEX is now in the realm of possibilities with vSphere.

<figure class="kg-card kg-image-card"><img src="https://img.youtube.com/vi/n_gY9JQtDvo/0.jpg" class="kg-image" alt="VPLEX" loading="lazy"></figure>

Other benefits for Oracle environments include spinning up a database VM from a template to quickly mount a database for a cost effective dev- and test environment. And giving you a platform to migrate from traditional Unix (HP-UX, AIX, Solaris Sparc) to x86 linux.

Support and licensing

For the official explanation of certification, support, and licensing please refer to the document “understanding Oracle certification, support, and licensing for VMware environments: (http://www.vmware.com/files/pdf/techpaper/vmw-understanding-oracle-certification-supportlicensing-environments.pdf)

Additional information on support and licensing can be found here.

Support;

Oracle is fully supported on an unmodified OS, as you know vSphere does not modify the OS so there should be no issue with support. The statement Oracle support makes is “Oracle will only provide support for issues that either are known to occur on the native OS, or can be demonstrated not to be as a result of running on VMware.”

Licensing;

You need to license every single core on every single node that is running Oracle , so creating clusters (subclusters) which you designate to Oracle will potentially give you significant license savings (think of it as making sure you squeeze every GHz of processing power out of every license). In other words, once you have licensed all cores of your host you can run as many Oracle VM’s on it as you want.

If you are looking for best practises for running Oracle Databases on VMware please consult this guide.

Or look at my Best Practices slide deck on slideshare:

(https://www.slideshare.net/fverloy/oracle-on-vsphere-best-practises?ref=http://filipv.net/2014/04/24/virtualization-improves-oracle/)

