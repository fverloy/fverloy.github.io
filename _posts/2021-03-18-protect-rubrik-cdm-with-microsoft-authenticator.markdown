---
layout: post
title: Protect Rubrik CDM with Microsoft Authenticator
date: '2021-03-18 10:16:00'
tags:
- ransomware
- rubrik
- backup
permalink: /protect-rubrik-cdm-with-microsoft-authenticator/
---

Rubrik released version 5.3.1 of the RCDM code a couple of days ago and one of the new feautures is the ability to protect all of your Rubrik login accounts with two-step verification by using the industry standard [time-based OTP](https://en.wikipedia.org/wiki/Time-based_One-Time_Password)(one-time password) codes.

The Microsoft Authenticator app has been steadily extended in functionality as well, [recently adding Microsoft account-based autofill capabilities across platforms.](https://www.thurrott.com/mobile/245091/microsoft-authenticator-gains-password-management-and-autofill-capabilities)

Setting it up is really straightforward, on the Rubrik UI click on your username you just logged in with and select “Two-Step Verification Configuration”

<img src="/assets/img/auth0.png">

The configuration wizard shows some third party applications we recommend, including Microsoft Authenticator.

<img src="/assets/img/auth1.png">

Next use the Microsoft Authenticator app to scan the QR code to setup the relationship, select “add account –\> personal account”, then click on “Scan QR code” and point your phone camera to the Rubrik GUI.

<img src="/assets/img/IMG_1342.png">

Type in the OTP code displayed in your Microsoft Authenticator app into the OTP field on the Rubrik UI.

<img src="/assets/img/auth2.png">

Next time you log into RCDM you will be prompted for your OTP code.

<img src="/assets/img/auth6.png">

Open your Microsoft Authenticator app to retrieve the code and type it in and you are all set.

<img src="/assets/img/IMG_1344.png">

You have now achieved another step towards your passwordless journey!

