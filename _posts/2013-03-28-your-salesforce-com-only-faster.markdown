---
layout: post
title: Your Salesforce.com, only faster.
date: '2013-03-28 23:00:00'
tags:
- riverbed
- networking
---

As mentioned in my previous post Riverbed has a joint SaaS optimization solution with Akamai called Steelhead Cloud Accelerator. In this blog post I will show you how to use this technology to accelerate your salesforce (people and the application).

The picture below is a diagram of the lab environment I’ll be using for this setup.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc1.jpg" class="kg-image" alt loading="lazy" width="848" height="257" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc1.jpg 600w, __GHOST_URL__ /content/images/2021/08/sfdc1.jpg 848w" sizes="(min-width: 720px) 720px"></figure>

The lab uses a WAN Simulator so we can simulate a cross-atlantic link towards Salesforce.com. For this simulation I have set the link to 200ms latency and 512Kbps.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc2.png" class="kg-image" alt loading="lazy" width="975" height="596" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc2.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc2.png 975w" sizes="(min-width: 720px) 720px"></figure>

For the Steelhead Cloud Functionality you need a specific firmware image, freely available to our customers on http://support.riverbed.com, you can recognize this by the -sca at the end of the version number (right hand corner in the screenshot below).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc3.png" class="kg-image" alt loading="lazy" width="925" height="297" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc3.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc3.png 925w" sizes="(min-width: 720px) 720px"></figure>

Once you are using the firmware you get an additional option under Configure –\> Optimization, called Cloud Accelerator. (see screenshot above).

Here you can register the Steelhead in our cloud portal (which is running as a public cloud service itself, running on Amazon Web Services). You can also enable one or more of our currently supported SaaS applications (Google Apps, Salesforce.com, and Office 365).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc4.png" class="kg-image" alt loading="lazy" width="656" height="549" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc4.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc4.png 656w"></figure>

When you register the appliance on the Riverbed Cloud Portal you need to grant the appliance cloud service to enable it.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc5.png" class="kg-image" alt loading="lazy" width="673" height="310" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc5.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc5.png 673w"></figure>

Once the appliance is granted service, the status on the Steelhead itself will change to “service ready”

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc6.png" class="kg-image" alt loading="lazy" width="649" height="185" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc6.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc6.png 649w"></figure>

So let’s first look at the unoptimized version of our SaaS application. As you can see in the screenshot below I have disabled the Steelhead optimization service so all connections towards Salesforce.com will be pass-through. You can also see the latency is 214ms on average and the bandwidth is 512Kbps.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc7.png" class="kg-image" alt loading="lazy" width="641" height="386" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc7.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc7.png 641w"></figure>

I logged into Salesforce.com and am attempting to download a 24MB PowerPoint presentation, as you can see in the screenshot below this is estimated to take about 7 minutes to complete. Time for another nice unproductive cup of coffee…

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc8.png" class="kg-image" alt loading="lazy" width="830" height="428" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc8.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc8.png 830w" sizes="(min-width: 720px) 720px"></figure>

If we now enable the optimization service on the Steelhead it will automatically detect that we are connecting to Salesforce.com and in conjunction with Akamai spin up a cloud Steelhead on the closest Akamai Edge Server next to the Salesforce.com datacenter I am currently using.

Looking at the current connections on the Steelhead you can see that my connections to Salesforce.com are now being symmetrically optimized by the Steelhead in the Lab and the Cloud Steelhead on the Akamai-ES.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc9.png" class="kg-image" alt loading="lazy" width="904" height="267" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc9.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc9.png 904w" sizes="(min-width: 720px) 720px"></figure>

Note the little lightning bolt in the notes section signifying that Cloud Acceleration is on.

Let’s attempt to download the presentation again.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc10.png" class="kg-image" alt loading="lazy" width="823" height="459" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc10.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc10.png 823w" sizes="(min-width: 720px) 720px"></figure>

Yeah, I think you could call that faster…

But that is not all, because we are using the same proven Steelhead technology including byte-level deduplication I can edit the PowerPoint file and upload it back to salesforce.com with a minimum of data transfer across the cloud.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc11.png" class="kg-image" alt loading="lazy" width="595" height="44"></figure>

I edited the first slide by changing the title and subtitle and will upload the changed file to my SaaS application, notice that the filename itself is also changed.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc12.png" class="kg-image" alt loading="lazy" width="574" height="205"></figure>

Looking at the current connections on the Steelhead you can see I am uploading the file at the same breakneck speed since I only need to transfer the changed bytes.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/sfdc13.png" class="kg-image" alt loading="lazy" width="739" height="163" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/sfdc13.png 600w, __GHOST_URL__ /content/images/2021/08/sfdc13.png 739w" sizes="(min-width: 720px) 720px"></figure>

So there you have it, Salesforce.com at lightning speeds!

NOTE: I have not mentioned the SSL based configuration needed to allow us to optimize https based SaaS applications (as all of them are), I will cover this in a later post.

