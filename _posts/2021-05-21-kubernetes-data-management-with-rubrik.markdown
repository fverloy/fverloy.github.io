---
layout: post
title: Kubernetes Data Management with Rubrik
date: '2021-05-21 08:42:00'
tags:
- rubrik
- kubernetes
- backup
---

Rubrik just held its yearly user conference [Forward](https://forward.rubrik.com/) during which we announced our [Kubernetes](https://kubernetes.io/) data management solution. In true Rubrik fashion we wanted to make this very easy to setup and use.

Backup for Kubernetes has been a hotly debated topic over the years, pundits like to point out that container workloads are supposed to be stateless and as such backing them up isn’t strictly needed. Reality of course is that the increased adoption of containerized workloads means we have all kinds of applications being managed by kubernetes these days, and in any case state has to live somewhere, the idea then is to provide data protection for those type of applications so you have the ability to safeguard your companies’ crown jewels, it’s data wherever it happens to sit.

Rubrik’s k8s data management solution is driven by Polaris, our SaaS platform, as such we can rapidly iterate and make sure your deployment is always up to date with the latest capabilities.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/k8s3.png" class="kg-image" alt loading="lazy" width="1344" height="730" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/k8s3.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/k8s3.png 1000w, __GHOST_URL__ /content/images/2021/08/k8s3.png 1344w" sizes="(min-width: 720px) 720px"></figure>

To get started simply start the onboarding wizard on your Polaris instance, the backup data will be stored on your existing Rubrik CDM cluster on-premises, during the setup we will deploy our Rubrik Service on the Kubernetes cluster as part of the cluster registration.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/k8s1.png" class="kg-image" alt loading="lazy" width="2000" height="883" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/k8s1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/k8s1.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/k8s1.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/k8s1.png 2400w" sizes="(min-width: 720px) 720px"></figure>

We are using the same SLA policy based protection for Kubernetes as for all our other supported applications, you simply declare the desired end state in an SLA and you can attach it to either the cluster level to auto-protect all the namespaces or apply the SLA on a more granular level for more specific levels of protection. Alternatively you can also use labels for automatic assignment of desired SLAs to namespaces containing those labels. On-demand snapshots are also an option if you prefer a one off backup copy outside the regular schedule.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/k8s2.png" class="kg-image" alt loading="lazy" width="2000" height="1085" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/k8s2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/k8s2.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/08/k8s2.png 1600w, __GHOST_URL__ /content/images/size/w2400/2021/08/k8s2.png 2400w" sizes="(min-width: 720px) 720px"></figure>

Once the SLA is applied we automatically start protecting the applications deployed within a Kubernetes Namespace and the associated data stored on the Persistent Volumes linked to those namespaces. The Namespace contains the definitions of the resources that make up the application, including references to the persistent volumes where application data is stored. We support both [CSI](https://kubernetes-csi.github.io/docs/) and non-CSI enabled storage platforms.

The goal of backing up data is the ability to restore said data, again the focus here is on simplicity. Simply select the restore point in the calendar view and proceed with the recovery option of your choosing.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/k8s4.png" class="kg-image" alt loading="lazy" width="1350" height="1302" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/k8s4.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/k8s4.png 1000w, __GHOST_URL__ /content/images/2021/08/k8s4.png 1350w" sizes="(min-width: 720px) 720px"></figure>

As you can see we can bring back the entire namespace, or even restore (download) individual files if you wanted to, the applications or data can be restored back to a new namespace, or even to an alternate Kubernetes cluster. You can also bring back the persistent volume without binding it to a persistent volume claim giving you the flexibility to attach this PV to an alternate namespace.

There you have it, kubernetes support from Rubrik. Happy containering!

