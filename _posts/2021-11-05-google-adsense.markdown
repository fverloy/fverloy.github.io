---
layout: post
title: Cloudflare Serverless fix to Google AdSense on the Ghost.io Starter Plan.
date: '2021-11-05 09:56:38'
tags:
- cloudflare
- serverless
- ghost-io
---

First of all, yes ads suck. I recently started using Google AdSense on my blog to recoup the costs of Ghost.io. I switched to Ghost from GitHub Pages which is completely free and a great way to host a static blog. I wanted some more flexibility however and also wanted a newsletter subscription service so I found Ghost.io. I'm currently on their Starter Plan which does not allow you to upload an ads.txt file from Google AdSense.

If you don't have the ads.txt file Google AdSense displays a warning in your AdSense portal that your earnings are at risk, Authorised Digital Sellers, or ads.txt is an [IAB Tech Lab initiative](https://iabtechlab.com/ads-txt/) that helps ensure that your digital ad inventory is only sold through sellers (such as AdSense) who you've identified as authorised. Creating your own ads.txt file gives you more control over who's allowed to sell ads on your site and helps prevent counterfeit inventory from being presented to advertisers.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.26.43.png" class="kg-image" alt loading="lazy" width="2000" height="467" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.26.43.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.26.43.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-05-at-10.26.43.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/11/Screenshot-2021-11-05-at-10.26.43.png 2400w" sizes="(min-width: 720px) 720px"></figure>

Since I can't fix it in Ghost directly (you can on the more expensive plans, just not the starter plan) I needed to look for a workaround, the fine folks over at Ghost pointed me to Cloudflare which I was already using as a registrar, DNS, and for DDOS protection. They also provide the abilitly to deploy serverless code using their [Cloudflare Workers](https://workers.cloudflare.com/) service, this allows me to deploy JavaScript to Cloudflare's edge, which ultimatly provides a way to host and manage my own ads.txt file, yay!

How it works

First of you need a to use Cloudflare for DNS services for your domain, it is free and works wonderfully.

In the Cloudflare portal click on 'Workers'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.10.09.png" class="kg-image" alt loading="lazy" width="2000" height="1217" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.10.09.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.10.09.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-05-at-10.10.09.png 1600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.10.09.png 2140w" sizes="(min-width: 720px) 720px"></figure>

Setup your free Cloudflare Workers subdomain by selecting your domain and clicking 'Set up'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.10.19.png" class="kg-image" alt loading="lazy" width="1814" height="1460" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.10.19.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.10.19.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-05-at-10.10.19.png 1600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.10.19.png 1814w" sizes="(min-width: 720px) 720px"></figure>

There is a free plan, with some limitations of course, or you can opt to go for the Pay-as-you-go model. Select the one you want and click (in my case) 'Continue with Free'.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.10.42.png" class="kg-image" alt loading="lazy" width="1820" height="1714" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.10.42.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.10.42.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-05-at-10.10.42.png 1600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.10.42.png 1820w" sizes="(min-width: 720px) 720px"></figure>

Now download your ads.txt file from your Google AdSense portal and open it. Copy and paste your values in the script below.

    async function handleRequest(request) {
      const init = {
        headers: {
          'content-type': 'text/plain',
        },
      }
      return new Response(adstxt, init)
    }
    addEventListener('fetch', event => {
      return event.respondWith(handleRequest(event.request))
    })
    const adstxt = `#
    google.com, pub-xxxxxxxxxxx, DIRECT, fxxxxxxxxx 
    `;

next click 'Save and Deploy'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.14.18.png" class="kg-image" alt loading="lazy" width="1022" height="554" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.14.18.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.14.18.png 1000w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.14.18.png 1022w" sizes="(min-width: 720px) 720px"></figure>

Now back to your domain and select 'Workers' and click 'Add route'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.15.34.png" class="kg-image" alt loading="lazy" width="2000" height="931" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.15.34.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.15.34.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-05-at-10.15.34.png 1600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.15.34.png 2140w" sizes="(min-width: 720px) 720px"></figure>

Set the Route to https://domain.name/ads.txt and select the Worker you just created, then click 'Save'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.16.00.png" class="kg-image" alt loading="lazy" width="1006" height="872" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.16.00.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.16.00.png 1000w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.16.00.png 1006w" sizes="(min-width: 720px) 720px"></figure>

Verify everything is set as expected.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.16.11.png" class="kg-image" alt loading="lazy" width="2000" height="1009" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-10.16.11.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-10.16.11.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-05-at-10.16.11.png 1600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-10.16.11.png 2132w" sizes="(min-width: 720px) 720px"></figure>

Now go to your website https://domain.name/ads.txt and verify if the file is being served. Done! As if I needed another reason to love Cloudflare 🤗

The Google AdSense portal will take a few days to update the ads.txt notification.

