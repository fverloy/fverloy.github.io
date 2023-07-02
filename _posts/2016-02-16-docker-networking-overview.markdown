---
layout: post
title: Docker networking overview
date: '2016-02-16 23:00:00'
tags:
- containers
- sdn
- docker
permalink: /docker-networking-overview/
---

 **Introduction**

There are of course a lot of blog posts out there already regarding Docker networking, I don’t want to replicate that work but instead wanted to provide a clear overview of what is possible with Docker networking today by showing some examples of the different options.

In general the networking piece of Docker, and arguably Docker itself, is still quite young so things move fast and will likely change over time. A lot of progress has been made via the SocketPlane acquisition last year and it’s subsequent pluggable model, but more about that later.

Docker containers are ephemeral by design (pets vs cattle), this leads to several potential issues, not the least of which is not being able to keep your firewall configuration up to date because of difficult IP address management, it’s also hard to connect to services that might disappear at any moment, and no, using DNS as a stopgap is not a good solution (DNS as a SPOF, don’t go there). Of course there are several options and methods available to overcome these things.

**Single host Docker networking**

You basically have 4 options for single host Docker networking; Bridge mode, Host mode, Container mode, and No networking.

**Bridge mode (the default Docker networking mode)**

The Docker deamon creates “docker0” a virtual ethernet bridge that forwards packets between all interfaces attached to it. All containers on the host are attached to this internal bridge which assings one interface as the containers’ “eth0” interface and another interface in the host’s namespace (think VRF). The container get’s a private IP address assignment. To prevent ARP collisions on the local network, the Docker daemon generates a random MAC address from the allocated IP address. In the example below Docker assigns the private IP 172.17.0.1 to the container.

<img src="/assets/img/docker11.png">

**Host mode**

In this mode the container shares the networking namespace of the host, directly exposing it to the outside world. This means you need to use port mapping to reach services inside the container, in Bridge mode, Docker can automatically assign ports and thus make them routable. In the example below the Docker host has the IP 10.0.0.4 and as you can see the container shares this IP address.

<img src="/assets/img/docker2.png">

**Container mode**

This mode forces Docker to reuse the networking namespace of another container. This is used if you want to provide custom networking from said container, this is for example what Kubernetes uses to provide networking for multiple containers. In the example below the container to which we are going to connect the subsequent containers into has the IP 172.17.0.2 and as you can see the container being launched has the same IP address.

<img src="/assets/img/docker3.png">

<img src="/assets/img/docker4.png">

**No networking**

This mode does not configure networking, useful for containers that don’t require network access, but it can also be used to setup custom networking. This is the mode Nuage Networks leverages pre-Docker 1.9 (more info here). In the example below you can see that our new container did not get any IP address assigned.

<img src="/assets/img/docker5.png">

By default Docker has inter-container communication enabled (–icc=true) meaning that containers on a host are free to communicate without restrictions which could be a security concern. Communication to the outside world is controlled via iptables and ip\_forwarding.

**Multi-host Docker networking**

In a real world scenario you will most likely end up using Docker containers across multiple hosts depending on the needs of you containerized application. So now you need to build container networks across these hosts to have you distributed application communicate internally, and externally.

As alluded to above in march of 2015 Docker, Inc. acquired the SDN startup SocketPlane, that has given rise to Libnetwork and the Container Network Model, meant to be the default multi-host networking setup going forward.

**Libnetwork**

Libnetwork provides a native Go implementation for connecting containers The goal of libnetwork is to deliver a robust Container Network Model that provides a consistent programming interface and the required network abstractions for applications.

One of the benefits of Libnetwork is that it uses a driver / plugin model to support many underlying network technologies while stil exposing a simple and consistent network model to the end-user (common API), Nuage Networks leverages this model by having a remote plugin.

Libnetwork also introduces the Container Network Model (CNM) to provide interoperation between networks and containers.

<img src="/assets/img/lnw.png">

The CNM defines a network sandbox, and endpoint and a network. The Network Sandbox is an isolated environment where the Networking configuration for a Docker Container lives. The Endpoint is a network interface that can be used for communication over a specific network. Endpoints join exactly one network and multiple endpoints can exist within a single Network Sandbox. And the Network is a uniquely identifiable group of endpoints that are able to communicate with each other. You could create a “Frontend” and “Backend” network and they would be completely isolated.

