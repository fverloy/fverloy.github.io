---
layout: post
title: VMware Cloud on AWS, Azure, and GCP
date: '2019-08-22 09:48:00'
tags:
- vmware
- cloud
- multicloud
- azure
- gcp
- aws
---

With the recent announcement of the Google Cloud VMware Solution you now have the possibility to run a VMware stack on each of the 3 major hyperscalers, AWS, Azure, and GCP. In this post I wanted to compare and contrast the 3 solutions, but before we do that let’s discuss why you would want to use either 3 of them if you are an enterprise customer today.

The benefits to running VMware in your private datacenter are well established and many IT admins have built their career on learning and implementing the VMware SDDC. They understand how to architect and operationalize the VMware stack to maximize efficiency in a private cloud setting. When looking at the public cloud there are a couple of things that could be very compelling for enterprise customers, the global infrastructure spanning many locations and availability zones provides global reach and resiliency, and the prospect of being able to more easily incorporate specific public cloud services provides to existing brown field deployments provides interesting scenarios, especially since you can consume it in a pay-as-you-go model. A typical enterprise customer has been building out it’s IT infrastructure for many years and in doing business has probably accumulated a large amount of customer specific data, by connecting this trove of data to public cloud specific services around business intelligence, machine learning, AI, etc. a new low friction avenue might open up to gain better insight and leapfrog the competition. So there is differentiation in both how the underlying VMware stack is offered and what is available through it as in what public cloud vendor you go with and the specific services they offer.

All 3 solutions are powered by VMware Cloud Foundation which is an integrated stack, using SDDC Manager for lifecycle management of all individual component of the stack, to bundle compute (vSphere), storage (VMware vSAN) and network virtualization (NSX) into a single platform that can be deployed on-premises as a private cloud or run as a service within a public cloud. It is available in 5 different editions providing an increasing set of enterprise capabilities.

**VMware Cloud on AWS**

This solution has been cooking the longest out of all 3 and certainly seems to be more rounded out at the moment, VMware announced a strategic partnership with AWS in 2016 and this resulted a.o. things in the initial availability of VMware Cloud on AWS in august of 2017. The service has been jointly developed by VMware and AWS with the goal of helping customers deploy and accelerate the migration of VMware vSphere-based workloads to the cloud while providing familiar operational tooling. It is also important to note that there are no ingress or egress charges to consume the native AWS services from the VMC on AWS environment.

<img src="/assets/img/VMC-on-AWS.jpg">

Since there is a lot of information on VMC on AWS publicly available already I humbly suggest grokking those sources instead of me proving yet another high-level overview of the solution. A good starting point imho is Brian Graf’s (Sr. Technical Marketing Manager - VMware Cloud on AWS) [blog](http://www.brianjgraf.com/category/vmc/)

**Azure VMware Solution by CloudSimple**

The Azure VMware Solution (AVS) is offered directly by Microsoft and is and powered by CloudSimple as a fully managed service, the service provides a VMware environment on Azure bare metal servers. The service is enabled by default on Enterprise Accounts and Cloud Service Provider subscriptions, if you have another Azure subscription you need to contact Microsoft Azure support, you can use your Azure enterprise agreement and Azure credits towards the CloudSimple service. CloudSimple is currently only available in East and West US regions. It is connected to your on-premises VMware environment either via ExpressRoute which provides bandwidth up to 100 Gbps or via a edge VPN gateway.

<img src="/assets/img/azure-vmware-solution-by-cloudsimple.png">

You can select the region for the CloudSimple service from the Azure portal.

<img src="/assets/img/Screenshot-2019-08-21-at-20.16.26.png">

To run the actual VMware software stack CloudSimple needs a minimum of 3 CloudSimple nodes to form a cluster, and you can have a max of 16 nodes in 1 cluster with a max of 4 clusters in a private cloud setup, they currently offer 2 different node types the CS36 and the CS28.

<img src="/assets/img/Screenshot-2019-08-21-at-20.39.32.png">

The nodes are added via the Azure portal.

<img src="/assets/img/Screenshot-2019-08-21-at-20.45.13.png">

The nodes together provide the bare metal hardware for the deployment of the private cloud (VCF consisting of vCenter, ESXi, NSX-T and vSAN), this is operated and maintained (i.e. VCF lifecycle management) for you as a service.

<img src="/assets/img/Screenshot-2019-08-21-at-20.50.39.png">

The initial deployment of the private cloud can take up to 2 hours. Now you still need access to your VCF instance running on Azure, to that end you can either use ExpressRoute or setup a point-to-site (i.e. per user) or site-to-site VPN connection which is created and managed via the CloudSimple portal inside your Azure portal.

<img src="/assets/img/Screenshot-2019-08-21-at-20.56.06.png">

Now that you have connectivity between your on-premises environment and the CloudSimple private cloud on Azure you can manage your workloads using vCenter. You can launch the vSphere client directly from the CloudSimple portal within your Azure portal.

<img src="/assets/img/Screenshot-2019-08-21-at-21.01.36.png">

You can still see the same virtual machines under management of vCenter directly in the Azure portal as well.

<img src="/assets/img/Screenshot-2019-08-21-at-21.04.04.png">

NOTE: there is also a upcoming (later this year according to Microsoft) Azure VMware Solution by Virtustream

**Google Cloud VMware Solution by CloudSimple**

The solution was announced on July 30th of 2019 but is not available yet, it is currently slated for availability on the Google Cloud Marketplace later this year. I am assuming the solution will be largely similar to the one CloudSimple is providing for Azure. Google Cloud said they will provide the first line of support, working closely with CloudSimple to help ensure customers receive a streamlined product support experience.

This is not the first collaboration between Google Cloud and VMware and as the pundits will point out it is something that was largely expected after Thomas Kurian became CEO of Google Cloud lending some additional enterprise credibility to the hyperscaler that is conceivably lagging most in this area compared to AWS and Azure. Previously they announced Google Cloud integrations for VMware NSX Service Mesh and SD-WAN by VeloCloud to allow customers to easily deploy and gain visibility into their hybrid workloads.

Google Cloud’s Anthos on VMware vSphere, including validations for vSAN, as the preferred hyperconverged infrastructure, to provide customers a multi-cloud solution and providing Kubernetes users the ability to create and manage persistent storage volumes for stateful workloads on-premises.

And a Google Cloud plug-in for VMware vRealize Automation providing customers with a way to deploy, orchestrate and manage Google Cloud resources from within their vRealize Automation environment.

**Compare and contrast**

The value prop of the 3 solutions is similar, provide a familiar way to manage and operationalize a true hybrid cloud environment. The way the solution is built, even though all 3 use VCF, differs quite a bit from a plumbing perspective. VMware Cloud on AWS is a joint engineering effort from AWS and VMware and looks the most mature, the Azure and GCP solution is underpinned by CloudSimple which is a third-party developer not directly associated with either VMware or any of the hyper-scalers. (although M12, the investment arm of Microsoft has invested in the company and CloudSimple’s founder and CEO is Guru Pangal, the co-founder of StorSimple, a cloud storage gateway company acquired by Microsoft in 2012.) The startup came out of stealth earlier this year with a mandate to enable VMware customers easily move workloads to public clouds which they certainly are achieving with a default stamp of approval from VMware (judging from the relative amount of fanfare). If you simply use availability as a grading measure today you can see that the VMC on AWS solution is globally available whilst the Azure solution is only available in 2 regions in the US and GCP still needs to get delivered. Another comparison would be the supported cluster size, right now the CloudSimple solution allows 64 hosts while the AWS solution can scale much larger. The maturity and options also seem to favour the AWS solution at the moment, with options like building a stretch cluster over multiple AWS availability zones etc.

In terms of pricing, VMware makes a public calculator available [here](https://cloud.vmware.com/vmc-aws/pricing), Microsoft does the same for the CloudSimple solution [here](https://azure.microsoft.com/en-in/pricing/details/azure-vmware-cloudsimple/). As an example for a 3 host on-demand solution for 1 month in US East VMC on AWS would round out at about $18,000 (with no extras like stretch cluster or SRM) for 3 x i3 hosts and the CloudSimple solution on Azure would come to around $14,000 for 3 x CS28 hosts. Note that the VMC on AWS i3 host type has a higher core (and double the memory) count at 36 (there is no 28 core equivilant) so pricing wise they are close.

If you talk to VMware and ask why customers choose to go the VMware Cloud on ‘Public Cloud’ route they point to 3 main scenarios, maintain and expand, consolidate and migrate, and workload flexibility. For maintain and expand the reason is that a customer could be running out of space on-premises and wants to quickly ramp up using the public cloud as an expansion to his existing on-premises SDDC. The second one is a consolidation of existing sites, maybe after M&A activity, and the unwillingness to sign on another colo instead opting to put all those satellite environments in the public cloud. The 3rd one is more around on-demand flexibility for things like test and dev workloads, or shorter lived workloads for some reason, taking advantage of the pay-as-you-go pricing model.

