---
layout: post
title: Protect Rubrik CDM with Microsoft Authenticator
date: '2021-03-18 10:16:00'
tags:
- ransomware
- rubrik
- backup
---

Rubrik released version 5.3.1 of the RCDM code a couple of days ago and one of the new feautures is the ability to protect all of your Rubrik login accounts with two-step verification by using the industry standard [time-based OTP](https://en.wikipedia.org/wiki/Time-based_One-Time_Password)(one-time password) codes.

The Microsoft Authenticator app has been steadily extended in functionality as well, [recently adding Microsoft account-based autofill capabilities across platforms.](https://www.thurrott.com/mobile/245091/microsoft-authenticator-gains-password-management-and-autofill-capabilities)

Setting it up is really straightforward, on the Rubrik UI click on your username you just logged in with and select “Two-Step Verification Configuration”

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/auth0.png" class="kg-image" alt loading="lazy" width="514" height="332"></figure>

The configuration wizard shows some third party applications we recommend, including Microsoft Authenticator.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/auth1.png" class="kg-image" alt loading="lazy" width="842" height="722" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/auth1.png 600w, __GHOST_URL__ /content/images/2021/08/auth1.png 842w" sizes="(min-width: 720px) 720px"></figure>

Next use the Microsoft Authenticator app to scan the QR code to setup the relationship, select “add account –\> personal account”, then click on “Scan QR code” and point your phone camera to the Rubrik GUI.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/IMG_1342.PNG" class="kg-image" alt loading="lazy" width="1125" height="2436" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/IMG_1342.PNG 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/IMG_1342.PNG 1000w, __GHOST_URL__ /content/images/2021/08/IMG_1342.PNG 1125w" sizes="(min-width: 720px) 720px"></figure>

Type in the OTP code displayed in your Microsoft Authenticator app into the OTP field on the Rubrik UI.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/auth2.png" class="kg-image" alt loading="lazy" width="848" height="1320" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/auth2.png 600w, __GHOST_URL__ /content/images/2021/08/auth2.png 848w" sizes="(min-width: 720px) 720px"></figure>

Next time you log into RCDM you will be prompted for your OTP code.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/auth6.png" class="kg-image" alt loading="lazy" width="2000" height="1018" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/auth6.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/auth6.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/auth6.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/auth6.png 2400w" sizes="(min-width: 720px) 720px"></figure>

Open your Microsoft Authenticator app to retrieve the code and type it in and you are all set.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/IMG_1344.PNG" class="kg-image" alt loading="lazy" width="1125" height="2436" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/IMG_1344.PNG 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/IMG_1344.PNG 1000w, __GHOST_URL__ /content/images/2021/08/IMG_1344.PNG 1125w" sizes="(min-width: 720px) 720px"></figure>

You have now achieved another step towards your passwordless journey!

