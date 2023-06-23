---
layout: post
title: Policy Based Abstractions through SDN
date: '2015-12-14 23:00:00'
tags:
- sdn
- networking
---

As I’m sure you’re tired of hearing by now IT is typically divided in multiple silo’s which don’t always see eye to eye. Sometimes people are afraid of needing to adjust perceived best practises in their own domain to better collaborate with the rest of the organization, in many cases though it’s simply a matter of not understanding each other because you are not speaking the same language.

The ideal scenario would be a world where each practise would expose it’s infrastructure, build on best practises, through APIs so other teams can interact with it in the most optimum way.

At Nuage Networks we provide API based access to our components making full scale automation a possibility but we can also bring together teams speaking different languages via our abstraction based policies.

**Nuage Networks Application Designer**

Application Designer is built for use by people with an understanding of application constructs that don’t necessarily need to understand, or care about, the underlying networking constructs, these are automatically abstracted by the Nuage platform.

In this example we initially start of with a fresh slate, no network constructs have been created beyond the L3 domain.

<img src="/assets/img/appd1.png">

If we go to Application Designer we can see the application services that are available, these would typically be created by the network team, it is an abstract representation of a network service, for example below we are creating the application service https, providing TCP communication to port 443.

<img src="/assets/img/appd2.png">

The application team can now use these application service abstractions to build out their application. In the example below we start by creating 3-tier application called Banking App.

<img src="/assets/img/appd3.png">

Next we can start to add our application tiers and interconnect them by using the application services abstractions that were previously created by the networking team. You do this by dragging and dropping items from the library onto the canvas.

<img src="/assets/img/appd4.png">

Once you have your application tiers mapped out you can use the application services to create flow security policy (what type of traffic is allowed between these 2 points) simply by drawing a line between the 2 tiers.

<img src="/assets/img/appd5.png">

In this case we are indicating we want HTTPS to be allowed from the Internet to the front-end application tier.

One you have your application mapped out and interconnected (you could also drag and drop other complete application on the canvas and specify connectivity between those as wel) you can add workloads to the tiers, these will then to the policies you have applied.

<img src="/assets/img/appd6.png">

Since the the system will translate these different abstractions to the correct networking constructs we can look at the network design and verify that our application model has been completely mapped to a set of networking policies.

<img src="/assets/img/appd7.png">

Furthermore, looking at the security policies we can see these have been translated as wel thus making it easy for different teams with different knowledge domains to focus on their area of expertise while at the same time tying everything together via our policy based abstractions.

<img src="/assets/img/appd8.png">