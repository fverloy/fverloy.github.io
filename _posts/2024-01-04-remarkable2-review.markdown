---
layout: post
title: Why do you need API Security?
date: '2023-07-03 08:00:00'
tags:
- API
- Security
permalink: /why-do-you-need-api-security/
---
In 2011, Marc Andreessen wrote his seminal piece in the Wall Street Journal on "Why Software Is Eating The World", arguing that every company would become a software company. Today he is similarly singing AI's praises, and published his article ["Why AI Will Save the World"]("https://a16z.com/2023/06/06/ai-will-save-the-world/"), notably on his "own" website this time. But we are not here to talk AI, but to focus on APIs. To add to Mr. Andreessen's point on software, we feel that "APIs fuel the software that’s eating the world”.

## APIs are everywhere

Apple recently announced StandBy Mode in iOS 17 and demoed it at WWDC at Apple Park.

![APIs are everywhere](/assets/img/whyapi1.png)

Behind the scenes of most of our pretty consumer software interfaces lie APIs pulling together data and exchanging information between multiple software components to make everything run. (Note: since iOS 16, Apple uses the weatherKit API)

The same goes for most modern enterprise applications, the way you make payments to your customers, the way you interact with your shipping partner, the way you exchange patient data, the way you connect a micro-services application together, everything is API based.

The flip side of being everywhere is that people with bad intentions also want to use that against you. Instead of circumventing perimeter security controls, defeating XDR tools, leveraging a day-zero vulnerability, etc. to steal an organization's data, you can potentially abuse an API and metaphorically walk through the front door, grab all the data you want, and walk back out.

From a security perspective, APIs look like traditional web applications, which has led to many folks treating them the same way

![similar](/assets/img/whyapi2.png)

Since most modern APIs use HTTP methods to communicate, people have defaulted to evaluate the HTTP transactions between the API consumer and API provider on a per transaction basis. The reality is that malicious API traffic looks like good traffic, barring some individual transaction based attacks like SQLi, XSS, etc., for which a Web Application Firewall is the right tool.

The reality is that traditional inline tools like a Web Application Firewall lack the context of how APIs function, and potentially misfunction, to evaluate whether API communication is good instead of malicious.

![waf](/assets/img/whyapi3.png)

## APIs are the default attack vector

The API has become the default attack vector, as back-end infrastructure continues to evolve.

![default](/assets/img/whyapi4.png)

Even if we decide to change our infrastructure component based on current best practices or specific application needs and concerns, the way we access the dataset powering the applications is via an API, so the API beckons as the entry path for a malicious user.

From a security perspective, the API is also an evolving entity. We see the adoption of "newer" API types or application and infrastructure specific APIs, which makes doing any transactional security evaluation on them even more difficult.

![types](/assets/img/whyapi5.png)

Depending on the type of API, the number of API calls, their CRUD implementation, the used data types, etc. change, so any API security approach should differentiate between them and understand them in context.

Exacerbating this issue is that certain apps use multiple API types for a single app, like Twitter for example.

![twitter 1](/assets/img/whyapi6.png)

For example, if we perform a search operation in the Twitter web client, we see many different REST API endpoints (denoted by different URLs), their data is structured in json format and delivered back to the front-end browser client for display

![twitter 2](/assets/img/whyapi7.png)

Now if you want to create a tweet, the endpoint used is a GraphQL one, which is represented by a single URL for all functionality (how do you create inline firewall rules for this?). The different functionalities provided by that single GraphQL API are represented by queries that can be standardized, named, or unnamed.

## The need for a dedicated API security platform

So it is about pulling together all relevant context over time and understanding how all these API calls are associated with a single consumer, and what that consumer's behavior tells us about their intentions (good vs bad). To connect those dots, you need superhuman (i.e. AI/ML) capabilities. 