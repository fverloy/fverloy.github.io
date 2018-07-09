---
layout: post
title:  "Unifying Docker Container and VM networking"
date:   2015-11-06 14:21:43 +0200
categories: SDN, Docker, Containers, Nuage Networks 
permalink: /2015/11/06/unifying-docker-container-and-vm-networking/
---

![blog image]({{ "/assets/dockerwhale.png" | absolute_url }})

**Introduction**

Most environments are not homogeneous, typically you have multiple types of workloads and I believe this will only increase in the near future with the rise of containers, PaaS, VM’s, bare metal,… In this brief overview I wanted to demonstrate how you can connect Virtual Machines and Containers on the same overlay network in an automated manner via our SDN solution. This way every time you spin up a new workload it will automatically get its network and security policy applied and behaves like any other endpoint on the network.

![blog image]({{ "/assets/dockervmnw1.png" | absolute_url }})

**Docker networking**

There are multiple options to do networking in Docker, typically a container (running a specific service) can be exposed externally by mapping an internal port to external one. When you install Docker, it creates three networks automatically (_bridge, null, and host_), when you run a container you can use the –net flag to specify which network you want to run a container on. By default the Docker daemon connects containers to the bridge network. If you run ifconfig on the host you can see the bridge as part of the host’s network stack.

The _none_ network adds a container to a container-specific network stack, this is what we will use in the case of Nuage Networks to connect the Docker container to our VRS (Open vSwitch)

The _host_ network adds a container on the hosts network stack. You’ll find the network configuration inside the container is identical to the host.

**Nuage Networks and Docker containers**

In the case of Nuage Networks we will attach every container to a tenant (overlay) network which is provided by our centralised management (VSD) and control (VSC) plane and configured on the Docker host in our VRS (Open vSwitch). This allows us to use our centralised networking and security policies providing IP configuration, firewall rules, QoS, etc. If traffic leaves the Docker host it is encapsulated in VXLAN so from a management point of view this no different then how we work with Virtual Machines.

**Demo**

I’ve created a L2 network (called DockerSN below) in Nuage (synced to OpenStack) where I’m connecting both my containers and VM workloads. The subnet has a range of 192.168.200.0/24.

![blog image]({{ "/assets/dockervmnw2.png" | absolute_url }})

So when I spin up a new container on my Docker host and connect it to the Nuage VRS I’ll automatically get the policies from that construct applied.

![blog image]({{ "/assets/dockervmnw3.png" | absolute_url }})

So as you can see above, my new container (gloomy_jang) has gotten the IP address 192.168.200.190, if we go back to our Virtualized Services Architect interface we can see 2 containers (one I created earlier) and a VM attached to the same subnet (could also be to a separate subnet ofc).

![blog image]({{ "/assets/dockervmnw4.png" | absolute_url }})

We can drill down on the newly created container and get all the network and security policy details

![blog image]({{ "/assets/dockervmnw5.png" | absolute_url }})

We now have connectivity between our container workloads and VM (192.168.200.2).

``[root@dockerhost ~]# docker exec 152f5660e56a ping -c 3 192.168.200.2
PING 192.168.200.2 (192.168.200.2) 56(84) bytes of data.
64 bytes from 192.168.200.2: icmp_seq=1 ttl=64 time=0.650 ms
64 bytes from 192.168.200.2: icmp_seq=2 ttl=64 time=0.450 ms
64 bytes from 192.168.200.2: icmp_seq=3 ttl=64 time=0.450 ms``