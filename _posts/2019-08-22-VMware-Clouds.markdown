---
layout: post
title:  "VMware Cloud on AWS, Azure, and GCP"
date:   2019-08-22 07:00:00 +0200
categories: Blog, VMware, Azure, GCP, Cloudsimple
permalink: /2019/08/22/VMware-Clouds/
---
![blog image]({{ "/assets/boy-standing-on-mountain-looking-at-clouds.jpg" | absolute_url }})

With the recent announcement of the Google Cloud VMware Solution you now have the possibility to run a VMware stack on each of the 3 major hyperscalers, AWS, Azure, and GCP. In this post I wanted to compare and contrast the 3 solutions, but before we do that let's discuss why you would want to use either 3 of them if you are an enterprise customer today.

The benefits to running VMware in your private datacenter are well established and many IT admins have built their career on learning and implementing the VMware SDDC. They understand how to architect and operationalize the VMware stack to maximize effiency in a private cloud setting. When looking at the public cloud there are a couple of things that could be very compelling for enterprise customers, the global infrastructure spanning many locations and availability zones provides global reach and resiliency, and the prospect of being able to more easily incorporate specific public cloud services provides to existing brown field deployments provides interesting scenarios, especially since you can consume it in a pay-as-you-go model. 

All 3 solutions are powered by VMware Cloud Foundation which is an integrated stack, using SDDC Manager for lifecycle management of all individual component of the stack, to bundle compute (vSphere), storage (VMware vSAN) and network virtualization (NSX) into a single platform that can be deployed on-premises as a private cloud or run as a service within a public cloud. It is available in 5 different editions providing an increasing set of enterprise capabilities.

**VMware Cloud on AWS**

This solution has been cooking the longest out of all 3 and certainly seems to be more rounded out at the moment, VMware announced a stratigic partnership with AWS in 2016 and this resulted a.o. things in the initial availability of VMware Cloud on AWS in august of 2017.

**Azure VMware Solution by CloudSimple**

NOTE: there is also a upcoming (coming later this year according to Microsoft) Azure VMware Solution by Virtustream

**Google Cloud VMware Solution by CloudSimple**



