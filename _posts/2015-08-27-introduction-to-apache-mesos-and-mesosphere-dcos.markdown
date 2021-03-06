---
layout: post
title:  "Introduction to Apache Mesos and Mesosphere DCOS"
date:   2014-05-12 14:21:43 +0200
categories: Mesos, Mesosphere, DCOS 
permalink: /2015/08/27/introduction-to-apache-mesos-and-mesosphere-dcos/
redirect_from:
  - /2015/08/27/introduction-to-apache-mesos-and-mesosphere-dcos/amp/
---

![blog image]({{ "/assets/apache-mesos-logo.jpg" | absolute_url }})

**A little history…**

Mesos started as a research project at UC Berkeley in early 2009 by Benjamin Hindman, Andy Konwinski, Matei Zaharia, Ali Ghodsi, Anthony D. Joseph, Randy Katz, Scott Shenker, and Ion Stoica. Benjamin then brought Mesos to Twitter where it now runs their datacenters, and later became Chief Architect at Mesosphere which builds the Mesosphere Datacenter Operating System (DCOS).

The motivation for building Mesos was to try and increase the performance and utilization of clusters, the team believed that static partitioning of resources in the datacenter should be considered harmful, for example;

Let’s say your datacenter has 9 hosts;

![blog image]({{ "/assets/mesos1.png" | absolute_url }})

And you statically partition your hosts and assign 3 each to 3 applications (in this example Hadoop, Spark, Ruby on Rails);

![blog image]({{ "/assets/mesos2.png" | absolute_url }})

Then the utilization of those hosts would be sub-optimal;

![blog image]({{ "/assets/mesos3.png" | absolute_url }})

So if you would use all resources, in this case the 9 hosts, as a shared pool that you can schedule if needed, utilization would improve;

![blog image]({{ "/assets/mesos4.png" | absolute_url }})

The second premise the team held was that they needed a new framework for distributed systems, i.e. not every use case lends itself well to something like Map/Reduce (which in itself led to to birth of Spark, but that’s another story for another day) and a new more straightforward and universally applicable framework was required.

What does the framework (distributed system) of Mesos look like?
![blog image]({{ "/assets/mesos5.png" | absolute_url }})

Typically in a distributed system you have a coordinator (scheduler) and workers (task execution).
The coordinator runs processes/tasks simultaneously (distributed), it handles process failures (fault-tolerance), and it optimizes performance (elasticity). In other words it is coordinating execution of what you are trying to run (does not need to be an entire program, this could also be a computation of some sorts) in the datacenter. As alluded to before, Mesos calls this combination scheduling.
![blog image]({{ "/assets/mesos6.png" | absolute_url }})

In other words a Mesos frameworks is a distributed system that has a scheduler.

Now what Mesos really is, is a level of abstraction between the scheduler and the machines where you are trying to execute it’s tasks.

![blog image]({{ "/assets/mesos7.png" | absolute_url }})

So in Mesos the scheduler communicates with the Mesos layer (via API’s) instead of directly with the machines. The idea here is to fix the issues of static partitioning whereby you no longer have a scheduler for each specific workload talking to it’s designated workers, but instead you have the schedulers talk to Mesos which in turn talks to the entire pool of resources.

![blog image]({{ "/assets/mesos8.png" | absolute_url }})

The immediate benefit is that you can run multiple distributed systems on the same cluster of machines and dynamically share those resources more efficiently (no more static partitioning).

Secondly because of this abstraction it provides common functionality (failure detection, task distribution, task starting, task monitoring, task killing, task cleanup) that each distributed system typically tries to implement in it’s own unique way.

**Where does Mesos fit as an abstraction layer in the datacenter?**

![blog image]({{ "/assets/mesos9.png" | absolute_url }})The Mesos layer wants to make it easier to build and run these frameworks by using and scheduling resources.
IaaS’ abstraction is machines, e.g. you give it a number and it provides x number of machines and thus considered a lower level of abstraction in Mesos’ concept.
PaaS is concerned with deploying and managing applications/services and does not care about the underlying infrastructure and thus considered a higher level of abstraction in Mesos’ concept. In terms of interactions, PaaS is probably interacted with by developers, where Mesos is interacted with by software through APIs.

In other words you could build a PaaS system on top of Mesos (like Marathon for example – which is more PaaS like than an actual PaaS but anyway) and you could run Mesos on top of an IaaS (like OpenStack for example). The idea of breaking down hard partitioning comes back again if you run your Mesos layer on top a combination of systems (like OpenStack + Hardware + VM’s for example) and are able to schedule your workloads across all of them, in that sense you can think of Mesos as a sort of datacenter kernel, i.e. it abstracts aways machines and allows you to build distributed systems on top of any of those underlying components.

So Apache Mesos is a distributed system for running and building other distributed systems. (Like Spark for example).

**Architectural details of Mesos**

In Mesos, the framework (distributed system) issues a request for what is needed at that specific time to the scheduler. This differs from traditional distributed systems as normally the person (again, in Mesos it will be the framework making the request, not a person) making the request would need to figure out the specification beforehand and request those resources, typically the requirements change (think Map/Reduce for example where there is a change in required resources between the Map and Reduce phases).

![blog image]({{ "/assets/mesos10.png" | absolute_url }})Mesos then offers the best approximation of those resources immediately instead of waiting to able to fulfil the request completely/exactly. (it wants to be non-blocking as this in most cases is sufficient, i.e. you don’t need the exact amount of requested resources immediately)

![blog image]({{ "/assets/mesos11.png" | absolute_url }})Now the framework (distributed system) uses the offers from Mesos to perform it’s own scheduling, e.g. “two-level scheduling”

This is then turned into a task and submitted to run somewhere in the datacenter.

The reason to have this system of “two-level scheduling” is to be able to support multiple distributed systems at the same time. Mesos provides the resource allocations to, in this case, Spark. Spark makes the decision about what tasks to run given the available resources. (i.e. I want to run these maps now because I can satisfy those requirements).
![blog image]({{ "/assets/mesos12.png" | absolute_url }})

So once the task are submitted from the framework to Mesos, these now need to be executed. The Mesos master gets the task to one of the slaves where an agent is running to manage launching those tasks. (i.e. if it’s a command it runs it, if it needs specific resources to perform the task, like .jar files, it pulls those down, runs it in a sandbox, and then launches the task).
Or alternatively, the framework can decide it wants to run the tasks with an executor (layer of indirection needed by the framework, which can also be used to run threads).

![blog image]({{ "/assets/mesos13.png" | absolute_url }})To provide resource isolation Mesos has built-in support for cgroups and namespaces, or you can also give it a docker container to run as a task. So this gives you multi-tenancy (frameworks) across your pooled resources (across machines and inside individual machines).

You can use reservations if you want, but then you are getting back into the hard partitioning space of course. If you have stateful applications you need reservations (the task always needs to run on the same machine(s)) and persistent volumes (data needs to survive restarts), which is also possible with Mesos.

**Mesosphere DCOS**

DCOS (Data Center Operating System) is taking the Mesos “kernel” and building around/upon that with additional services and functionality. Add-on modules like mesos-dns, tooling like a CLI, a GUI, a repository for the packages that you want to run, and frameworks like Marathon (a.k.a. distributed init), Chronos (a.k.a. distributed cron) , etc.

Like the name implies it is meant to be an operating system that spans all of the machines in your datacenter or cloud. DCOS should run in any modern Linux environment, public and private cloud, virtual machines and bare metal. (supported platforms are: Amazon AWS, Google Compute Engine, Microsoft Azure, OpenStack, VMware, RedHat, CentOS, CoreOS, and Ubuntu). Currently around 40 services have been made available for DCOS and can be found in the public repository (Hadoop, Spark, Cassandra, Jenkins, Kafka, MemSQL,…)

[![Mesosphere](http://img.youtube.com/vi/0I6qG9RQUnY/0.jpg)](https://youtu.be/0I6qG9RQUnY)  