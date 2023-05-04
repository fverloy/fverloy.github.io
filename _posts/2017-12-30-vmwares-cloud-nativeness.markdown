---
layout: post
title: VMware’s Cloud Nativeness
date: '2017-12-30 23:00:00'
tags:
- vmware
- cloudnative
---

At first glance it feels like VMware’s efforts related to cloud native applications are meant to cater to the traditional admins, people who are mainly running virtual machines today but want, or need, to support other types of workloads as well. People who secretly don’t “get” this new container hotness\* and really want to manage their environment with the tools and knowledge that got them where they are today and are very comfortable with. Or to put it another way, VMware is ultimately trying to cater to both the infrastructure/operations person needing to support these new types of workloads and the developer building them.

Before looking at VMware’s portfolio related to cloud native applications let’s take a step back and examine both what CNA means and what drives it. According to Pivotal, Cloud-native is an approach to building and running applications that fully exploits the advantages of the cloud computing delivery model. Cloud-native is about how applications are created and deployed, not where. “Where” meaning the underlying infrastructure is not as important as it once was, granted, virtualisation already decreased our reliance on static infrastructure, but if we are honest with ourselves infrastructure was still the thing we got excited about the most as VMware admins. Depending on where you see yourself on the “Public Cloud” is the only thing that makes sense going forward scale, you’ll be more or less inclined to accept the idea that cloud computing can be considered a delivery model that is appropriate for both public and private cloud like environments alike. Core to the cloud computing model is the ability to offer seemingly unlimited computing power, on-demand, along with modern data and application services for developers. It is this last piece, I believe, where private cloud solutions fall down the most compared to public cloud vendors, the onslaught of services delivered at more and more rapidly increasing pace by companies like Amazon (AWS), Microsoft (Azure), and Google (GCP) are creating an understandable pull, attracting new workloads. Therefore it seems that when we discuss Cloud Native Applications from the lens of VMware, we again tend to mostly argue the needs of these applications related to the infrastructure piece. If we agree that products and services delivered through software are becoming major competitive differentiators of the business then it is precisely this what is driving the need of Cloud Native Applications. This is where Marc Andreesen’s software is eating the world quote usually comes in…

The way VMware describes it’s portfolio for CNA on their own website is;

> _VMware offers cloud-native solutions across container networking, security, storage, automation, operations and monitoring for enterprises and service providers._

Since VMware is trying hard to reach beyond it’s traditional enterprise base, including forays into multiple open-source type projects, I wanted to try and provide a somewhat comprehensive overview of everything it’s doing in those spaces and it’s potential use cases. Generally speaking VMware’s cloud native portfolio includes VMware vSphere Integrated Containers, for running containerised workloads with existing infrastructure. But please don’t interpret this as a silver bullet to “lift and shift” all your legacy applications to a more modern environment, start by asking why you can’t re-architecture your existing apps, and only then ask yourself if you will truly benefit from moving to this new approach, maybe for easier maintenance or improved scalability, but I’m willing to bet that mostly this wont be the best approach for your old and crusty stuff. For applications that naturally fit a containerised approach the story if different of-course. Secondly VMware Photon Platform is targeted to building new cloud-native infrastructure solutions from the ground up.

<figure class="kg-card kg-image-card kg-card-hascaption"><img src=" __GHOST_URL__ /content/images/2021/08/vmware-components.png" class="kg-image" alt loading="lazy" width="1108" height="611" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmware-components.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vmware-components.png 1000w, __GHOST_URL__ /content/images/2021/08/vmware-components.png 1108w" sizes="(min-width: 720px) 720px"><figcaption>Very simplified view of interconnected components</figcaption></figure>

**vSphere Integrated Containers**

VIC is meant for people who want to stick with the more traditional vSphere infrastructure but also provide Container based services to internal developers. It is available with vSphere Enterprise Plus 6.0 or later, or with vSphere with Operations Management Enterprise Plus. It consists of 3 main components, the vSphere Integrated Container Engine (VIC Engine), Admiral, the Container Management Portal, and Harbor, the Container Registry (based on Docker Distribution). The portal provides a user interface and an API for managing container repositories, images, hosts, and instances. The registry furnishes a user interface and an API so developers can make their own container repositories and images. The engine is a container runtime integrated with vSphere.

vSphere, as opposed to Linux, acts as the container host, containers are deployed as VMs (not in VMs), every container is fully isolated from the host and from the other containers.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vmware-vic-marketecture.png" class="kg-image" alt loading="lazy" width="1108" height="530" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vmware-vic-marketecture.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vmware-vic-marketecture.png 1000w, __GHOST_URL__ /content/images/2021/08/vmware-vic-marketecture.png 1108w" sizes="(min-width: 720px) 720px"></figure>

With VIC the idea is that you can still leverage the existing management toolsets in your vSphere based environment, since these tools typically have an understanding about the virtual abstraction as a VM, the idea is that you run a Container as a VM (C-VM in the picture above). The goal is to run both traditional and containerised applications side-by-side on a common infrastructure, this, if you will, is it’s main use case, take what you know and extent it so you can support container based workloads in a way that is transparent for the developer in theory. It’s meant to avoid having to build a completely separate environment.

**VMware Photon Platform**

VMware Photon Platform is a container-optimised cloud platform, at first it seems to have a somewhat similar goal to VIC, which is to enable developers to build, test, and run container based workloads while still allowing “traditional IT” to maintain security, control and performance of data center infrastructure. But the use cases have a different focus, namely;

Kubernetes as a Service: Developers can deploy, resize, and destroy Kubernetes clusters to develop, test, and automate containerised applications. Platform as a Service: Photon Platform integrates with Pivotal Cloud Foundry to build and scale applications. Continuous integration and delivery: Try to improve the CI/CD pipeline with uniformity and reusability, especially in environments with high container churn. API-managed on-premises cloud: IT can deploy resources and automate their management through a RESTful API. Security: The VMware Lightwave security service can protect applications, Kubernetes clusters, and Photon Platform components.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/photon_platform.png" class="kg-image" alt loading="lazy" width="1108" height="422" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/photon_platform.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/photon_platform.png 1000w, __GHOST_URL__ /content/images/2021/08/photon_platform.png 1108w" sizes="(min-width: 720px) 720px"></figure>

Note that the Photon Controller is no longer a part of the Photon Platform, VMware has made the decision to stop driving this project and instead lean on the open source community to continue this effort. It was originally envisioned to replace vCenter as the control plane for these new type of workloads since vCenter’s monolithic design was never expected to serve these types of applications. More recently of-course the community has moved to other control plane architectures and container orchestration systems.

**NSX-T as the secret sauce.**

With VIC you can select vSphere networks that appear in the Docker client as container networks. NSX-T intends to extent the networking capabilities and make them more enterprise appropriate. By default a container sits behind a NAT’d interface and as such form a operations point of view you struggle with visibility. An other challenge is consuming services outside the container realm, if you have services that need to be consumed from your traditional VMs or even bare metal systems you typically need to pass via bridging nodes potentially choking traffic. NSX-T tries to overcome these challenges by supporting native container networking and micro-segmentation for CaaS (Container-as-a-Service ) and PaaS (Platform-as-a-Service), providing tools with which network and security teams can operationalise containerised apps in a vSphere environment.

**Statefulness and persistent storage**

When discussing containers and container storage you cannot ignore the difference between stateless and stateful workloads and the impact that has on storage persistency. Containers are ephemeral, so if you want to run stateful applications that require persistent storage you need to take some additional steps to ensure your data is maintained through the lifecycle of the containers. VMware provides two persistent volume offerings for situations like these, one for Docker and one for Kubernetes. For Docker VMware provides the vSphere Docker Volume Service (vDVS), this plug-in abstracts the underlying storage of the vSphere environment and makes it available as Docker volumes, it also integrates with Docker Swarm. For stateful containers orchestrated by Kubernetes you can also leverage persistent vSphere Storage (vSAN, VMFS, and NFS) with Kubernetes persistent volume, dynamic provisioning and StatefulSet primitives. Both of these projects are now under the Project Hatchway umbrella.

**Operationalising a Cloud Native environment with VMware**

Again going back to using the tools you know as a VMware admin, VMware has tried to extend vRealize Automation to make it more relevant for these new types of workloads, it leverages the open source Project Admiral, a scalable and lightweight container management platform, to deploy and manage containers through virtual container hosts on VMware vSphere Integrated Containers. The idea is still to provide a separation between the operational folks and the developers. Developers can provision container hosts from the vRealize Automation service catalog as well as model containerised applications using unified service blueprints or Docker Compose.

**Open Source components**

**Photon OS**

Version 2.0 of Photon OS was announced at the beginning of November, Photon OS is a Container-Optimized Linux Operating System. It is an open source minimal Linux container host optimized for cloud-native applications, cloud platforms, and VMware infrastructure. Think of container Operating Systems in general as an OS that provides only the minimal functionality required for deploying applications inside software containers, but typically including built-in mechanisms for shared services consumption and orchestration. Photon OS even comes in 2 flavours, the minimal version is a lightweight host tailored to running containers when performance is paramount, whilst the full version includes additional packages to help deploy containerized applications. Other notable container OSes include a.o. CoreOS, RancherOS, Atomic, and to some extent Intel Clear Linux.

For easy consumption Photon OS is available as a pre-built image for the 3 main public cloud providers as well as an ISO and OVA image. Photon OS includes the open source version of the Docker daemon. After installing Photon OS and enabling Docker, you can immediately run a Docker container.

I’ve written about Photon (and AppCatalyst, and Bonneville) on this blog before if you want some more background.

**vSphere Integrated Containers Engine**

vSphere Integrated Containers Engine (VIC Engine) is the key element of vSphere Integrated Container, it is a container runtime for vSphere, allowing developers familiar with Docker to develop in containers and deploy them alongside traditional VM-based workloads on vSphere clusters, and allowing for these workloads to be managed through the vSphere UI in a way familiar to existing vSphere admins. The VIC Engine provides lifecycle operations, vCenter support, logs, basic client authentication, volume, and basic networking support.

**Harbor**

Harbor is an enterprise-class container registry server based on Docker Distribution, it is embedded in both vSphere Integrated Containers and Photon Platform, it provides advanced security, identity, role based access control, auditing, and management services for Docker images. With Harbor, you can deploy a private registry to maintain your data behind the company firewall. In addition, Harbor supports AD/LDAP integration and the setup of multiple registries with images replicated between registries for high availability.

**Admiral**

Admiral is a key component of vSphere Integrated Containers solution. It is a container management solution with an accent on modelling containerised applications and providing placement based on dynamic policy allocation. It manages Docker hosts, policies, multi-container templates, and applications to simplify and automate resource utilisation and application delivery. Developers can use Docker Compose, Admiral Templates, or the Admiral UI to compose their app and deploy it using the Admiral provisioning and orchestration engine. VMware admins can manage container host infrastructure and apply governance to its usage, including resource grouping, policy based placement, quotas and reservations, and elastic placement zones.

**Photon controller**

Photon controller is a distributed, multi-tenant host controller optimised for containers. The Photon Controller exposes RESTful APIs, SDKs, and CLI tooling to automate infrastructure resources. It is built to support open container orchestration frameworks such as Kubernetes and Pivotal Cloud Foundry as well virtualised environments. Photon Controller functions as the brain of Photon Platform.

NOTE: Photon Controller is no longer actively maintained by VMware.

**Xenon**

Xenon is a decentralised Control Plane framework for writing small REST-based services (Microservices). The photon controller project makes heavy use of Xenon to build a scalable and highly available Infrastructure-as-a-Service fabric, composed of stateful services, responsible for configuration (desired state), work flows (finite state machine tasks), grooming and scheduling logic.

**Project Lightwave**

Project Lightwave offers enterprise-grade, identity and access management services such as single sign-on, authentication, authorization, and certificate authority, as well as certificate key management for container based workloads. It is designed for environments that require multi-tenant, multi-master, scalable LDAP v3 directory services. Lightwave authentication services support Kerberos, OAuth 2.0/OpenID Connect, SAML and WSTrust which enable interoperability with other standards-based technologies in the datacenter.

**Photon OS + Project Lightwave**

Lightwave is an open source security platform from VMware that authenticates and authorises users and groups with AD or LDAP. Through integration between Photon OS and Project Lightwave, organisations can enforce security and governance on container workloads, for example, by ensuring only authorised containers are run on authorised hosts, by authorised users.

**Evolution of the Software Defined Data Center**

One of the core tenets of the SDDC is the speed at which you can adapt your IT infrastructure to the needs of the business, so what if those needs are not foreseen in the original plans for the SDDC, in other words what if they go beyond the capabilities of the software that is powering your SDDC environment. To this end VMware is partnering with Pivotal to deliver something called a Developer-Ready Infrastructure. Integrating Pivotal Cloud Foundry (PCF) with VMware SDDC infrastructure solutions. PCF will provide a PaaS to the developer community so they don’t have to worry (or care) about the underlying infrastructure at all, and at the same time the VMware admin is still empowered by his VMware toolset to maintain and provide services to make this PaaS run.

**VMware PKS**

At VMworld 2017 VMware announced their collaboration with Pivotal to deliver Kubernetes-based container services for multi-cloud enterprises and service providers. It pulls in many of the technologies mentioned before to make a Kubernetes environment more “enterprise grade”, like NSX-T for the networking piece and Project Hatchway for persistent storage. Additionally PKS will include a service broker that provides out-of-the-box access to GCP services. It will enable a VMware admin to expose certain GCP services so that development teams can provision and consume GCP services by creating and managing “service instances” with the kubectl CLI or API. The GCP service broker supports offering GCP subscription services such as Google Cloud Storage, Google BigQuery, and Google Stackdriver. These services will be able to be consumed by applications running on-premises or from within GCP.

In summary, VMware is trying to provide a bridge between multiple different parties, a bridge between developers and VMware admins, a bridge between traditional VMware infrastructure and Public Cloud based services (incidentally, this was also the premise of VMware’s former vCloud Air platform, now sold to French cloud hosting provider OVH.) The question you should pose yourself is, I think, whether it makes sense to incorporate these new architectures, to what extend are your applications unique snowflakes that do not fit this new world of micro-services, have you tended to pets all your life or can some of them be considered cattle. Furthermore it remains to be seen if the pendulum will swing back at some point. Legacy will be here for a long time yet.

_\*That was tongue in cheek, just in case that wasn’t clear_

