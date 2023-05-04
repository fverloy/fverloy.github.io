---
layout: post
title: Rubrik 6.0 Summer Release Overview
date: '2021-08-02 05:00:00'
tags:
- rubrik
- backup
- cloud
- ransomware
---

Rubrik made the 6.0 Summer Release generally available a few days ago, in this blog post I wanted to provide an overview of some of the new features.

**AppFlows and 24H CDP support**

AppFlows is a new application on the Polaris SaaS platform (it went GA a few days after the RCDM 6.0 release) that delivers orchestrated disaster recovery for VMware (initially), it unifies backup and DR in one platform, provides DR failover and failback with near-zero RPO (i.e. CDP integration) and will be closer integrated with our existing Radar solution for orchestrated ransomware recovery.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appflows.png" class="kg-image" alt loading="lazy" width="2000" height="903" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appflows.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appflows.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/appflows.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/appflows.png 2400w" sizes="(min-width: 720px) 720px"></figure>

Continuous data protection (CDP, leveraging the VMware VAIO framework) also had to be extended to go from 4H to 24H to bring it inline with more typical DR requirements.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/cdpsummer.png" class="kg-image" alt loading="lazy" width="1096" height="421" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/cdpsummer.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/cdpsummer.png 1000w, __GHOST_URL__ /content/images/2021/08/cdpsummer.png 1096w" sizes="(min-width: 720px) 720px"></figure>

**Radar enhancements**

Radar has gotten a number of further enhancements including a new dashboard to provide a global view into the unstructured data estate so you can rapidly assess anomalous behaviour.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/radar.png" class="kg-image" alt loading="lazy" width="2000" height="1131" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/radar.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/radar.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/radar.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/radar.png 2400w" sizes="(min-width: 720px) 720px"></figure>

We also provide bulk ransomware recovery to enable fast recovery for large-scale ransomware events, including integration with our new VMware in-place recovery (CBT-restore) capabilities. A public API is now also available for Radar to enable integration with SIEM and SOAR solutions like Palo Alto Networks Cortex XSOAR.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/xsoar.png" class="kg-image" alt loading="lazy" width="2000" height="1065" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/xsoar.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/xsoar.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/xsoar.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/xsoar.png 2400w" sizes="(min-width: 720px) 720px"></figure>

**Immutable Cloud Archives**

To extend our immutability from our local filesystem Atlas into the Cloud we needed to support the ability to use CloudOut to archive to Azure immutable storage. This will provide ransomware protection and regulatory compliance for long-term retention data.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/cloudoutimm.png" class="kg-image" alt loading="lazy" width="1104" height="948" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/cloudoutimm.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/cloudoutimm.png 1000w, __GHOST_URL__ /content/images/2021/08/cloudoutimm.png 1104w" sizes="(min-width: 720px) 720px"></figure>

**Large File Sharding**

A number of improvements were made to ensure Rubrik leading edge performance for enterprise applications, including large file sharding, this will parallelize the ingest of large individual files like those found in healthcare (e.g. EPIC), and media and entertainment environments.

**Backup from NetApp SnapMirror**

We now support backup from the replicated copy of your NetApp NAS, this way there is no impact on the primary NAS host during backup, we will leverage the read-only SnapMirror copy as the source of data when backing up.

**Adaptive Throttling of NAS backup**

Adaptive throttling for NAS measures the latency during backup, if we detect increases in latency Rubrik will throttle down ongoing file backups to reduce the load on the NAS host (typically only noticeable on older or slower NAS hosts).

**Oracle Advanced Cloning Options**

The UI has been updated to enhance the user experience when initiating dissimilar recoveries between source and destination Oracle environments. The updated UI will allow you to search file paths from within the UI, but we also still support uploading the ACO file as in the past.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/oac.png" class="kg-image" alt loading="lazy" width="1105" height="1399" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/oac.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/oac.png 1000w, __GHOST_URL__ /content/images/2021/08/oac.png 1105w" sizes="(min-width: 720px) 720px"></figure>

**Oracle Data Guard support**

We now support backup from the Oracle Data Guard standby database for critical production workloads. Rubrik will discover the standby databases and initiate backup from them.

**In-place recovery for VMware**

A new option to recover VMware VMs. It allows one to preserve a VM’s identity and transfer only the deltas (reverse CBT) by leveraging the existing data on the primary side. If you have an environment that doesn’t allow external NFS storage to be used we can now also leverage CBT-restore instead of Live Mount.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/cbtrestore.png" class="kg-image" alt loading="lazy" width="1104" height="307" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/cbtrestore.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/cbtrestore.png 1000w, __GHOST_URL__ /content/images/2021/08/cbtrestore.png 1104w" sizes="(min-width: 720px) 720px"></figure>

**Cloud Cluster ES (Elastic Storage)**

A rejigged version of Cloud Cluster that directly writes backup data to Amazon S3 or Azure Blob, this will lead to cost savings for larger sets of data while still maintaining performance for backup and restore operations.

**Azure VM Application Consistency**

Through Polaris Cloud Native Protection for Azure VM’s you can now enable App Consistency using Windows VSS or Pre- and Post-scripts (or both).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/azurevss.png" class="kg-image" alt loading="lazy" width="1030" height="543" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/azurevss.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/azurevss.png 1000w, __GHOST_URL__ /content/images/2021/08/azurevss.png 1030w" sizes="(min-width: 720px) 720px"></figure>

**MS Teams native protection and new M365 Dashboard**

You can now backup and recover Teams data via our Polaris platform similar to how you can already protect Exchange Online, SharePoint, and OneDrive. We also introduced a new dashboard providing the ability to track Microsoft 365 protection progress, and quickly understanding if there are any issues both with the Microsoft services itself and our protection level. The new dashboard can track jobs, storage usage, compliance and provides a quick link to events.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/teams.png" class="kg-image" alt loading="lazy" width="2000" height="1101" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/teams.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/teams.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/teams.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/teams.png 2400w" sizes="(min-width: 720px) 720px"></figure>

**Cloud Cost Insights**

This provides instantaneous cloud cost insights during SLA creation/edit to be able to configure cost-optimal policies. CDM shall provide clear recommendations on when to enable or disable consolidation and compare costs across SLA Domains.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/cloudcost.png" class="kg-image" alt loading="lazy" width="1090" height="1008" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/cloudcost.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/cloudcost.png 1000w, __GHOST_URL__ /content/images/2021/08/cloudcost.png 1090w" sizes="(min-width: 720px) 720px"></figure>

_NOTE: These were just the highlights of the 6.0 Summer Release, a full release notes overview is available via support.rubrik.com, I also did not include the 6.0.x updates coming in the next couple of weeks/months._

