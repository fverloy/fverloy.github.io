---
layout: post
title:  "SSL Acceleration"
date:   2013-02-11 14:21:43 +0200
categories: Riverbed, SSL
permalink: /2013/02/11/ssl-acceleration/
redirect_from:
  - /2013/02/11/ssl-acceleration/amp/
---
One of the prerequisites for WAN optimization is that the traffic we are attempting to de-duplicate across the WAN is not encrypted, we need “clear-text” data in order to find data patterns so de-duplication is most optimum.

But Steelhead can optimize SSL encrypted data by applying the same optimization methods, while still maintaining end-to-end security and keeping the trust model intact.

To better understand how we perform SSL optimization let’s first look at a simple example of requesting a secured webpage from a webserver.

![blog image]({{ "/assets/ssl1.png" | absolute_url }})

The encryption using a private key/public key pair ensures that the data can be encrypted by one key but can only be decrypted by the other key pair. The trick in a key pair is to keep one key secret (the private key) and to distribute the other key (the public key) to everybody.

All of the same optimizations that are applied to normal non-encrypted TCP traffic, you can also apply to encrypted SSL traffic. Steelhead appliances accomplish this without compromising end-to-end security and the established trust model. Your private keys remain in the data center and are not exposed in the remote branch office location where they might be compromised.

The Riverbed SSL solution starts with Steelhead appliances that have a configured trust relationship, enabling the them to exchange information securely over their own dedicated SSL connection. Each client uses unchanged server addresses and each server uses unchanged client addresses; no application changes or explicit proxy configuration is required. Riverbed uses a unique technique to split the SSL handshake.

The handshake (the sequence depicted above) is the sequence of message exchanges at the start of an SSL connection. In an ordinary SSL handshake, the client and server first establish identity using public-key cryptography, and then negotiate a symmetric session key to use for data transfer. When you use Riverbed’s SSL acceleration, the initial SSL message exchanges take place between the Web browser and the server- side Steelhead appliance. At a high level, Steelhead appliances terminate an SSL connection by making the client think it is talking to the server and making the server think it is talking to the client. In fact, the client is talking securely to the Steelhead appliances. You do this by configuring the server-side Steelhead appliance to include proxy certificates and private keys for the purpose of emulating the server.

![blog image]({{ "/assets/ssl2.png" | absolute_url }})

When the Steelhead appliance poses as the server, there does not need to be any change to either the client or the server. The security model is not compromised—the optimized SSL connection continues to guarantee server-side authentication, and prevents eavesdropping and tampering. The connection between the two Steelheads is secured by the use of secure peering (a separate SSL tunnel running between the two appliances).

