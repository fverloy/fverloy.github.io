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

<img src="/assets/img/Screenshot-2021-10-20-at-16.22.56.png">

Select the .iso image and click continue

<img src="/assets/img/Screenshot-2021-10-20-at-16.24.18.png">

Select "Windows 10 and later x64"

<img src="/assets/img/Screenshot-2021-10-20-at-16.24.59.png">

Select UEFI and optionally UEFI Secure Boot

<img src="/assets/img/Screenshot-2021-10-20-at-16.25.40.png">

Instead of selecting Finish, go ahead and click Customize Settings if you want to change the name of the VM or the location of the files, if not, click Finish.

<img src="/assets/img/Screenshot-2021-10-20-at-16.26.53.png">

Next click on 'Encryption' and enter a password of your choice

<img src="/assets/img/Screenshot-2021-10-20-at-16.29.39.png">

The VM will now be encrypted

<img src="/assets/img/Screenshot-2021-10-20-at-16.31.59.png">

Next we need to add a Trusted Platform Module as that is a prerequisite for Windows 11. You can do this by going back to all settings and clicking ' Add Device...' on the top right

<img src="/assets/img/Screenshot-2021-10-20-at-16.33.56.png">

Select 'Trusted Platform Module' and click 'Add...'

<img src="/assets/img/Screenshot-2021-10-20-at-16.34.21.png">

Acknowledge the information pane and click on 'Show All'

<img src="/assets/img/Screenshot-2021-10-20-at-16.35.44.png">

Optionally you can disable side channel mitigations to improve the performance (but obviously lower its security posture) of the VM. You do this by selecting 'Advanced'

<img src="/assets/img/Screenshot-2021-10-20-at-16.37.17.png">

Click the 'Disable Side Channel Mitigations' checkbox

<img src="/assets/img/Screenshot-2021-10-20-at-16.37.49.png">

Now you can change the amount of virtual CPU and Memory that is allocated to the VM, you can also opt to enable hypervisor applications in the VM if you want to make use of WSL 2 (Windows Subsystem for Linux), which uses Hyper-V to run a Linux distribution on Windows 11. (Turtles all the way down).

<img src="/assets/img/Screenshot-2021-10-20-at-16.41.58.png">

If you are happy with your settings go ahead and boot the VM and follow the Windows installation steps.

<img src="/assets/img/Screenshot-2021-10-20-at-16.43.19.png">

No more Cortana during setup (yay!)

<img src="/assets/img/Screenshot-2021-10-20-at-16.50.43.png">

Once you are done with setup you can install VMware Tools to add to relevant drivers to Windows 11. Click on 'Virtual Machine' --\> 'Install VMware Tools'

<img src="/assets/img/Screenshot-2021-10-20-at-16.57.50.png">

Doubleclick 'setup64' to install VMware Tools and run through the installation wizard. After the wizard finishes you need to restart the VM.

<img src="/assets/img/Screenshot-2021-10-20-at-16.59.57.png">

After the reboot you can enjoy a more appropriate resolution.

<img src="/assets/img/Screenshot-2021-10-20-at-17.03.16.png">

To install WSL, right click the start button and select 'Windows Terminal (Admin)'

<img src="/assets/img/Screenshot-2021-10-20-at-17.04.17.png">

In Windows Terminal (you are in the Windows Powershell shell by default) type  
wsl --install

<img src="/assets/img/Screenshot-2021-10-20-at-17.04.40.png">

Once installation is complete, reboot your VM for the changes to take effect.

<img src="/assets/img/Screenshot-2021-10-20-at-17.07.05.png">

After the reboot, the installation of Ubuntu (the default distribution used by WSL) continues

<img src="/assets/img/Screenshot-2021-10-20-at-17.13.22.png">

et voilà! MacOS running VMware Fusion running Windows 11 running Hyper-v running Ubuntu Linux

<img src="/assets/img/image.png">

NOTE: There is another, experimental way to achieve the same goal and potentially with better performance.

VMware is also shipping a currently-experimental vTPM device that boasts a reduced performance impact by employing a new encryption model, as well as several new options for managing Fusion and Workstation installations at scale.

To use this experimental device, just add the following to the Windows 11 .vmx config file and restart Fusion:

    managedVM.autoAddVTPM="software"

Go the Virtual Machine folder on your MacBook and rightclick the VM, select 'Show Package Contents'

<img src="/assets/img/Screenshot-2021-11-05-at-21.33.28.png">

Select the .vmx file and open it in TextEdit and add the line above to end of the file and save it.

<img src="/assets/img/Screenshot-2021-11-05-at-21.33.48.png">
<img src="/assets/img/Screenshot-2021-11-11-at-15.34.46.png">

This replaces the need to do full-VM encryption.

