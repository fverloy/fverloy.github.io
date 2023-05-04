---
layout: post
title: Running Windows 11 (and WSL) on Mac using VMware Fusion 12.2
date: '2021-10-20 15:16:22'
tags:
- vmware
- windows
- microsoft
---

I needed a Windows 11 VM to do some testing and since I currently don't have any Windows hardware anymore I decided to use VMware Fusion on my (intel) mac instead. Follow the steps below to avoid the dreaded "this machine is not compatible with Windows 11" message because a (v)TPM is missing by default.

First step is to acquire an ISO image which you can find here: [https://www.microsoft.com/en-us/software-download/windows11](https://www.microsoft.com/en-us/software-download/windows11)

Next we run Fusion and create a new VM using the "install from disc or image" option.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.22.56.png" class="kg-image" alt loading="lazy" width="1322" height="1096" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.22.56.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.22.56.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.22.56.png 1322w" sizes="(min-width: 720px) 720px"></figure>

Select the .iso image and click continue

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.24.18.png" class="kg-image" alt loading="lazy" width="1314" height="1094" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.24.18.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.24.18.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.24.18.png 1314w" sizes="(min-width: 720px) 720px"></figure>

Select "Windows 10 and later x64"

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.24.59.png" class="kg-image" alt loading="lazy" width="1316" height="1100" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.24.59.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.24.59.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.24.59.png 1316w" sizes="(min-width: 720px) 720px"></figure>

Select UEFI and optionally UEFI Secure Boot

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.25.40.png" class="kg-image" alt loading="lazy" width="1326" height="1094" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.25.40.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.25.40.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.25.40.png 1326w" sizes="(min-width: 720px) 720px"></figure>

Instead of selecting Finish, go ahead and click Customize Settings if you want to change the name of the VM or the location of the files, if not, click Finish.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.26.53.png" class="kg-image" alt loading="lazy" width="1316" height="1100" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.26.53.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.26.53.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.26.53.png 1316w" sizes="(min-width: 720px) 720px"></figure>

Next click on 'Encryption' and enter a password of your choice

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.29.39.png" class="kg-image" alt loading="lazy" width="1322" height="792" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.29.39.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.29.39.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.29.39.png 1322w" sizes="(min-width: 720px) 720px"></figure><figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.30.37.png" class="kg-image" alt loading="lazy" width="1330" height="614" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.30.37.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.30.37.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.30.37.png 1330w" sizes="(min-width: 720px) 720px"></figure>

The VM will now be encrypted

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.31.59.png" class="kg-image" alt loading="lazy" width="1342" height="580" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.31.59.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.31.59.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.31.59.png 1342w" sizes="(min-width: 720px) 720px"></figure>

Next we need to add a Trusted Platform Module as that is a prerequisite for Windows 11. You can do this by going back to all settings and clicking ' Add Device...' on the top right

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.33.56.png" class="kg-image" alt loading="lazy" width="1320" height="796" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.33.56.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.33.56.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.33.56.png 1320w" sizes="(min-width: 720px) 720px"></figure>

Select 'Trusted Platform Module' and click 'Add...'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.34.21.png" class="kg-image" alt loading="lazy" width="1324" height="694" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.34.21.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.34.21.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.34.21.png 1324w" sizes="(min-width: 720px) 720px"></figure>

Acknowledge the information pane and click on 'Show All'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.35.44.png" class="kg-image" alt loading="lazy" width="1318" height="328" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.35.44.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.35.44.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.35.44.png 1318w" sizes="(min-width: 720px) 720px"></figure>

Optionally you can disable side channel mitigations to improve the performance (but obviously lower its security posture) of the VM. You do this by selecting 'Advanced'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.37.17.png" class="kg-image" alt loading="lazy" width="1316" height="956" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.37.17.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.37.17.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.37.17.png 1316w" sizes="(min-width: 720px) 720px"></figure>

Click the 'Disable Side Channel Mitigations' checkbox

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.37.49.png" class="kg-image" alt loading="lazy" width="1314" height="1302" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.37.49.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.37.49.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.37.49.png 1314w" sizes="(min-width: 720px) 720px"></figure>

Now you can change the amount of virtual CPU and Memory that is allocated to the VM, you can also opt to enable hypervisor applications in the VM if you want to make use of WSL 2 (Windows Subsystem for Linux), which uses Hyper-V to run a Linux distribution on Windows 11. (Turtles all the way down).

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.41.58.png" class="kg-image" alt loading="lazy" width="1422" height="926" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.41.58.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.41.58.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.41.58.png 1422w" sizes="(min-width: 720px) 720px"></figure>

If you are happy with your settings go ahead and boot the VM and follow the Windows installation steps.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.43.19.png" class="kg-image" alt loading="lazy" width="2000" height="1577" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.43.19.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.43.19.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-16.43.19.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.43.19.png 2080w" sizes="(min-width: 720px) 720px"></figure>

No more Cortana during setup (yay!)

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.50.43.png" class="kg-image" alt loading="lazy" width="2000" height="1577" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.50.43.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.50.43.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-16.50.43.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.50.43.png 2054w" sizes="(min-width: 720px) 720px"></figure>

Once you are done with setup you can install VMware Tools to add to relevant drivers to Windows 11. Click on 'Virtual Machine' --\> 'Install VMware Tools'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.57.50.png" class="kg-image" alt loading="lazy" width="2000" height="1527" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.57.50.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.57.50.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-16.57.50.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.57.50.png 2180w" sizes="(min-width: 720px) 720px"></figure>

Doubleclick 'setup64' to install VMware Tools and run through the installation wizard. After the wizard finishes you need to restart the VM.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.59.57.png" class="kg-image" alt loading="lazy" width="1588" height="1008" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-16.59.57.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-16.59.57.png 1000w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-16.59.57.png 1588w" sizes="(min-width: 720px) 720px"></figure>

After the reboot you can enjoy a more appropriate resolution.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.03.16.png" class="kg-image" alt loading="lazy" width="1921" height="1081" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-17.03.16.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-17.03.16.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-17.03.16.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.03.16.png 1921w" sizes="(min-width: 720px) 720px"></figure>

To install WSL, right click the start button and select 'Windows Terminal (Admin)'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.04.17.png" class="kg-image" alt loading="lazy" width="936" height="1274" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-17.04.17.png 600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.04.17.png 936w" sizes="(min-width: 720px) 720px"></figure>

In Windows Terminal (you are in the Windows Powershell shell by default) type  
wsl --install

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.04.40.png" class="kg-image" alt loading="lazy" width="2000" height="1097" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-17.04.40.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-17.04.40.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-17.04.40.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.04.40.png 2380w" sizes="(min-width: 720px) 720px"></figure>

Once installation is complete, reboot your VM for the changes to take effect.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.07.05.png" class="kg-image" alt loading="lazy" width="2000" height="1088" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-17.07.05.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-17.07.05.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-17.07.05.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.07.05.png 2382w" sizes="(min-width: 720px) 720px"></figure>

After the reboot, the installation of Ubuntu (the default distribution used by WSL) continues

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.13.22.png" class="kg-image" alt loading="lazy" width="1962" height="1310" srcset=" __GHOST_URL__ /content/images/size/w600/2021/10/Screenshot-2021-10-20-at-17.13.22.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/10/Screenshot-2021-10-20-at-17.13.22.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/10/Screenshot-2021-10-20-at-17.13.22.png 1600w, __GHOST_URL__ /content/images/2021/10/Screenshot-2021-10-20-at-17.13.22.png 1962w" sizes="(min-width: 720px) 720px"></figure>

et voilà! MacOS running VMware Fusion running Windows 11 running Hyper-v running Ubuntu Linux

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/10/image.png" class="kg-image" alt loading="lazy" width="400" height="226"></figure>

NOTE: There is another, experimental way to achieve the same goal and potentially with better performance.

VMware is also shipping a currently-experimental vTPM device that boasts a reduced performance impact by employing a new encryption model, as well as several new options for managing Fusion and Workstation installations at scale.

To use this experimental device, just add the following to the Windows 11 .vmx config file and restart Fusion:

    managedVM.autoAddVTPM="software"

Go the Virtual Machine folder on your MacBook and rightclick the VM, select 'Show Package Contents'

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-21.33.28.png" class="kg-image" alt loading="lazy" width="672" height="348" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-21.33.28.png 600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-21.33.28.png 672w"></figure>

Select the .vmx file and open it in TextEdit and add the line above to end of the file and save it.

<figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-21.33.48.png" class="kg-image" alt loading="lazy" width="1472" height="424" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-05-at-21.33.48.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-05-at-21.33.48.png 1000w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-05-at-21.33.48.png 1472w" sizes="(min-width: 720px) 720px"></figure><figure class="kg-card kg-image-card"><img src=" __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-11-at-15.34.46.png" class="kg-image" alt loading="lazy" width="2000" height="1007" srcset=" __GHOST_URL__ /content/images/size/w600/2021/11/Screenshot-2021-11-11-at-15.34.46.png 600w, __GHOST_URL__ /content/images/size/w1000/2021/11/Screenshot-2021-11-11-at-15.34.46.png 1000w, __GHOST_URL__ /content/images/size/w1600/2021/11/Screenshot-2021-11-11-at-15.34.46.png 1600w, __GHOST_URL__ /content/images/2021/11/Screenshot-2021-11-11-at-15.34.46.png 2146w" sizes="(min-width: 720px) 720px"></figure>

This replaces the need to do full-VM encryption.

