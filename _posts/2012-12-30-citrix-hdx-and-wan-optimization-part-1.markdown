---
layout: post
title: Citrix HDX and WAN optimization (part 1)
date: '2012-12-30 23:00:00'
tags:
- citrix
- networking
permalink: /citrix-hdx-and-wan-optimization-part-1/
---

 **Introduction**

In this first of a two part blog post about Citrix HDX I want to explore the impact of HDX on the Wide Area Network, part one will serve as the introduction, and in part two I will testrun some of the scenarios described in part one.

HDX came to be because Citrix was finally getting competitive pressure on its Independent Computing Architecture (ICA) protocol from Microsoft with RDP version 7 and beyond and Teradici/VMware with PCoIP. (And arguably other protocols like Quest EOP Xstream, HP RGS, RedHat SPICE, etc.)

Citrix’s reaction to these competitive pressures has been to elevate the conversation above the protocol, stating that a great user experience is more than just a protocol, thus Citrix created the HDX brand to discuss all the elements in addition to ICA that Citrix claims allow it to deliver the best user experience.

**HDX Brands**

HDX is not a feature or a technology — it is a brand.

Short for “High Definition user eXperience,” HDX is the umbrella term that encapsulates several different Citrix technologies. Citrix has created HDX sub-brands, these include the list below and each brand represents a variety of technologies:

- HDX Broadcast (ICA)
- Capabilities for providing virtual desktops and applications over any network. This is the underlying transport for many of the other HDX technologies; it includes instant mouse click feedback, keystroke latency reduction, multi-level compression, session reliability, queuing and tossing.
- HDX MediaStream
- Capabilities for multimedia such as sound and video, using HDX Broadcast as it’s base, including client side rendering (streaming the content to the local client device for playing via local codecs with seamless embedding into the remote session).
- Flash redirection (Flash v2), Windows Media redirection.
- HDX Realtime
- Capabilities for real time communications such as voice and web cameras, using HDX Broadcast as it’s base, it includes EasyCall (VoIP integration), and bi-directional audio functionality.
- HDX SmartAccess
- Refers mainly to the Citrix Access Gateway (SSL VPN) and cloud gateway components for single sign-on.
- HDX RichGraphics (incl 3D, 3D PRO, and GDI+ remoting)
- Capabilities in remoting high end graphics using HDX Broadcast as it’s base, uses image acceleration and progressive display for graphically intense images. (formerly known as project appollo)
- HDX Plug-n-Play
- Capabilities to provide connectivity for local devices and applications in a virtualized environment, including USB, multi-monitor support, smart card support, special folder redirection, universal printing, and file-type associations.
- HDX WAN Optimization
- Capabilities to locally cache bandwidth intensive data and graphics, locally stage streamed applications (formally known as Intellicache, relying mostly on their Branch Repeater product line).
- HDX Adaptive Orchestration
- Capabilities that enable seamless interaction between the HDX technology categories. The central concept is that all these components work adaptively to tune the unified HDX offering for the best possible user experience.
<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hdx1.png" class="kg-image" alt loading="lazy" width="432" height="178"></figure>

The goal of this post is to provide an overview of these HDX sub-brands and technologies that directly relate to the network, and WAN optimization, in order to have a clearer understanding of marketing vs. technology impact.

Not every HDX feature is available on both XenApp and XenDesktop, (and now also VDI in-a-box after the acquisition of Kaviza) the table below shows the feature matrix for both:

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hdx2.png" class="kg-image" alt loading="lazy" width="974" height="523" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/hdx2.png 600w, __GHOST_URL__ /content/images/2021/08/hdx2.png 974w" sizes="(min-width: 720px) 720px"></figure>

**HDX and the network**

As stated before most of the HDX technologies are either existing ICA components or rely on ICA (HDX Broadcast) as a remoting protocol. As such we should be able to (WAN) optimize most of the content within HDX one way or another.

**HDX MediaStream**

HDX MediaStream is used to optimize the delivery of multimedia content, it interacts with the Citrix Receiver (ICA Client) to determine the optimal rendering location (see overview picture below) for Windows Media and Flash content.

Within HDX MediaStream the process of obtaining the multimedia content and displaying the multimedia content are referenced by the terms fetching and rendering respectively.

Within HDX MediaStream, fetching the content is the process of obtaining or downloading the multimedia content from a location external (Internet, Intranet, fileserver (for WMV only)) to the virtual desktop. Rendering utilizes resources on the machine to decompress and display the content within the virtual desktop. In a Citrix virtual desktop that is being accessed via Citrix Receiver, rendering of content can executed by either the client or the hypervisor depending on the policies and environmental resources available.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hdx3.png" class="kg-image" alt loading="lazy" width="432" height="197"></figure>

Adaptive display (server side rendering) provides the ability to fetch and render multimedia content on the virtual machine running in the datacenter and send the rendered content over ICA to the client device. This translates to more bandwidth needed on the network than client side rendering. Howerver in certain scenarios client side rendering can use more bandwidth than server side rendering, it is after all, adaptive.

HDX MediaStream Windows Media Redirection (client side rendering) provides the ability to fetch Windows Media content (inclusive of WMV, DivX, MPEG, etc.) on the server and render the content within the virtual desktop by utilizing the resources on the client hosting Citrix Receiver (Windows or Linux). When Windows Media Redirection is enabled via Citrix policy, Windows video content is sent to the client through an ICA Virtual Channel in its native, compressed format for optimal performance. The processing capability of the client is then utilized to deliver smooth video playback while offloading the server to maximize server scalability. Since the data is sent in its native compressed format this should result in less bandwidth needed on the network than server side rendering.

HDX MediaStream Flash Redirection (client side rendering) provides the ability to harness the bandwidth and processing capability of the client to fetch and render Flash content. By utilizing Internet Explorer API hooks, Citrix Receiver is able to securely capture the content request within the virtual desktop and render the Flash data stream directly on the client machine. Added benefits include increased server hypervisor scalability as the servers are no longer responsible for processing and delivering Flash multimedia to the client.

This usually decreases the wan bandwidth requirements by 2 to 4 times compared to Adaptive Display (server side rendering).

**HDX MediaStream network considerations**

In some cases, Window Media Redirection (client-side rendering of the video) can used significantly more bandwidth than Adaptive Display (server-side rendering of the video).

In the case of low bit rate videos, Adaptive Display may utilize more bandwidth than the native bitrate of the Windows Media content. This extra usage of bandwidth actually occurs since full screen updates are being sent across the connection rather than the actual raw video content.

Packet loss over the WAN connection is the most restricting aspect of an enhanced end-user experience for HDX MediaStream.

Citrix Consulting Solutions recommends Windows Media Redirection (client-side rendering) for WAN connections with a packet loss less than 0.5%.

Windows Media Redirection requires enough available bandwidth to accommodate the video bit rate. This can be controlled using SmartRendering thresholds. SmartRendering controls when the video reverts back to server side rendering because the bandwidth is not available, Citrix recommends setting the threshold to 8Mbps.

WAN optimization should provide the most benefits when the video is rendered on the client since the data stream for the compressed Windows Media content is similar between client devices, once the video has been viewed by one person in the branch, very little bandwidth is consumed when other workers view the same video.

**HDX RichGraphics 3D Pro**

HDX 3D Pro can be used to deliver any application that is compatible with the supported host operating systems, but is particularly suitable for use with DirectX and OpenGL-driven applications, and with rich media such as video.

The computer hosting the application can be either a physical machine or a XenServer VM with Multi-GPU Passthrough. The Multi-GPU Passthrough feature is available with Citrix XenServer 6.0

For CPU-based compression, including lossless compression, HDX 3D Pro supports any display adapter on the host computer that is compatible with the application that you are delivering. To use GPU-based deep compression, HDX 3D Pro requires that the computer hosting the application is equipped with a NVIDIA CUDA-enabled GPU and NVIDIA CUDA 2.1 or later display drivers installed. For optimum performance, Citrix recommends using a GPU with at least 128 parallel CUDA cores for single-monitor access.

To access desktops or applications delivered with XenDesktop and HDX 3D Pro, users must install Citrix Receiver. GPU-based deep compression is only available with the latest versions of Citrix Receiver for Windows and Citrix Receiver for Linux.

HDX 3D Pro supports all monitor resolutions that are supported by the GPU on the host computer. However, for optimum performance with the minimum recommended user device and GPU specifications, Citrix recommends maximum monitor resolutions for users’ devices of 1920 x 1200 pixels for LAN connections and 1280 x 1024 pixels for WAN connections.

Users’ devices do not need a dedicated GPU to access desktops or applications delivered with HDX 3D Pro.

HDX 3D Pro includes an image quality configuration tool that enables users to adjust in real time the balance between image quality and responsiveness to optimize their use of the available bandwidth.

**HDX RichGraphics 3D Pro network considerations**

HDX 3D PRO has significant bandwidth requirements depending on the encoding used (NVIDA CUDA encoding, CPU encoding, and Lossless.)

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hdx4.png" class="kg-image" alt loading="lazy" width="380" height="109"></figure>

When supported NVIDIA chipsets are utilized, HDX 3D Pro offers the ability to compress the ICA session in a video stream. This significantly reduces bandwidth and CPU usage on both ends by utilizing the NVIDA CUDA-based deep compression. If a NVIDIA GPU is not present to provide compression, the server CPU can be utilized to compress the ICA stream. This method, however, does introduce a significant impact on CPU utilization. The highest quality method for delivering a 3D capable desktop is by using the Lossless option. As the Lossless title states, no compression of the ICA stream occurs allowing for pixel perfect images to be delivered to the end point. This option is available for delivering medical imaging software that cannot have degraded image quality. This level of high quality imaging does come with the price of very high bandwidth requirements.

**HDX RichGraphics GDI and GDI+ remoting**

GDI (Graphics Device Interface) and GDI+ remoting allows Microsoft office specifically (although other apps, like wordpad, use GDI also) to be remoted to the client using native graphics commands instead of bitmaps. By using native graphics commands, it saves on server side CPU, saves network bandwidth and eliminates visual artifacts as it doesn’t need to be compressed using image compression.

**General network factors for Remoting protocols (including RDP/RemoteFX, ICA, PCoIP, Quest EoP,…)**

- Bandwidth – the protocols take all they can get, 2 Mbps is required for a decent user experience. (see planning bandwidth requirements below)
- Latency – at 50ms things start getting tough (sometimes even at 20ms)
- Packet loss – should stay under 1%

**Planning bandwidth requirements for HDX (XenDesktop example)**

Citrix publishes the numbers below in a medium (user load) user environment, this gives some indication as to what to expect in terms of network sizing.

- MS Office-based 43Kbps
- Internet 85 Kbps
- Printing (5MB Word doc) 555-593 Kbps
- Flash video (server rendered) 174 Kbps
- Standard WMV video (client rendered) 464 Kbps
- HD WMV video (client rendered) 1812 Kbps

_These are estimates. If a user watches a WMV HD video with a bit rate of 6.5 Mbps, that user will require a network link with at least that much bandwidth. In addition to the WMV video, the link must also be able to support the other user activities happening at the same time._

Also, if multiple users are expected to be accessing the same type of content (videos, web pages, documents, etc.), integrating WAN Optimization into the architecture can drastically reduce the amount of bandwidth consumed. However, the amount of benefit is based on the level of repetition between users.

Note: Riverbed Steelhead can optimize ICA/HDX traffic extremely well, we even support the newer multi-stream ica protocol. In part 2 of this blog I will demonstrate the effectiveness of Steelhead on HDX traffic and talk about our Citrix specific optimizations like our very effective Citrix QoS, Riverbed Steelheads also have the ability to decode the ICA Priority Packet Tagging that identifies the virtual channel from which each Citrix ICA packet originated. As part of this capability, Riverbed specifically developed a packet-order queuing discipline that respects the ordering of ICA packets within a flow, even when different packets from a given flow are classified by Citrix into different ICA virtual channels. This allows the Steelhead to deliver very granular Quality of Service (QoS) enforcement based on the virtual channel in which the ICA data is transmitted. Most importantly, this feature prevents any possibility of out-of-order packet delivery as a result of Riverbed’s QoS enforcement; out-of-order packet delivery would cause significant degradation in performance and responsiveness for the Citrix ICA user. Riverbed’s packet-order queuing capability is patent-pending, and not available from any other WAN optimization vendor.

Real world impact can be seen in the picture below of a customer saving 14GB of ICA traffic over a transatlantic link every month.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/08/hdx5.png" class="kg-image" alt loading="lazy" width="648" height="390" srcset=" __GHOST_URL__ /content/images/size/w600/2021/08/hdx5.png 600w, __GHOST_URL__ /content/images/2021/08/hdx5.png 648w"></figure>