---
layout: post
title:  "Your Salesforce.com, only faster."
date:   2013-03-29 14:21:43 +0200
categories: Salesforce, Riverbed
permalink: /2012/11/06/your-salesforce-com-only-faster/
---
As mentioned in my previous post Riverbed has a joined SaaS optimization solution with Akamai called Steelhead Cloud Accelerator. In this blog post I will show you how to use this technology to accelerate your salesforce (people and the application).

The picture below is a diagram of the lab environment I’ll be using for this setup.

![blog image]({{ "/assets/sfdc1.jpg" | absolute_url }})

The lab uses a WAN Simulator so we can simulate a cross-atlantic link towards Salesforce.com. For this simulation I have set the link to 200ms latency and 512Kbps.

![blog image]({{ "/assets/sfdc2.png" | absolute_url }})

For the Steelhead Cloud Functionality you need a specific firmware image, freely available to our customers on http://support.riverbed.com,  you can recognize this by the -sca at the end of the version number (right hand corner in the screenshot below).

![blog image]({{ "/assets/sfdc3.png" | absolute_url }})

Once you are using the firmware you get an additional option under Configure –> Optimization, called Cloud Accelerator. (see screenshot above).

Here you can register the Steelhead in our cloud portal (which is running as a public cloud service itself, running on Amazon Web Services). You can also enable one or more of our currently supported SaaS applications (Google Apps, Salesforce.com, and Office 365).

![blog image]({{ "/assets/sfdc4.png" | absolute_url }})

When you register the appliance on the Riverbed Cloud Portal you need to grant the appliance cloud service to enable it.

![blog image]({{ "/assets/sfdc5.png" | absolute_url }})

Once the appliance is granted service, the status on the Steelhead itself will change to “service ready”

![blog image]({{ "/assets/sfdc6.png" | absolute_url }})

So let’s first look at the unoptimized version of our SaaS application. As you can see in the screenshot below I have disabled the Steelhead optimization service so all connections towards Salesforce.com will be pass-through. You can also see the latency is 214ms on average and the bandwidth is 512Kbps.

![blog image]({{ "/assets/sfdc7.png" | absolute_url }})

I logged into Salesforce.com and am attempting to download a 24MB PowerPoint presentation, as you can see in the screenshot below this is estimated to take about 7 minutes to complete. Time for another nice unproductive cup of coffee…

![blog image]({{ "/assets/sfdc8.png" | absolute_url }})

If we now enable the optimization service on the Steelhead it will automatically detect that we are connecting to Salesforce.com and in conjunction with Akamai spin up a cloud Steelhead on the closest Akamai Edge Server next to the Salesforce.com datacenter I am currently using.

Looking at the current connections on the Steelhead you can see that my connections to Salesforce.com are now being symmetrically optimized by the Steelhead in the Lab and the Cloud Steelhead on the Akamai-ES.

![blog image]({{ "/assets/sfdc9.png" | absolute_url }})

Note the little lightning bolt in the notes section signifying that Cloud Acceleration is on.

Let’s attempt to download the presentation again.

![blog image]({{ "/assets/sfdc10.png" | absolute_url }})

Yeah, I think you could call that faster…

But that is not all, because we are using the same proven Steelhead technology including byte-level deduplication I can edit the PowerPoint file and upload it back to salesforce.com with a minimum of data transfer across the cloud.

![blog image]({{ "/assets/sfdc11.png" | absolute_url }})

I edited the first slide by changing the title and subtitle and will upload the changed file to my SaaS application, notice that the filename itself is also changed.

![blog image]({{ "/assets/sfdc12.png" | absolute_url }})

Looking at the current connections on the Steelhead you can see I am uploading the file at the same breakneck speed since I only need to transfer the changed bytes.

![blog image]({{ "/assets/sfdc13.png" | absolute_url }})

So there you have it, Salesforce.com at lightning speeds!

NOTE: I have not mentioned the SSL based configuration needed to allow us to optimize https based SaaS applications (as all of them are), I will cover this in a later post.

