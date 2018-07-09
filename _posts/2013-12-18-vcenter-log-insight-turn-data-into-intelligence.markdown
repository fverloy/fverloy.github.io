---
layout: post
title:  "vCenter Log Insight – Turn data into intelligence"
date:   2013-12-18 14:21:43 +0200
categories: VMware, vCenter Log Insight
permalink: /2013/12/18/vcenter-log-insight-turn-data-into-intelligence/
---
I haven’t been blogging a lot the last couple of months, I’m still trying to drink from the water-hose having recently started at VMware, and of course customers come before blogs…

One of the products in VMware’s portfolio which in my humble opinion doesn’t get enough attention externally is vCenter Log Insight, but maybe that’s just because I got really excited when I first learned about Splunk and log analytics.

In IT every device, os, application and everything in between generates lot’s of unstructured log data, so much so that most of it probably gets discarded. I remember when I first started out in IT support 14 years ago grep’ing my way through unix logs, or even worse clicking away all those yellow warning signs on Windows boxes.

The basic premise of vCenter Log Insight is turning all that unstructured log data into actionable data, building relevant dashboards that add meaning instead of meaning more chores for the IT department (or business).

As an example let’s look at building a dashboard out of troubleshooting data, in our case latency issues with storage in a virtual environment.

![blog image]({{ "/assets/log1.png" | absolute_url }})

As you can see above we have collected about 30 million log entries in total, about a million every 12 hours as you can see in the graph in the upper half of the screen, which is just a visual representation of the collected data in the lower half of the screen.

We could then do a simple text search on all of the collected log data.

![blog image]({{ "/assets/log2.png" | absolute_url }})

By doing this we have cut down the results from 30+ million, most of them unrelated to our storage performance issues, to 443 all of them related to scsi performance.

![blog image]({{ "/assets/log3.png" | absolute_url }})

Since we are chasing a problem we can further fine-tune the results by searching for performance deterioration cutting the results further down to 252 events.

![blog image]({{ "/assets/log4.png" | absolute_url }})

Because we need to filter out the actual devices that are impacted by/impacting the performance issue we could click on the scsi id in the text results and vCenter Log Insight will automatically try to find a pattern in all filtered results.

![blog image]({{ "/assets/log5.png" | absolute_url }})

We then click on “Extract Field”, in this case there is a match and we can now define a user-defineable field.

![blog image]({{ "/assets/log6.png" | absolute_url }})

In the interactive analytics section we can now use any of the built-in fields (in this case – latency in microseconds) to build a visual representation of our filtered results.

![blog image]({{ "/assets/log7.png" | absolute_url }})

We can now refine our query based on the user-defined field “scsci_device” we defined earlier.

![blog image]({{ "/assets/log8.png" | absolute_url }})

since this gives us some useful information (we see several devices, some have higher latency than others) we can now opt to save this query to the dashboard by clicking “Add to dashboard”.

![blog image]({{ "/assets/log9.png" | absolute_url }})

We can further fine-tune by adding some constraints to the results, in this case we are looking for devices with a high latency.

![blog image]({{ "/assets/log10.png" | absolute_url }})

So now we have a new graph excluding all devices that have a latency of less than 100.000 microseconds which we can also add to the dashboard.

So if we go to the dashboard view we can see the 2 graphs we just added.

![blog image]({{ "/assets/log11.png" | absolute_url }})

The dashboard is highly customisable so we can change the naming, position, and size of the graphs.

![blog image]({{ "/assets/log12.png" | absolute_url }})

Besides these user created dashboards there are already a great number of predefined dashboards available under the guise of contents packs from different vendors. In our case we just have the vSphere content pack installed which we can switch to by clicking on “My dashboards”.

![blog image]({{ "/assets/log13.png" | absolute_url }})

So now you can easily look at the predefined dashboard by making a selection on the left, in this case “ESXi Hosts”.

![blog image]({{ "/assets/log14.png" | absolute_url }})

You can download the 3rd party content packs from the link below:
https://solutionexchange.vmware.com/store/loginsight

![blog image]({{ "/assets/log5.png" | absolute_url }})

Of course there is also integration between vCOPS and vCenter Log Insight so you can have analytics on both structured (vCOPS) and unstructured (Log Insight) data to help you find the proverbial needle in the haystack.