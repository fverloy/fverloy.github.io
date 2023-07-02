---
layout: post
title: Riverbed Whitewater and Caringo CAStor
date: '2012-12-25 23:00:00'
tags:
- riverbed
- storage
- backup
permalink: /riverbed-whitewater-and-caringo-castor/
---

One of the coolest, in my humble opinion of course, solutions I was able to work with when I was at Dell, was the CAStor Object Storage system from Caringo, OEM’ed as the Dell DX6000.

The CTO and co-founder of Caringo is Paul Carpentier (the –car– in Caringo) known as the father of the Content Addressing concept. He previously worked at FilePool, where he invented the technology that created the Content Addressed Storage industry. FilePool, was sold to EMC who turned his work into the Centera platform.

CAS or Object Storage is one of three major forms of storage concepts (you can argue about how many forms of storage there are until the cows come home, but for the sake of simplicity I’ll focus on the three major ones here). The other two being file- and block storage.

In general the argument is that a certain type of content (documents, pictures, databases,…) requires, or at least works/fits better, when it resides on a specific type of storage. The access patterns of the different types of data make it that for example relational data is better served from a database on block storage as opposed to separate files from a file server.

Block storage has access to raw, unformatted hardware, when speed and space efficiency are most important this is probably a good fit. For example databases containing relational customer information.

File storage is most common to end-users, it takes a formatted harddrive and is exposed to the user as a filesystem. It is an abstraction on top of the storage device and thus you loose speed and space in favour of simplicity, it has inherent scalability issues (both in terms of the maximun size of a document, and the maximum number of documents you can store) and does not perform well when there is high demand for a certain piece of data by multiple users.

Object storage is probably the least familiar form of storage to the general public, it does not provide access to raw blocks of data, and it does not offer file based access\*. It provides access to whole objects (files + metadata) or BLOBs of data. Typically you access it using a HTTP REST API specific to that CAS system. It lends itself particularly well to content like backup and archive data since that doesn’t change often. But also things like audio- and video files, medical images, large amounts of documents, etc. The object is to store a large, and growing, amount of data at a relatively low cost.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/dell-dx-overview.jpg" class="kg-image" alt loading="lazy" width="371" height="261"></figure>

The data and metadata are both written to the object storage system and get a unique UUID to reference it later.

A CAS system consist of a number of storage nodes (typically x86 servers with internal disks in a JBOD configuration – remember the relatively inexpensive remark above), in the case of CAStor it is a symmetric distributed system meaning that all nodes are exactly the same and they perform the same functions.

Data is stored and retrieved using HTTP REST APIs, the client writes data to the cluster using HTTP POST commands, the cluster has an internal load balancing algorithm that decides which node in the cluster will respond (HTTP 301 redirect response) to the client, the client is redirected to the node selected by the cluster and repeats it’s POST command, this time the node will issue the HTTP 100 continue command to tell the client to start writing the data.

Once the data is received the node issues the HTTP 201 response to let the client know the data has been written. The response also includes a UUID (Universally Unique Identifier) which is needed later to retrieve the data. Now since one of the goals of a CAS system is to make sure your data is kept safe, the cluster now needs to replicate your freshly written data unto other nodes in the cluster, this replica is written shortly after the original data has been received (so yes, in theory if the cluster fails very shortly after the initial write the data was not replicated). All objects on the CAStor system are exact replicas, there is no special “original data set”.

In order to get the replica onto one or more of the other nodes in the cluster, the other node will issue a HTTP 200 GET request for the original piece of data, if you later need to read the data again the client would also issue a HTTP 200 GET request to a random node in the cluster, the cluster will locate the data (UUID) and again redirect (301) the client to one of the nodes containing the data using it’s internal load-balancing algorithm.

Now Riverbed has a Cloud Storage Gateway called Whitewater specifically designed to store and retrieve backup and archive data on object storage systems. Whitewater is compatible with a set of REST APIs that allows it to interact with a lot of public cloud storage systems like Amazon S3, Google, Azure, HP Cloud Services, AT&T Synaptic Storage as a Service, Nirvanix, Telus, Rackspace, and others. But Whitewater also allows you to utilize the same concept in a private cloud setup as long as you either use OpenStack Swift, or EMC Atmos.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/caringo1.png" class="kg-image" alt loading="lazy" width="616" height="305" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/caringo1.png 600w, __GHOST_URL__ /content/images/2021/08/caringo1.png 616w"></figure>

Recently Caringo announced the beta version of CAStor for OpenStack giving organizations an option to include a more robust, fully supported object store while still benefiting from the progress the OpenStack community is achieving.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/caringo2.png" class="kg-image" alt loading="lazy" width="315" height="215"></figure>

So by leveraging the Whitewater integration with OpenStack, you can now have your backup and archive data stored on very robust object storage in your own private cloud. The number of replicas and the geographical location (or in case of a private cloud your DR center location) of the replicas is dictated by the cloud storage policy (managed by the Content Router in Caringo terms), Whitewater would write to the CAS system and the right policy would be invoked to replicate the data in the right locations.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/caringo3.png" class="kg-image" alt loading="lazy" width="344" height="136"></figure>

The integration with your existing backup software is seamless since the Whitewater appliance presents itself using either CIFS or NFS towards your internal network while connecting to the object storage system using it’s specific APIs. Since Whitewater uses deduplication (and encryption in-flight and at-rest) you both benefit from having less data (dedupe) to store and having a cheaper (x86 servers) storage system to store it on. In terms of scalability the system takes on new nodes and these are automatically used to redistribute the load if needed and go towards the unlimited (128bit namespace) scaling of the system.

How does your backup system scale these days..? 😉

\*You can deploy a filesystem gateway to a CAS system so you can offer access via a filesystem if you want.

