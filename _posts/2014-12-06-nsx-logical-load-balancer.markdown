---
layout: post
title:  "NSX Logical Load Balancer"
date:   2014-12-06 14:21:43 +0200
categories: VMware, SDN, NSX
permalink: /2014/12/06/nsx-logical-load-balancer/
---
VMware NSX is a network virtualisation and security platform that enables inclusion of 3rd party integration for specific functionality, like advanced firewalling, load-balancing, anti-virus, etc. Having said that VMware NSX also provides a lot of services out of the box that depending on the customer use-case can cover a lot of requirements.

One of these functionalities is load balancing using the NSX Edge Virtual Machine that is part of the standard NSX environment. The NSX Edge load balancer enables network traffic to follow multiple paths to a specific destination. It distributes incoming service requests evenly (or depending on your specific load balancing algorithm) among multiple servers in such a way that the load distribution is transparent to users. Load balancing thus helps in achieving optimal resource utilization, maximizing throughput, minimizing response time, and avoiding overload. NSX Edge provides load balancing up to Layer 7.

![blog image]({{ "/assets/lb1.png" | absolute_url }})

The technology behind our NSX load balancer is based on HAProxy and as such you can leverage a lot of the HAProxy documentation to build advanced rulesets (Application Rules) in NSX.

Like the picture above indicates you can deploy the logical load balancer either “one-armed” (Tenant B example) or transparently inline (Tenant A example).

To start using the load balancing functionality you first need to deploy an NSX edge in your vSphere environment.

Go to Networking & Security i your vSphere Web Client and select NSX Edges, then click on the green plus sign to start adding a new NSX edge. Select Edge Services Gateway as an install type and give your load balancer an indicative name.

![blog image]({{ "/assets/lb2.png" | absolute_url }})

Next you can enter a read-only CLI username and select a password of your choice. You can also choose to enable SSH access to the NSX Edge VM.

![blog image]({{ "/assets/lb3.png" | absolute_url }})

Now you need to select the size of the Edge VM, because this is a lab environment I’ll opt for compact but depending on your specific requirements you should select the most appropriate size. Generally speaking more vCPU drives more bandwidth and more RAM drives more connections per second. The available options are;

Compact: 1 vCPU, 512 MB
Large: 2 vCPU, 1024 MB
Quad Large: 4 vCPU, 1024 MB
X-Large: 6 vCPU, 8192 MB

Click on the green plus sign to add an Edge VM, you need to select a cluster/resource pool to place the VM and select the datastore. Typically we would place NSX edge VMs in a dedicated Edge rack cluster. For more information on NSX vSphere cluster design recommendations please refer to the NSX design guide available here: https://communities.vmware.com/servlet/JiveServlet/previewBody/27683-102-3-37383/NSXvSphereDesignGuidev2.1.pdf

![blog image]({{ "/assets/lb4.png" | absolute_url }})

Since we are deploying a one-armed load balancer we only need to define one interface next. Click the green plus sign to configure the interface. Give it a name, select the logical switch it needs to connect to (i.e. where are the VMs that I want to load balance located) configure an IP address in this specific subnet (VXLAN virtual switch) and you are good to go.

![blog image]({{ "/assets/lb5.png" | absolute_url }})

Next up configure a default gateway.

![blog image]({{ "/assets/lb6.png" | absolute_url }})

Configure the firewall default policy and select Accept to allow traffic.

![blog image]({{ "/assets/lb7.png" | absolute_url }})

Verify all setting and click Finish to start deploying the Edge VM.

![blog image]({{ "/assets/lb8.png" | absolute_url }})

After a few minuted the new Edge VM will be deployed and you can start to configure load balancing for the VMs located on the Web-Tier-01 VXLAN virtual switch (as we selected before) by double clicking the new Edge VM.

Click on Manage and select Load Balancer, next click on Edit and select the Enable Load Balancer check box.

![blog image]({{ "/assets/lb9.png" | absolute_url }})

Next thing we need is an application profile, you create an application profile to define the behavior of a particular type of network traffic. After configuring a profile, you associate the profile with a virtual server. The virtual server then processes traffic according to the values specified in the profile.

Click on Manage and select Load Balancer, then click on Application Profiles, give the profile a name. In our case we are going to load balance 2 HTTPS webservers but are not going to proxy the SSL traffic so we’ll select Enable SSL Passthrough.

![blog image]({{ "/assets/lb10.png" | absolute_url }})

Now we need to create a pool containing the webservers that will serve the actual content.

Select Pools, give it a name, select the load balancing algorithm.

Today we can choose between;

Round-robin
IP Hash
Least connection
URI
HTTP Header
URL

Then select a monitor to check if the severs are serving https (in this case) requests.
You can add the members of the pools from the same interface.

![blog image]({{ "/assets/lb11.png" | absolute_url }})

Now we create the virtual server that will take the front-end connections from the webclients and forward them to the actual webservers. Note that the Protocol of the virtual server can be different from the protocols of the back-end servers. i.e. You could do HTTPS from the client webbrowser to the virtual server and HTTP from the virtual server to the back-end for example.

![blog image]({{ "/assets/lb12.png" | absolute_url }})

Now you should be able to reach the IP (VIP) of the virtual server we just created and start to load balance your web-application.

![blog image]({{ "/assets/lb13.png" | absolute_url }})

Since we selected round-robin as our load-balancing algorithm each time you hit refresh (use control and click on refresh to avoid using the browsers cache) on your browser you should see the other webserver.