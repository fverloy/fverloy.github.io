---
layout: post
title: Cloudflare Serverless fix to Google AdSense on the Ghost.io Starter Plan.
# subtitle: Excerpt from Soulshaping by Jeff Brown
cover-img: /assets/img/photo-1536173257297-533e34457207.jpeg
thumbnail-img: /assets/img/photo-1536173257297-533e34457207.jpeg
share-img: /assets/img/photo-1536173257297-533e34457207.jpeg
tags: [cloudflare, google, adsense, serverless]
permalink: /google-adsense/
---
First of all, yes ads suck. I recently started using Google AdSense on my blog to recoup the costs of Ghost.io. I switched to Ghost from GitHub Pages which is completely free and a great way to host a static blog. I wanted some more flexibility however and also wanted a newsletter subscription service so I found Ghost.io. I'm currently on their Starter Plan which does not allow you to upload an ads.txt file from Google AdSense.

If you don't have the ads.txt file Google AdSense displays a warning in your AdSense portal that your earnings are at risk, Authorised Digital Sellers, or ads.txt is an IAB Tech Lab initiative that helps ensure that your digital ad inventory is only sold through sellers (such as AdSense) who you've identified as authorised. Creating your own ads.txt file gives you more control over who's allowed to sell ads on your site and helps prevent counterfeit inventory from being presented to advertisers.

![cloudflare1](/assets/img/Screenshot-2021-11-05-at-10.26.43.png)

Since I can't fix it in Ghost directly (you can on the more expensive plans, just not the starter plan) I needed to look for a workaround, the fine folks over at Ghost pointed me to Cloudflare which I was already using as a registrar, DNS, and for DDOS protection. They also provide the abilitly to deploy serverless code using their Cloudflare Workers service, this allows me to deploy JavaScript to Cloudflare's edge, which ultimatly provides a way to host and manage my own ads.txt file, yay!

How it works

First of you need a to use Cloudflare for DNS services for your domain, it is free and works wonderfully.

In the Cloudflare portal click on 'Workers'

![cloudflare2](/assets/img/Screenshot-2021-11-05-at-10.10.09.png)

Setup your free Cloudflare Workers subdomain by selecting your domain and clicking 'Set up'

![cloudflare3](/assets/img/Screenshot-2021-11-05-at-10.10.19.png)

There is a free plan, with some limitations of course, or you can opt to go for the Pay-as-you-go model. Select the one you want and click (in my case) 'Continue with Free'.

![cloudflare4](/assets/img/Screenshot-2021-11-05-at-10.10.42.png)

Now download your ads.txt file from your Google AdSense portal and open it. Copy and paste your values in the script below.

```javascript
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
const adstxt =  `#
google.com, pub-xxxxxxxxxxx, DIRECT, fxxxxxxxxx 
`;
```
next click 'Save and Deploy'

![cloudflare5](/assets/img/Screenshot-2021-11-05-at-10.14.18.png)

Now back to your domain and select 'Workers' and click 'Add route'

![cloudflare6](/assets/img/Screenshot-2021-11-05-at-10.15.34.png)

Set the Route to https://domain.name/ads.txt and select the Worker you just created, then click 'Save'

![cloudflare7](/assets/img/Screenshot-2021-11-05-at-10.16.00.png)

Verify everything is set as expected.

![cloudflare8](/assets/img/Screenshot-2021-11-05-at-10.16.11.png)

Now go to your website https://domain.name/ads.txt and verify if the file is being served. Done! As if I needed another reason to love Cloudflare 🤗

The Google AdSense portal will take a few days to update the ads.txt notification.