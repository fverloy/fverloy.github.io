---
layout: post
title: Whitewater 3 – waves of innovation washing onto the shore
date: '2013-08-08 22:00:00'
tags:
- riverbed
- storage
---

Riverbed recently released the latest edition of it’s cloud storage gateway, both upgrading the software and providing new hardware options.

**What is Riverbed Whitewater?**

Whitewater is an on-premise appliance that connects your internal network with a cloud storage provider, it easily integrates your existing back-up/archive infrastructure with cloud storage, leveraging the cloud as a low cost tier for long term storage.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa1.png" class="kg-image" alt loading="lazy" width="1108" height="500" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/wwa1.png 1000w, __GHOST_URL__ /content/images/2021/08/wwa1.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Whitewater brings cloud scale cost and protection (cloud data durability is extremely high (11x9s) due to advanced cloud architectures) benefits into your existing infrastructure. At the same time Whitewater provides fast restore since the local cache will hold the most recent backup data.

In contrast to Riverbed Steelhead, the WAN optimization solution, whitewater is single-ended (you only need 1 appliance in your datacenter), whereas Steelhead requires an appliance (or softclient) at both ends of the WAN connection.

On the front-end it presents itself as a CIFS/NFS share, providing easy integration with existing back-up applications, and on the back-end it connects to a cloud storage system using REST APIs.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa2.png" class="kg-image" alt loading="lazy" width="698" height="420" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa2.png 600w, __GHOST_URL__ /content/images/2021/08/wwa2.png 698w"></figure>

Data that is written to Whitewater is deduplicated inline, and securely (encrypted at rest and in-flight) transferred to your cloud storage provider/system.

I’ve written about Whitewater a couple of times before;

http://filipv.net/2013/05/19/amazon-glacier-and-backup-economics/

http://filipv.net/2012/12/26/riverbed-whitewater-and-caringo-castor/

**What’s new? – Bigger, Better, Faster**

- Whitewater now supports up to 2.88PB of source data locally cached
- Up to 14.4PB of source data in the cloud
- Scalability by optionally allowing you to connect disk shelve extensions
- Faster performance, now ingesting up to 2.5TB/Hr
- 10Gb connectivity
- Ability to locally pin a data set
- Ability to perform replication to a remote whitewater (peer replication)
- Symantec Enterprise Vault support

**Storage shelve extensions**

The current version allows you to connect 2 additional storage shelves, greatly expanding the local cache. This combined with local data pinning and peer replication makes it feasible to use the system as a backup to disk system without the cloud tier. But the main purpose of the solution remains leveraging cloud storage economics for long term retention.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa3.png" class="kg-image" alt loading="lazy" width="450" height="278"></figure>

**Locally pin a data set**

If you have a particular data set for which your SLA to the business requires a shorter RTO you can optionally lock this data set on the local cache (changes will still be replicated to the peer and/or cloud storage). This way you can ensure that this data set will always be recovered from the local cache at LAN speed.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa4.png" class="kg-image" alt loading="lazy" width="481" height="191"></figure>

**Peer replication**

Another standard feature (at no additional license cost) in version 3 of Whitewater is the ability to replicate data to a peer Whitewater at a DR site.

Since Whitewater uses inline deduplication this means that the primary appliance will sent only deduplicated (and encrypted) traffic towards the DR site, thus greatly reducing network transmissions. The secondary whitewater first needs to acknowledge the data before it is replicated to the cloud as a 3rd tier.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa5.png" class="kg-image" alt loading="lazy" width="1108" height="468" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/wwa5.png 1000w, __GHOST_URL__ /content/images/2021/08/wwa5.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Although we are only transferring deduplicated data we still allow you to control the bandwidth used for replication both to the peer whitewater and the cloud.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa6.png" class="kg-image" alt loading="lazy" width="983" height="420" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa6.png 600w, __GHOST_URL__ /content/images/2021/08/wwa6.png 983w" sizes="(min-width: 720px) 720px"></figure>

**Symantec Enterprise Vault support**

Whitewater allows you to integrate the cloud as a storage vault for Symantec Enterprise Vault. Click here for more information on Enterprise Vault.

**What if my datacenter is lost and I need to restore from the cloud?**

First of all we would recommend replicating to a peer whitewater in a DR site so you don’t incur cloud restore charges or transmission delays. But we do allow you to download a virtual whitewater for FREE (read-only) which will allow you to quickly (or at least quicker since we are pulling out deduplicated data) restore your data and get back online.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa7.png" class="kg-image" alt loading="lazy" width="1018" height="373" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa7.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/wwa7.png 1000w, __GHOST_URL__ /content/images/2021/08/wwa7.png 1018w" sizes="(min-width: 720px) 720px"></figure>

**A word about deduplication**

In order to make cloud storage economically feasible Whitewater first deduplicates data before sending it to the cloud. Withe deduplication only unique data is stored on the disk thus guaranteeing much more efficient utilization of any storage.

In the process of deduplication the incoming data stream is split into blocks. A fingerprint (digital signature) is created for each block to uniquely identify it, as well as a signature index. The index provides the list of references in order to determine if a block already exists on disk. When the deduplication algorithm finds an incoming data block that has been processed before (a duplicate), it does not store it again but it creates a reference to it. References are generated every time a duplicate is found. If a block is unique, the deduplication system writes it to disk.

Some deduplication techniques split each file into fixed length blocks, others, like Whitewater use variable length blocks. Fixed Block deduplication involves determining a block size (size varies based on the system but is fixed) and segmenting files/data into those block sizes.

Variable Block deduplication involves using algorithms to determine a variable block size. The data is split based on the algorithm’s determination. When something changes, i.e. data is added so the blocks shift then the algorithm will determine the shift so the blocks that follow are not “lost” by the algorithm, fixed block length cannot do this.

In the example below we have a fixed block length of 3, so the incoming data is “sliced” into block of 3 characters. The arrow indicates a change to the data, i.e. we add a new character (A) upstream, the result of which is, since the boundaries with fixed length do not change, that all blocks now contain different data and there are zero block matches meaning all blocks are unique and will be written to disk.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa8.png" class="kg-image" alt loading="lazy" width="839" height="228" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa8.png 600w, __GHOST_URL__ /content/images/2021/08/wwa8.png 839w" sizes="(min-width: 720px) 720px"></figure>

Notice how the variable block deduplication has seemingly random block sizes. While this does not look too efficient compared to fixed block, notice what happens when we add the same upstream element to the data.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa9.png" class="kg-image" alt loading="lazy" width="818" height="229" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa9.png 600w, __GHOST_URL__ /content/images/2021/08/wwa9.png 818w" sizes="(min-width: 720px) 720px"></figure>

Since the variable block length algorithm has determined the boundary for this particular data to lie between C and BB only the first block (AABC) has changed and needs to be written to disk, the other blocks remain unchanged and can be referenced by the deduplication algorithm.

Since Whitewater uses variable segment length inline deduplication this allows for higher dedupe ratios than fixed block length deduplication (see above), once we have deduplicated the data we use LZ compression to further compact the data. We see an average data set reduction of 10 to 30x depending on the source data.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/wwa10.png" class="kg-image" alt loading="lazy" width="1108" height="408" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/wwa10.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/wwa10.png 1000w, __GHOST_URL__ /content/images/2021/08/wwa10.png 1108w" sizes="(min-width: 720px) 720px"></figure>

If you are an existing Riverbed Whitewater customer you can download Whitewater 3.0.x [here](https://support.riverbed.com/software/whitewater.htm)

