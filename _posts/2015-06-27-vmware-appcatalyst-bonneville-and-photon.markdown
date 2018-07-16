---
layout: post
title:  "VMware AppCatalyst, Bonneville, and Photon."
date:   2015-06-27 14:21:43 +0200
categories: VMware, AppCatalyst, Bonneville, Photon 
permalink: /2015/06/27/vmware-appcatalyst-bonneville-and-photon/
redirect_from:
  - /2015/06/27/vmware-appcatalyst-bonneville-and-photon/amp/
---
VMware has lot’s and lot’s of customers, running lot’s and lot’s of workloads, both dev and test workloads and production workloads, you know like, super duper important stuff that cannot, under any circumstance break.

As the whole DevOps “movement” makes clear both developers and operations teams have different requirements and different responsibilities. Developers want speed, the operations teams want stability, both are trying to respond to the demands of the business which wants faster time to market. Gross oversimplification I know, but since devops is getting a lot of attention lately you’ll have no problem digging up articles and blogposts on that.

![blog image]({{ "/assets/photon1.jpg" | absolute_url }})

Now as VMware’s customers rightfully expect, the idea is to try and marry both worlds and make these new methods enterprise grade without loosing the original benefits. I personally notice a lot of resistance in that enterprise customers understand the benefits, and some internal teams are pushing hard to incorporate these, but still a lot of people seem to adopt this “let’s wait and see / it’s just another fad” attitude.

![blog image]({{ "/assets/photon2.jpg" | absolute_url }})

VMware has been doing a lot of work in the past around making it’s products more “DevOps” friendly with things like vRealize CloudClient a command-line utility that provides verb-based access with a unified interface across vCloud Automation Center APIs, and vRealize Code Stream which provides release automation and continuous delivery to enable frequent, reliable software releases, while reducing operational risks.

> You don’t have meetings with other teams, you talk to their API instead.
> -Adrian Cockcroft

And now at the recent DockerCon in San Francisco VMware announced the tech-preview of AppCatalyst and Project Bonneville.

VMware AppCatalyst is an API and Command Line Interface (CLI)-driven MacOS type 2 hypervisor (based on VMware Fusion but without the GUI, 3D graphics support, virtual USB support, and Windows guest support) that is purpose-built for developers, with the goal of bringing the datacenter environment to the desktop. Currently a technology preview, VMware AppCatalyst offers developers a fast and easy way to replicate a private cloud locally on their desktop for building and testing containerized and microservices-based applications. The tool includes Project Photon (already announced in April), an open source minimal Linux container host, Docker Machine and integration with Vagrant. Panamax and Kitematic support are planned in the near future. AppCatalyst uses MacOS as its host operating system (i.e., the user must use MacOS 10.9.4 or later as their host operating system to use AppCatalyst).

You can download the Tech Preview of AppCatalyst here, it comes with an installer so it pretty easy to get up and running. Once it is installed AppCatalyst does not appear under your Applications folder, instead you can use your Terminal to navigate to /opt/vmware/appcatalyst

![blog image]({{ "/assets/photon3.png" | absolute_url }})

As mentioned above AppCatalyst comes pre-bundled with Project Photon – VMware’s compact container host Linux distribution. When you download AppCatalyst, you can point docker-machine at it, start up a Photon instance almost instantly (since there’s no Linux ISO to download), and start using Docker.

Another common use of the desktop hypervisor is with Vagrant. Developers build Vagrant files and then Vagrant up their deployment. Vagrant creates and configures virtual development environments, it can be seen as a higher-level wrapper to AppCatalyst. You can find the plugin for Vagrant here. (git clone https://github.com/vmware/vagrant-vmware-appcatalyst.git)

Since Project Photon is included in AppCatalyst it’s pretty easy to get started with deploying a Photon Linux Container Host.

``appcatalyst vm create photon1
Info: Cloned VM from ‘/opt/vmware/appcatalyst/photonvm/photon.vmx’ to ‘/Users/filipv/Documents/AppCatalyst/photon1/photon1.vmx’``

``appcatalyst vm list
Info: VMs found in ‘/Users/filipv/Documents/AppCatalyst’
photon1``

``appcatalyst vmpower on photon1
2015-06-27T10:18:38.530| ServiceImpl_Opener: PID 2949
Info: Completed power op ‘on’ for VM at ‘/Users/filipv/Documents/AppCatalyst/photon1/photon1.vmx’``

``appcatalyst guest getip photon1
192.168.2.128``

I can now SSH into the VM:

![blog image]({{ "/assets/photon4.png" | absolute_url }})

And securely launch a Docker container via Project Photon:

![blog image]({{ "/assets/photon5.png" | absolute_url }})

![blog image]({{ "/assets/photon6.png" | absolute_url }})

As mentioned before the idea is to interface with AppCatalyst via REST API calls, you can enable this by first starting the app catalyst-deamon and then going to port 8080 on your localhost.

![blog image]({{ "/assets/photon7.png" | absolute_url }})

once the deamon is running we can start to make REST API calls, for example retrieve the IP address of the Docker VM we previously created:

![blog image]({{ "/assets/photon8.png" | absolute_url }})

Since the last VMworld VMware has been talking about this concept of containers and VMs being better together, this kinda led to a lot of discussion about overhead of the hypervisor and each container needing it’s own OS, potential lock-in, etc. But again, this is where VMware is trying to marry Dev with Ops and make the use of containers feasible in the enterprise environment. Project Bonneville takes another step in this direction by making containers first class citizens on the vSphere hypervisor.

Bonneville orchestrates all the back-end systems: VM template (with Photon), storage, network, Docker image cache, etc. It can manage and configure native ESX storage and network primitives automatically as part of a container deploy.

Bonneville is a Docker daemon with custom VMware graph, execution and network drivers that delivers a fully-compatible API to vanilla Docker clients. The pure approach Bonneville takes is that the container is a VM, and the VM is a container. There is no distinction, no encapsulation, and no in-guest virtualization. All of the necessary container infrastructure is outside of the VM in the container host. The container is an x86 hardware virtualized VM – nothing more, nothing less.

![blog image]({{ "/assets/photon9.png" | absolute_url }})

Bonneville uses VMFork (Instant Clone / Project Fargo) to spin up a new VM every time a container is launched, by doing this the operations team now sees VM instances in it’s environment that it can treat, i.e. “operationlize”, just like regular Virtual Machines (Bonneville updates VM names and metadata fields for the container VMs it creates for full transparency in vCenter and any vSphere ecosystem products), and the obvious added benefit is that each container might be a VM but each container is not using a full blown linux host os to run. Instant Cloned VMs are powered on and fully booted in under a second and use no physical memory initially.

You can see a demo of Project Bonneville below:
[![Project Bonneville](http://img.youtube.com/vi/q0Xg7mVOO8o/0.jpg)](https://youtu.be/q0Xg7mVOO8o) 