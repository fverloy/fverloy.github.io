---
layout: post
title: vCenter Log Insight – Turn data into intelligence
date: '2013-12-17 23:00:00'
tags:
- vmware
permalink: /vcenter-log-insight-turn-data-into-intelligence/
---

I haven’t been blogging a lot the last couple of months, I’m still trying to drink from the water-hose having recently started at VMware, and of course customers come before blogs…

One of the products in VMware’s portfolio which in my humble opinion doesn’t get enough attention externally is vCenter Log Insight, but maybe that’s just because I got really excited when I first learned about Splunk and log analytics.

In IT every device, os, application and everything in between generates lot’s of unstructured log data, so much so that most of it probably gets discarded. I remember when I first started out in IT support 14 years ago grep’ing my way through unix logs, or even worse clicking away all those yellow warning signs on Windows boxes.

The basic premise of vCenter Log Insight is turning all that unstructured log data into actionable data, building relevant dashboards that add meaning instead of meaning more chores for the IT department (or business).

As an example let’s look at building a dashboard out of troubleshooting data, in our case latency issues with storage in a virtual environment.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log1.png" class="kg-image" alt loading="lazy" width="1108" height="780" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log1.png 1000w, __GHOST_URL__ /content/images/2021/08/log1.png 1108w" sizes="(min-width: 720px) 720px"></figure>

As you can see above we have collected about 30 million log entries in total, about a million every 12 hours as you can see in the graph in the upper half of the screen, which is just a visual representation of the collected data in the lower half of the screen.

We could then do a simple text search on all of the collected log data.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log2.png" class="kg-image" alt loading="lazy" width="1108" height="71" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log2.png 1000w, __GHOST_URL__ /content/images/2021/08/log2.png 1108w" sizes="(min-width: 720px) 720px"></figure>

By doing this we have cut down the results from 30+ million, most of them unrelated to our storage performance issues, to 443 all of them related to scsi performance.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log3.png" class="kg-image" alt loading="lazy" width="1108" height="184" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log3.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log3.png 1000w, __GHOST_URL__ /content/images/2021/08/log3.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Since we are chasing a problem we can further fine-tune the results by searching for performance deterioration cutting the results further down to 252 events.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log4.png" class="kg-image" alt loading="lazy" width="1108" height="292" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log4.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log4.png 1000w, __GHOST_URL__ /content/images/2021/08/log4.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Because we need to filter out the actual devices that are impacted by/impacting the performance issue we could click on the scsi id in the text results and vCenter Log Insight will automatically try to find a pattern in all filtered results.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log5.png" class="kg-image" alt loading="lazy" width="1108" height="182" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log5.png 1000w, __GHOST_URL__ /content/images/2021/08/log5.png 1108w" sizes="(min-width: 720px) 720px"></figure>

We then click on “Extract Field”, in this case there is a match and we can now define a user-defineable field.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log6.png" class="kg-image" alt loading="lazy" width="1108" height="452" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log6.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log6.png 1000w, __GHOST_URL__ /content/images/2021/08/log6.png 1108w" sizes="(min-width: 720px) 720px"></figure>

In the interactive analytics section we can now use any of the built-in fields (in this case – latency in microseconds) to build a visual representation of our filtered results.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log7.png" class="kg-image" alt loading="lazy" width="1108" height="266" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log7.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log7.png 1000w, __GHOST_URL__ /content/images/2021/08/log7.png 1108w" sizes="(min-width: 720px) 720px"></figure>

We can now refine our query based on the user-defined field “scsci\_device” we defined earlier.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log8.png" class="kg-image" alt loading="lazy" width="1048" height="768" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log8.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log8.png 1000w, __GHOST_URL__ /content/images/2021/08/log8.png 1048w" sizes="(min-width: 720px) 720px"></figure>

since this gives us some useful information (we see several devices, some have higher latency than others) we can now opt to save this query to the dashboard by clicking “Add to dashboard”.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log9.png" class="kg-image" alt loading="lazy" width="1108" height="262" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log9.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log9.png 1000w, __GHOST_URL__ /content/images/2021/08/log9.png 1108w" sizes="(min-width: 720px) 720px"></figure>

We can further fine-tune by adding some constraints to the results, in this case we are looking for devices with a high latency.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log10.png" class="kg-image" alt loading="lazy" width="1108" height="95" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log10.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log10.png 1000w, __GHOST_URL__ /content/images/2021/08/log10.png 1108w" sizes="(min-width: 720px) 720px"></figure>

So now we have a new graph excluding all devices that have a latency of less than 100.000 microseconds which we can also add to the dashboard.

So if we go to the dashboard view we can see the 2 graphs we just added.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log11.png" class="kg-image" alt loading="lazy" width="1108" height="558" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log11.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log11.png 1000w, __GHOST_URL__ /content/images/2021/08/log11.png 1108w" sizes="(min-width: 720px) 720px"></figure>

The dashboard is highly customisable so we can change the naming, position, and size of the graphs.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log12.png" class="kg-image" alt loading="lazy" width="1108" height="618" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log12.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log12.png 1000w, __GHOST_URL__ /content/images/2021/08/log12.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Besides these user created dashboards there are already a great number of predefined dashboards available under the guise of contents packs from different vendors. In our case we just have the vSphere content pack installed which we can switch to by clicking on “My dashboards”.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log13.png" class="kg-image" alt loading="lazy" width="640" height="648" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log13.png 600w, __GHOST_URL__ /content/images/2021/08/log13.png 640w"></figure>

So now you can easily look at the predefined dashboard by making a selection on the left, in this case “ESXi Hosts”.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/log14.png" class="kg-image" alt loading="lazy" width="1108" height="542" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/log14.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/log14.png 1000w, __GHOST_URL__ /content/images/2021/08/log14.png 1108w" sizes="(min-width: 720px) 720px"></figure>

You can download the 3rd party content packs from the link below: https://solutionexchange.vmware.com/store/loginsight

Of course there is also integration between vCOPS and vCenter Log Insight so you can have analytics on both structured (vCOPS) and unstructured (Log Insight) data to help you find the proverbial needle in the haystack.

