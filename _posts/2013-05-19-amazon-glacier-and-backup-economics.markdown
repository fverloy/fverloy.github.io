---
layout: post
title:  "Amazon Glacier and Backup economics"
date:   2013-05-19 14:21:43 +0200
categories: Amazon, Glacier, Backup
permalink: /2013/05/19/amazon-glacier-and-backup-economics/
redirect_from:
  - /2013/05/19/amazon-glacier-and-backup-economics/amp/
---
In the summer of last year Amazon announced Amazon Glacier, an extremely low cost storage service designed for data archiving and backup.

This makes it a very compelling solution for offloading your backup data to the cloud at low cost, but the point of a backup solution is not backing up your data, it is enabling restore of said data. The time it takes for the restore to complete must fit in your RTO (how long can the business wait before the data is back and useable), and this is where Amazon Glacier potentially falls down because the SLA it adheres to for getting your data back is between 3 and 5 hours, this is the reason why it is primarily marketed as an archive solution whereby the time constraints are less stringent and the cost of storing the archive takes precedent over the RTO. (If you need faster access to your data look at Amazon S3, but of course take into account the cost differential there).

![blog image]({{ "/assets/glacier1.png" | absolute_url }})

But have no fear, you can have your cake and eat it too, with Riverbed Whitewater you can leverage the low cost Amazon Glacier storage and still get fast restores. Whitewater is a tiered backup solution that ingests data from your existing, unmodified backup server, using inline deduplication to minimize the local storage required to maintain a full backup of your data locally, and sending the rest up to Amazon Glacier. Because most restores your users request are for relatively new data, chances are this data is stored on the local disks of the Whitewater appliance and the restore will be at LAN speed. The pricing of Amazon Glacier (see picture above) also assumes that storage retrieval will be infrequent (this is calculated in the pricing model), like say for archiving purposes, and with Whitewater it can be for backup purposes as well.

![blog image]({{ "/assets/glacier3.png" | absolute_url }})

![blog image]({{ "/assets/glacier4.gif" | absolute_url }})So a serious reduction in data protection costs, eliminating tape, tape vaulting and disaster recovery storage sites. Improving DR readiness with secure anywhere accessible (think DR for example) Amazon Glacier storage services providing 11 9???s of durability. No need to change your existing backup application and processes, using less storage in Glacier because of our inline deduplication, and with local LAN speed restores. End-to-end security with secure data in flight and at rest with SSL v3 and AES 256 bit encryption.