---
layout: post
title: Apple Studio Display VPN Issue.
tags: [Apple, VPN]
date: '2023-05-31 11:45:00'
cover-img: /assets/img/asd.jpeg
thumbnail-img: /assets/img/asd.jpeg
share-img: /assets/img/asd.jpeg
---

The Apple Studio Display is a bit of a strange device, it is basically a large iPad powered by an A13 Bionic chip. It also messes up your VPN services.

I wanted to try out Google Bard, but since I'm based in Belgium, and the EU is pretty negative on Generative AI at the moment Bard is not available here.

![Google Bard Belgium](/assets/img/bard.png)

Not to worry, this is where a VPN comes in!

Not so fast!

Turns out that the Apple Studio Display uses a network connection called "Studio Display" with a self-assigned IP to communicate with your Mac and retrieve iPadOS updates. 

I'm using ProtonVPN which tries to use that "internal" connection to establish a VPN connection which doesn't work. 

A way around this is to change the service order of your network interfaces in MacOS, to do that go to "system settings" --> "Network" and select the dropdown at the bottom of the screen (...) 

![service order](/assets/img/nw.png)

Put your actual network connection at the top and you should be good to go.

![service order](/assets/img/bard2.png)

Now you can go ahead and play with Bard. 