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

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd1.png" class="kg-image" alt loading="lazy" width="1108" height="563" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd1.png 1000w, __GHOST_URL__ /content/images/2021/08/appd1.png 1108w" sizes="(min-width: 720px) 720px"></figure>

If we go to Application Designer we can see the application services that are available, these would typically be created by the network team, it is an abstract representation of a network service, for example below we are creating the application service https, providing TCP communication to port 443.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd2.png" class="kg-image" alt loading="lazy" width="1108" height="1117" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd2.png 1000w, __GHOST_URL__ /content/images/2021/08/appd2.png 1108w" sizes="(min-width: 720px) 720px"></figure>

The application team can now use these application service abstractions to build out their application. In the example below we start by creating 3-tier application called Banking App.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd3.png" class="kg-image" alt loading="lazy" width="1108" height="1133" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd3.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd3.png 1000w, __GHOST_URL__ /content/images/2021/08/appd3.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Next we can start to add our application tiers and interconnect them by using the application services abstractions that were previously created by the networking team. You do this by dragging and dropping items from the library onto the canvas.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd4.png" class="kg-image" alt loading="lazy" width="1108" height="659" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd4.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd4.png 1000w, __GHOST_URL__ /content/images/2021/08/appd4.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Once you have your application tiers mapped out you can use the application services to create flow security policy (what type of traffic is allowed between these 2 points) simply by drawing a line between the 2 tiers.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd5.png" class="kg-image" alt loading="lazy" width="1108" height="852" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd5.png 1000w, __GHOST_URL__ /content/images/2021/08/appd5.png 1108w" sizes="(min-width: 720px) 720px"></figure>

In this case we are indicating we want HTTPS to be allowed from the Internet to the front-end application tier.

One you have your application mapped out and interconnected (you could also drag and drop other complete application on the canvas and specify connectivity between those as wel) you can add workloads to the tiers, these will then to the policies you have applied.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd6.png" class="kg-image" alt loading="lazy" width="1108" height="712" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd6.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd6.png 1000w, __GHOST_URL__ /content/images/2021/08/appd6.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Since the the system will translate these different abstractions to the correct networking constructs we can look at the network design and verify that our application model has been completely mapped to a set of networking policies.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd7.png" class="kg-image" alt loading="lazy" width="1108" height="483" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd7.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd7.png 1000w, __GHOST_URL__ /content/images/2021/08/appd7.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Furthermore, looking at the security policies we can see these have been translated as wel thus making it easy for different teams with different knowledge domains to focus on their area of expertise while at the same time tying everything together via our policy based abstractions.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/appd8.png" class="kg-image" alt loading="lazy" width="1108" height="341" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/appd8.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/appd8.png 1000w, __GHOST_URL__ /content/images/2021/08/appd8.png 1108w" sizes="(min-width: 720px) 720px"></figure>