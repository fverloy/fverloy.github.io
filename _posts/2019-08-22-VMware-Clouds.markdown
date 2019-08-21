---
layout: post
title:  "VMware Cloud on AWS, Azure, and GCP"
date:   2019-08-22 07:00:00 +0200
categories: Blog, VMware, Azure, GCP, Cloudsimple
permalink: /2019/08/22/VMware-Clouds/
---
![blog image]({{ "/assets/boy-standing-on-mountain-looking-at-clouds.jpg" | absolute_url }})

With the recent announcement of the Google Cloud VMware Solution you now have the possibility to run a VMware stack on each of the 3 major hyperscalers, AWS, Azure, and GCP. In this post I wanted to compare and contrast the 3 solutions, but before we do that let's discuss why you would want to use either 3 of them if you are an enterprise customer today.

The benefits to running VMware in your private datacenter are well established and many IT admins have built their career on learning and implementing the VMware SDDC. They understand how to architect and operationalize the VMware stack to maximize effiency in a private cloud setting. When looking at the public cloud there are a couple of things that could be very compelling for enterprise customers, the global infrastructure spanning many locations and availability zones provides global reach and resiliency, and the prospect of being able to more easily incorporate specific public cloud services provides to existing brown field deployments provides interesting scenarios, especially since you can consume it in a pay-as-you-go model. 

All 3 solutions are powered by VMware Cloud Foundation which is an integrated stack, using SDDC Manager for lifecycle management of all individual component of the stack, to bundle compute (vSphere), storage (VMware vSAN) and network virtualization (NSX-T) into a single platform that can be deployed on-premises as a private cloud or run as a service within a public cloud. It is available in 5 different editions providing an increasing set of enterprise capabilities.

**VMware Cloud on AWS**

This solution has been cooking the longest out of all 3 and certainly seems to be more rounded out at the moment, VMware announced a stratigic partnership with AWS in 2016 and this resulted a.o. things in the initial availability of VMware Cloud on AWS in august of 2017. The service has been jointly developed by VMware and AWS with the goal of helping customers deploy and accelerate the migration of VMware vSphere-based workloads to the cloud while providing familiar operational tooling. It is also important to note that there are no ingress or egress charges to consume the native AWS services from the VMC on AWS environment.

![blog image]({{ "/assets/VMC-on-AWS.jpg" | absolute_url }})

**Azure VMware Solution by CloudSimple**

The Azure VMware Solution (AVS) is offered directly by Microsoft and is and powered by CloudSimple as a fully managed service, the service provides a VMware environment on Azure bare metal servers. The service is enabled by default on Enterprise Accounts and Cloud Service Provider subscriptions, if you have another Azure subscription you need to contact Microsoft Azure support, you can use your Azure enterprise agreement and Azure credits towards the CloudSimple service. CloudSimple is currently only available in East and West US regions. It is connected to your on-premises VMware environment via ExpressRoute which provides bandwidth up to 100 Gbps.

![blog image]({{ "/assets/azure-vmware-solution-by-cloudsimple.png" | absolute_url }})

You can select the region for the CloudSimple service from the Azure portal.

![blog image]({{ "/assets/Screenshot 2019-08-21 at 20.16.26.png" | absolute_url }})

To run the actual VMware software stack CloudSimple needs a minimum of 3 CloudSimple nodes tpo form a cluster, and you can have a max of 16 nodes in 1 cluster with a max of 4 clusters in a private cloud setup, they currently offer 2 different node types the CS36 and the CS28.

![blog image]({{ "/assets/Screenshot 2019-08-21 at 20.39.32.png" | absolute_url }})

The nodes are added via the Azure portal

![blog image]({{ "/assets/Screenshot 2019-08-21 at 20.45.13.png" | absolute_url }})

The nodes together provide the bare metal hardware for the deployment of the private cloud (VCF consisting of vCenter, ESXi, NSX-T and vSAN), this is operated and maintained (i.e. VCF lifecycle management) for you as a service.

![blog image]({{ "/assets/Screenshot 2019-08-21 at 20.50.39.png" | absolute_url }})

The initial deployment of the private cloud can take up to 2 hours.

NOTE: there is also a upcoming (coming later this year according to Microsoft) Azure VMware Solution by Virtustream

**Google Cloud VMware Solution by CloudSimple**

**Compare and contrast**

