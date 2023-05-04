---
layout: post
title: Rubrik 6.0 Summer Release Overview
date: '2021-08-02 05:00:00'
tags:
- rubrik
- backup
- cloud
- ransomware
cover-img: /assets/img/ksenia-makagonova-A-tHpo6p9f8-unsplash.jpg
thumbnail-img: /assets/img/ksenia-makagonova-A-tHpo6p9f8-unsplash.jpg
share-img: /assets/img/ksenia-makagonova-A-tHpo6p9f8-unsplash.jpg
---

Rubrik made the 6.0 Summer Release generally available a few days ago, in this blog post I wanted to provide an overview of some of the new features.

**AppFlows and 24H CDP support**

AppFlows is a new application on the Polaris SaaS platform (it went GA a few days after the RCDM 6.0 release) that delivers orchestrated disaster recovery for VMware (initially), it unifies backup and DR in one platform, provides DR failover and failback with near-zero RPO (i.e. CDP integration) and will be closer integrated with our existing Radar solution for orchestrated ransomware recovery.

<img src="/assets/img/appflows.png">

Continuous data protection (CDP, leveraging the VMware VAIO framework) also had to be extended to go from 4H to 24H to bring it inline with more typical DR requirements.

<img src="/assets/img/cdpsummer.png">

**Radar enhancements**

Radar has gotten a number of further enhancements including a new dashboard to provide a global view into the unstructured data estate so you can rapidly assess anomalous behaviour.

<img src="/assets/img/radar.png">

We also provide bulk ransomware recovery to enable fast recovery for large-scale ransomware events, including integration with our new VMware in-place recovery (CBT-restore) capabilities. A public API is now also available for Radar to enable integration with SIEM and SOAR solutions like Palo Alto Networks Cortex XSOAR.

<img src="/assets/img/xsoar.png">

**Immutable Cloud Archives**

To extend our immutability from our local filesystem Atlas into the Cloud we needed to support the ability to use CloudOut to archive to Azure immutable storage. This will provide ransomware protection and regulatory compliance for long-term retention data.

<img src="/assets/img/cloudoutimm.png">

**Large File Sharding**

A number of improvements were made to ensure Rubrik leading edge performance for enterprise applications, including large file sharding, this will parallelize the ingest of large individual files like those found in healthcare (e.g. EPIC), and media and entertainment environments.

**Backup from NetApp SnapMirror**

We now support backup from the replicated copy of your NetApp NAS, this way there is no impact on the primary NAS host during backup, we will leverage the read-only SnapMirror copy as the source of data when backing up.

**Adaptive Throttling of NAS backup**

Adaptive throttling for NAS measures the latency during backup, if we detect increases in latency Rubrik will throttle down ongoing file backups to reduce the load on the NAS host (typically only noticeable on older or slower NAS hosts).

**Oracle Advanced Cloning Options**

The UI has been updated to enhance the user experience when initiating dissimilar recoveries between source and destination Oracle environments. The updated UI will allow you to search file paths from within the UI, but we also still support uploading the ACO file as in the past.

<img src="/assets/img/oac.png" class="kg-image" alt loading="lazy" width="1105" height="1399" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/oac.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/oac.png 1000w,/assets/img/oac.png 1105w" sizes="(min-width: 720px) 720px"></figure>

**Oracle Data Guard support**

We now support backup from the Oracle Data Guard standby database for critical production workloads. Rubrik will discover the standby databases and initiate backup from them.

**In-place recovery for VMware**

A new option to recover VMware VMs. It allows one to preserve a VM’s identity and transfer only the deltas (reverse CBT) by leveraging the existing data on the primary side. If you have an environment that doesn’t allow external NFS storage to be used we can now also leverage CBT-restore instead of Live Mount.

<img src="/assets/img/cbtrestore.png">

**Cloud Cluster ES (Elastic Storage)**

A rejigged version of Cloud Cluster that directly writes backup data to Amazon S3 or Azure Blob, this will lead to cost savings for larger sets of data while still maintaining performance for backup and restore operations.

**Azure VM Application Consistency**

Through Polaris Cloud Native Protection for Azure VM’s you can now enable App Consistency using Windows VSS or Pre- and Post-scripts (or both).

<img src="/assets/img/azurevss.png">

**MS Teams native protection and new M365 Dashboard**

You can now backup and recover Teams data via our Polaris platform similar to how you can already protect Exchange Online, SharePoint, and OneDrive. We also introduced a new dashboard providing the ability to track Microsoft 365 protection progress, and quickly understanding if there are any issues both with the Microsoft services itself and our protection level. The new dashboard can track jobs, storage usage, compliance and provides a quick link to events.

<img src="/assets/img/teams.png">

**Cloud Cost Insights**

This provides instantaneous cloud cost insights during SLA creation/edit to be able to configure cost-optimal policies. CDM shall provide clear recommendations on when to enable or disable consolidation and compare costs across SLA Domains.

<img src="/assets/img/cloudcost.png">

_NOTE: These were just the highlights of the 6.0 Summer Release, a full release notes overview is available via support.rubrik.com, I also did not include the 6.0.x updates coming in the next couple of weeks/months._

