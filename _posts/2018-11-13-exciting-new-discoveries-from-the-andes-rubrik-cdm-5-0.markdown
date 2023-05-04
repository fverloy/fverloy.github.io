---
layout: post
title: Exciting new discoveries from the Andes (Rubrik CDM 5.0)
date: '2018-11-13 23:00:00'
tags:
- rubrik
- backup
---

The Andes or Andean Mountains are the longest continental mountain range in the world, forming a continuous highland along the western edge of South America. The Andes extend from north to south through seven South American countries: Venezuela, Colombia, Ecuador, Peru, Bolivia, Chile and Argentina.

Every year numerous new animal species are discovered in this region, oftentimes in areas that have been explored before. For example, last year a new Andean amphibian species was [found](https://www.independent.co.uk/environment/david-attenborough-rubber-frog-andes-mountains-first-amphibian-british-naturalist-bbc-planet-earth-a7616521.html) and named after the world famous British naturalist David Attenborough.

Just like discovering new animal species in what feels like already deeply explored and well understood territory, adding new insights and capabilities in technology can advance some historically held assumptions. Rubrik, with it’s just announced version 5.0 of it’s software, codenamed Andes, has done just that. One example is extending on our existing Oracle support by adding the capability to perform Live Mounts of Oracle workloads, allowing customers to recover multi-terabyte databases in a matter of minutes.

I’m continually amazed by what our product and engineering teams deliver as the pace of innovation keeps ramping up, Andes being our 14th(sic) release!

Some of the highlights of the Andes release are;

- Oracle Instant Recovery
- SAP HANA support
- NAS Direct Archive
- Elastic App Service
- Windows Bare Metal Recovery
- EPIC EHR support

**Oracle Instant Recovery**

Rubrik introduced Oracle support in version 4 of the software (codenamed Alta for those playing along at home), with the initial focus on providing support for the Oracle DBA’s current backup and recovery workflow through RMAN integration. In 5.0 Rubrik is extending support with the ability to have Oracle backup driven through SLAs governing backup frequency, retention, replication, archiving and so on. Once the system is operational Rubrik will also discover newly added Oracle instances and databases and automatically (if you choose to) add them to existing SLA policies.

One of the most popular features for MS SQL server is now also coming to Oracle with the ability to Live Mount the Oracle datafiles on the Rubrik cluster itself and then exposing them to the Oracle host of your choice for instant and/or selective recovery. Live Mount also includes the ability to recover to any point-in-time. Via instant recovery the database (or specific tablespace) can then be live migrated from the Rubrik cluster to the running Oracle instance.

**SAP HANA support**

Rubrik has built a SAP certified solution to offer protection for SAP HANA environments, leveraging SAP’s native Backint APIs Rubrik is able to perform backup, restore, inquire, and delete functions via either SAP HANA studio, SAP Cockpit, or native hbdsql.

**NAS Direct Archive**

As the data deluge continues customers need to protect and manage ever increasing amounts of data, this is very apparent in the NAS space where we are frequently confronted with multi-PB environments. Instead of focusing on becoming a just another storage location for these large environments Rubrik decided to bring more data intelligence to the problem and improve the overall TCO for the customer by offloading the data to another storage tier.

Rubrik will backup and index all NAS data but instead of storing it on the Rubrik cluster it will be passed through immediately to a storage location of your choice, this could be another NAS location, a on-premises object store, or a public cloud blob storage location, no lock-in.

**Elastic App Service**

The mix of applications at customers varies greatly and so does the way these are managed, some customers like using native application tools for backup and recovery or have built in-house toolsets and prefer sticking to existing workflows. To bring those workloads (think MySQL, PostgreSQL,…) into the Rubrik fold and provide benefits like SLA driven retention, replication, archiving, plus indexing, predictive search, compression, and deduplication Rubrik created EAS. Think of it as a storage-efficient, highly available, distributed storage repository that can ingest all forms of data and apply Rubrik’s brand of data management to it.

**Windows Bare Metal Recovery**

Rubrik Andes has the ability to backup full Windows volumes, including systems state, and then providing a automated, single-step process to recover said volumes. The way the solution is built provides additional flexibility for recovery or migration purposes, since the physical Windows workloads end up as virtual hard drives on the Rubrik cluster they can be easily used for physical to virtual, and physical to cloud migrations. It is also much simpler to use the resulting data set to migrate from one hypervisor to another if needed furthering the goal of making data independent of the underlying infrastructure.

**EPIC EHR support**

Rubrik supports InterSystem’s Caché through storage array-snapshot integration. Caché is the primary database used by Epic for EHR data. By utilizing the open APIs provided by storage arrays for taking snapshots and identifying differentials between the snapshots, Rubrik provides seamless protection of any application using the storage array volumes, while providing the same simplicity and ease of data management through SLA Policies.

Besides treading on somewhat traditional territory Rubrik is also expanding support for newer types of workloads with the initial integration of [Datos IO](https://www.rubrik.com/product/datos-io-overview/) in Rubrik CDM. Outside of a new software release Rubrik also recently announced support for [VMware Cloud on AWS.](https://www.rubrik.com/blog/rubrik-vmware-cloud-aws-data-management/)

**Polaris O365 support**

In April of this year Rubrik launced it’s own SaaS platform called Polaris, joining the Polaris GPS and Polaris Radar application which are already GA, today Rubrik is also announcing support for Office 365. Through the unified Polaris interface customers can now protect and manage their O365 environment using the same SLA policy system for on-premises, or public cloud resources bringing the first SaaS application into the fold. O365 support includes the capability for global search and single item recovery, Rubrik’s architecture stores O365 backup data in public cloud blob storage, the focus remains on acting as a data management control plane and not forcing customers to consume alternate storage locations.

