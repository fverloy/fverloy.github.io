---
layout: post
title:  "VMware Horizon DaaS and Google Chromebooks"
date:   2014-02-13 14:21:43 +0200
categories: VMware, Google, Chromebooks
permalink: /2014/02/13/vmware-horizon-daas-and-google-chromebooks/
redirect_from:
  - /2014/02/13/vmware-horizon-daas-and-google-chromebooks/amp/
---
Yesterday at VMware’s Partner Exchange (PEX), VMware announced that it is joining forces with Google to modernize corporate desktops for the Mobile Cloud Era by providing businesses with secure, cloud access to Windows applications, data and Desktops on Google Chromebooks.

In this post I want to provide an overview of what this means for VMware’s customers.

So first things first, what exactly is the “Mobile Cloud Era”?

If we look back (and generalise a bit) we can define two big computing architectures that defined IT in the past, these are the Mainframe era and the Client-Server era, more recently we are moving more and more towards a third computing architecture called the Mobile-Cloud era.

![blog image]({{ "/assets/daas1.png" | absolute_url }})

The mainframe era was defined by highly centralised, highly controlled IT infrastructures that were meant to connect a relatively small user population to a small number of applications (mainly data processing).
In the client-server era the focus shifted to decentralisation and powering a large number of different types of applications each requiring (or so the thinking was) it’s own silo. The mobile-cloud era is all about letting billions of users consume IT as a Service, enabling access to any application, be it on-premises or in the cloud, from any location on any device.

![blog image]({{ "/assets/daas2.png" | absolute_url }})

So why are VMware and Google doing this?

I don’t think the actual delivery mechanism matters that much, we are in the fast moving/changing mobile cloud era and device choices are fluid, what really matters is the application. And, like it or not, most corporate customers are still relying heavily on the Microsoft Office suite to perform their day to day business.

Now, there is a transition under way (the cloud piece in the mobile cloud era) towards SaaS based productivity applications like Google Apps, which is why ChromeOS and the Chromebook exist in the first place. This transition will neither be all encompassing (in my humble opinion) and immediate, so until we get there this is a great option.

What is VMware Horizon DaaS?

During last year’s VMworld Europe in Barcelona VMware announced the acquisition of a company called Desktone. Desktone delivers desktops (DaaS) and applications as a service. The desktops can either be persistent Windows 7/8 desktops (so the actual Windows client-os, compatible with all your applications – it can also be a Linux desktop if you want) or Windows RDSH based desktops (the option you have here is that you can use the Windows server OS to get round the Microsoft VDI licensing snafu to potentially allow for additional cost-savings), or Remote Applications. The Desktone solutions have recently been brought under the Horizon umbrella, having been renamed to VMware Horizon DaaS.

![blog image]({{ "/assets/daas3.jpg" | absolute_url }})

You can access these services using PCoIP or RDP, this in contrast to the joint Google solution announced yesterday which will leverage VMware’s HTML5 Blast protocol for remote access.

What is the solution announced yesterday?

The combination of Horizon DaaS as a “back-end” and ChromeOS/Chromebooks as the “front-end” allows browser based, via the HTML5 Blast protocol, access to Windows desktops and applications via Google Chromebooks.

The Chromebooks are a low cost, “cloud first” form factor that combined with Horizon DaaS allow you to fully utilise this in a corporate setting. Different low-cost hardware options exist by vendors like Dell, HP, Samsung and Acer. Google itself also has a higher-end (if you want to maintain a low TCO for this combined solution maybe not for you) Chromebook called the Pixel.

![blog image]({{ "/assets/daas4.jpg" | absolute_url }})

Initially the solution will be available as an on-premises service, the joint solution is expected to be delivered as a fully managed, subscription DaaS offering by VMware and other vCloud Service Provider Partners, in the public cloud or within hybrid deployments.

Demo of Windows Desktops and Apps as a Cloud service on Chromebooks

 [![DaaS](http://img.youtube.com/vi/1P0y09vR-L4/0.jpg)](https://youtu.be/1P0y09vR-L4)  