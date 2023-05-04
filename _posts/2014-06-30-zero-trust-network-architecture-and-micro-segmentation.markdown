---
layout: post
title: Zero Trust Network Architecture and Micro-Segmentation
date: '2014-06-30 22:00:00'
tags:
- networking
---

 **A killer application**

As defined by Wikipedia: In marketing terminology, a killer application (commonly shortened to killer app) is any computer program that is so necessary or desirable that it proves the core value of some larger technology, such as computer hardware, gaming console, software, a programming language, software platform, or an operating system. In other words, customers would buy the underlying technology just to run that application.

**The Zero Trust Network Architecture**

There is a simple philosophy at the core of Zero Trust: Security professionals must stop trusting packets as if they were people. Instead, they must eliminate the idea of a trusted network (usually the internal network) and an untrusted network (external networks). In Zero Trust, all network traffic is untrusted. Thus, security professionals must verify and secure all resources, limit and strictly enforce access control, and inspect and log all network traffic.

The core concepts of Zero Trust are:

- There is no longer a trusted and an untrusted interface on our security devices.
- There is no longer a trusted and an untrusted network.
- There are no longer trusted and untrusted users.

Zero Trust mandates that information security pros treat all network traffic as untrusted. Zero Trust doesn’t say that employees are untrustworthy but that trust is a concept that information security pros should not apply to packets, network traffic, and data. The malicious insider reality demands a new trust model. By changing the trust model, we reduce the temptation for insiders to abuse or misuse the network, and we improve our chances of discovering security breaches before they impact the environment.

These approaches wrap security controls around much smaller groups of resources – often down to a small group of virtualized resources or individual VMs. Micro-segmentation has been understood to be a best practice approach from a security perspective, but difficult to apply in traditional environments.

**Micro-segmentation**

Traditionally, network segmentation is a function of a switch. From a network perspective micro-segmentation is a design where each device on a network gets its own dedicated segment (collision domain) to the switch. Each network device gets the full bandwidth of the segment and does not have to share the segment with other devices. Micro-segmentation reduces and can even eliminate collisions because each segment is its own collision domain.

From a security perspective traditional segmentation works by implementing (next-generation) firewalls that act as “choke points” on the network. When application traffic is directed towards the firewall it enforces it’s rule-set and packets are blocked or allowed to pass through. This is a completely workable solution if you are just implementing controls (choke points) at limited places in the network, i.e. between the internal and external network, between business networks and the production network etc. If you would apply this same method to micro-segmentation however you would need to invest in large network boxes and with the advent of VM mobility you would potentially need to constantly update security rules to keep your policies up to date.

**VMware NSX and micro-segmentation**

In Virtual Machine land we would normally connect VMs to a virtual switch (port-group) and pipe that combined traffic to a network choke point, i.e. firewall, this breaks the idea of the Zero Trust architecture as you are already grouping certain VMs together. With NSX, policy can be applied to the individual VM level independent of placement. Every VM is literally first connected to the in-kernel statefull firewall before traffic goes out on the network. This implies that security can be implemented independent of the way the logical network is architected, i.e. it does not matter if this particular VM is grouped with other VM’s in say a DMZ segment, traffic will be filtered between VM’s making sure Zero Trust is maintained.

