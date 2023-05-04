---
layout: post
title: VMware OpenStack Virtual Appliance
date: '2014-07-31 22:00:00'
tags:
- openstack
- vmware
---

VMware provides an OpenStack Virtual Appliance, VOVA for short, to help VMware admins get some hands-on experience with using OpenStack in a VMware environment. It is however purely a proof of concept appliance and is not supported in any way by VMware. To find out more about the OpenStack effort at VMware in general please visit https://communities.vmware.com/community/vmtn/openstack

You can download the VOVA appliance OVF package (OpenStack Icehouse release) [here](https://partnerweb.vmware.com/programs/vmdkimage/vova/VOVA_ICEHOUSE.ova).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/ova1.png" class="kg-image" alt loading="lazy" width="1024" height="293" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/ova1.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/ova1.png 1000w, __GHOST_URL__ /content/images/2021/08/ova1.png 1024w" sizes="(min-width: 720px) 720px"></figure>

VOVA only works with vCenter 5.1 and above and only supports a single Datacenter, you should also not run production workloads on this cluster as a precaution. If you are running multiple hosts in your cluster you should enable DRS in “fully automated mode”, the cluster should also have only one shared datastore for all the hosts in the cluster. It is recommended to deploy the VOVA appliance on a host that is not part of the cluster managed by the appliance. (so you can manage multiple clusters in your vCenter instance).

When the OVF package is deployed it will show you the OpenStack Dashboard URL on first boot.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vova2.png" class="kg-image" alt loading="lazy" width="1024" height="570" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vova2.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vova2.png 1000w, __GHOST_URL__ /content/images/2021/08/vova2.png 1024w" sizes="(min-width: 720px) 720px"></figure>

You can login with the credentials demo/vmware

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vova3.png" class="kg-image" alt loading="lazy" width="202" height="299"></figure>

The appliance comes pre-loaded with a Debian disk image which allows you to easily launch new instances.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vova4.png" class="kg-image" alt loading="lazy" width="300" height="276"></figure>

Spawning the first VM can take a while because the 1 GB Debian disk image must be copied from the file system of the VOVA appliance to your cluster’s Datastore. All subsequent instances should be significantly faster (under 30 seconds).

VOVA also allows you to test the OpenStack CLI tools which directly acces the OpenStack APIs, you need to SSH (root/vmware) into the VOVA’s console and run the CLI commands from there.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vova5.png" class="kg-image" alt loading="lazy" width="1024" height="223" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/vova5.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/08/vova5.png 1000w, __GHOST_URL__ /content/images/2021/08/vova5.png 1024w" sizes="(min-width: 720px) 720px"></figure>

The vCenter Web Client plug-in for OpenStack is also included allowing you to see OpenStack instances from the Web Client

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/vova6.png" class="kg-image" alt loading="lazy" width="300" height="198"></figure>

Currently the VOVA appliance has some limitations;

- No Neutron support: Neutron with vSphere requires the VMware NSX solution. We plan to release a future version of VOVA that can optionally leverage NSX.
- No Security Groups support. With vSphere, VMware NSX is required for security groups network filtering. We plan to release a future version of VOVA that can optionally leverage NSX.
- No exposed options to configure floating IPs. This is possible with the current appliance, but it has not been exposed via the OVF options.
- No support for sparse disks. If you try to upload your own disk images, the images must be flat, not sparse.
- No support for Swift (object storage). VOVA has no plans to leverage OpenStack swift for object storage. You are free to deploy swift on your own in another VM.

Also keep in mind that VOVA is not a product and will likely be discontinued once production-quality solutions with similar ease-of-use are made available (remember that there is nothing “special” about VOVA, it is just the open source OpenStack code running on Ubuntu, with proper configuration for using vSphere). However in the months following, expect to see updated versions of VOVA with the option of using Neutron + Security groups via VMware NSX.

