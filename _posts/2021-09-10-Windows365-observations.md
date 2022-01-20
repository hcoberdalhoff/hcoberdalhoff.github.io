---
layout: post
title: Windows 365 observations
date: 2021-09-10
author: Hans-Carl
categories:
    - Windows
    - RemoteWork
---
# Windows 365 
## What are we talking about
I had the opportunity to play around with Windows 365, Microsoft's new Cloud-based Desktop-as-a-Service (DaaS). To be more specific, we are testing out Windows 365 "Business Edition". 

## Cloud PCs
Windows 365 allows to deploy a virtual Windows VM per user on Azure, but will not force you to manage the VM or Azure resources yourself.

Basically, you just assign it like a license to a user and it will just appear for the user.

![Cloud PC Dashboard](/images/2021-09-13-14-08-12.png)

## Editions
Windows 365 "Business" is different from its bigger "Enterprise" counterpart, as 
- You can not choose a custom OS image.
- You can not integrate existing on-prem networks (no VPN/VNet-Attach)
- You can not choose the location of your VMs (the Azure datacenter is chosen for you)
- You can have a max. of 300 VMs 

We actively chose this as it has a wonderful upside: There is no need for an Active Directory Domain Controller. The VMs will natively join you tenant's AzureAD and automatically be picked up by your Intune/Endpoint Manager - if you [configured AzureAD/MEM accordingly](https://docs.microsoft.com/en-us/mem/intune/fundamentals/deployment-guide-enrollment-windows#windows-10-automatic-enrollment). Nice.

# Performance. 
Well - In short - If you push it with load, your notebook will be faster. If you just work casually, you will have a good experience.

Have a look at [ThioJoe's video](https://www.youtube.com/watch?v=RmTAcskU3pM) for more insights and benchmarks. 

## Sizing and Throttling
A bit more nuanced; We chose a 2vCPU configuration. These are hosted on "general purpose" VMs and are perfectly fine for typical Office work. They do feel throttled when under load - which makes sense; MS is expected to protect other VMs on the same host from overly eager competition on resources like CPU and storage throughput. 

When at rest or used for normal office work (which is "spiky" load with a lot of nothing-happens in between) it is responsive and fast even with a 2 vCPU setup.

The currently VMs also have no GPUs - so no gaming ;) Go get [XBox XCloud](https://www.xbox.com/xbox-game-pass/cloud-gaming) if want that.

# Management

## AzureAD and Intune/MEM
The VMs join your AzureAD. As said before, if you configure automatic enrolement in AzureAD/Intune, the VMs will also automatically receive Intune/MEM management, including all policies that already active in your environment. 

![MEM Device Management](/images/2021-09-14-16-03-41.png)

In our case this gave us a great start, as the VM basically came up ready to use in our corporate design and with our applications installed. So, from a OS/client perspective you have basically all management options you could ask for.

![RJ Software Deployment](/images/2021-09-14-16-13-51.png)

Btw, shout out to our friends at [glueckkanja-gab AG](http://glueckkanja-gab.com/) for the awesome RealmJoin addon and software deployment :)

## Provisioning
Windows 365 Business Edition (the service, not the VMs) on the other side does not have any central management from an admin perspective. No Provisioning configuration in Intune/MEM for example, which you would get with Windows 365 Enterprise. This actually makes sense, as you don't need it: You can't choose VM-images, Azure Datacenter locations or the initial OS language in this edition of the service. So, who needs a GUI for nothing? ;)

![Nothing here](/images/2021-09-14-16-01-39.png)

The only amount of central management is assigning the Windows365 license to a user.

## Self-Service
A user can self-service his VM. Basically you can reboot you own VM if something hangs (or you actually managed to shut down your own VM). Or can destructively restore/recreate your VM from scratch, if you seriously messed up your VM.

![Self Service Options](/images/2021-09-14-11-43-07.png)

Sorry for the screenshot being in German, btw.

# Security and Longevity

## No TPM, No SecureBoot
The VMs offered are "v1" VMs, they do not have (virtual) TPM and no SecureBoot. If you configure Bitlocker/Drive Encryption as Intune/MEM policy, it is implicitly skipped on these VMs. On the plus side, you don't need to change your existing policy sets. As a VM can't be lost on a train or in a hotel, the main reason for disk encryption on a Windows client is not given - so.. ok.

The missing SecureBoot and other TPM-based features like CredentialGuard leave these VMs open to some malware/cyber-attacks most current laptops are protected against. This is not a major risk, but you should be aware.

### Update 2022-01
Windows 365 Business now offers Windows 11 on newer "Gen 2" VMs by default. These VMs do have a TPM module (as to support by Windows 11) BUT they do not offer disk encryption. (Probably to prevent unneeded load on the storage systems.)

## Windows 11
Missing TPM and SecureBoot might harm the longevity of these VMs - Windows 11 does require a TPM and Secure Boot. What will happen then? Will every user have to destroy his existing VM and create a new one? Or will there be some in-place migration? Btw, do not try to update your VM to Windows 11 yourself right now. [It will fail.](https://www.zdnet.com/article/windows-365-can-you-install-windows-11-on-a-cloud-pc/) I tried, too.

### Update 2022-01
Windows 365 Business now offers Windows 11 by default. This is done using "Gen 2" VMs incl. a virtual TPM module as mentioned above. You can not inplace-upgrade an existing VM, but when you "recreate" (destroy/rebuild) your VM it will automagically get Windows 11. No, there is no choice btw - at least with Win365 Business.

## Do you need Encryption?
About hosting your working environment on Azure: As with any workload you host in the cloud - you have to trust your hoster, in this case Microsoft, to a certain degree regarding your data. As most people using this service will also probably be using e.g. Office365 this should be a given. There is no easy way here to completely encrypt/lock down the VM from the inside.

### Update 2022-01
Even though the new Win11 VMs do have a TPM module, disk encryption is still suppressed. 

## Check your Device Compliance rules
About trust... If you Intune/MEM and AzureAD Conditional Access, be aware that if you "trust" compliant devices you might have to recheck those policies and assumptions. A Windows365 VM will not offer biometry like Windows Hello and no encryption (see above). Also it is not in the "physical" possession of your user. 

## Deployment Location
We are located in Germany. According to short tests using the [Office Connectivity Check](https://connectivity.office.com/), the VMs we got are hosted in the UK. 

![Office Connectivity Check](/images/2021-09-14-15-40-47.png)

So, if you have rules/regulations forcing e.g. your data or compute to be in your own country - you might need to upgrade to Windows365 Enterprise and manually choose the deployment location of your VMs.

I imagine some security-officers at medium and large sized companies will have questions about these VMs, which are not encrypted, do not have SecureBoot, are possibly not in the right country and might (in many cases) be implicitly trusted/compliant" in regards to Conditional Access. Just keep that in mind if you are the IT-guy or -girl recommending the service. ;)

## (No) Single Sign On 
Something that reminds me strongly of WVD/AVD.. There is no SSO from AzureAD into the VM. You sign in to either https://windows365.microsoft.com or your Remote Desktop Client using AzureAD credentials, but when you start/connect to your VM you need to present you password again. 

![No SSO](/images/2021-09-14-15-52-45.png)

You can save your password on all of the clients, so it does not hinder you day to day. I just hope MS will improve this in the future, so we do not unnecessarily store credentials in many places.

## Similarities to AVD
If you feel reminded of WVD/AVD, you might be onto something. The "Feed URL" and the URL of the web client in the service are identical to those in WVD/AVD. There seems to be a lot of shared code across these products (which totally makes sense, btw).

# Productivity

## IPads ..
One primary usecase in our company is to allow VIPs to have access to a Windows environment from an iPad. And to be honest this works pretty fantastic when used on an iPad! Mouse and Keyboard are well supported. The Remote Desktop Client (RD Client) even shapes the iPadOS blob of a mouse cursor into a Windows-themed cursor. 

## .. and external Monitors
One caveat is, that the iOS/iPadOS RD Client does not really support external monitors. It works, but you get the obligatory letterboxing / black borders as with any other regular iPad-app. Also the resolutions offered by the client do not really match the needs of most external displays. 

![iPad Resolutions](/images/2021-09-14-16-27-53.png)

I hope MS will improve this - yes, there are apps with correct support for external screens on iPads ;)

## WebClient, Windows etc.
Windows365 uses the same HTML5 web client and RD Client already seen with WVD/AVD. They do work and they do work nicely.

## It is a Remote Session
It is "RDP-like" remote session - how will it handle in place Windows Version upgrades? Remember, installing the Windows 11 Beta fails probably because of that. 

Applications that do generally not work over RDP will be a problem here to. (Ok, to be fair - there are not maaaany of those anymore luckily).

## What about Apps in my Datacenter?
Windows 365 Business has no means to connect to an existing Azure- or on-prem network. If your apps are not exposed to the internet, you won't reach them.

Of course you can install a VPM-Client in your VM - as long as this will not cut of your own remote session ;) So there might be ways to do it. It just is not part of the offering.

If you need on-prem connectivity, use Windows 365 Enterprise or AVD which support these usecases.

## OS Customization
Everyone gets the same image - Currently the OS is deploying with region set to USA and language set to `en-us` although we are in Germany. At least the VM seems to be in the UK, not in the US, so MS seems to try to locate it near to you.

You will need to use tools like Intune/MEM to configure and customize the VMs - or tell the user how to change language etc. if needed.

## Teams Video and Audio Integration
Microsoft Teams is prepared to be used with this service. If you have a fitting device and client to access the service, your audio and video will be redirected to your physical device and not suffer latency and bandwidth limitations of a remote session. This currently works fine on the Windows RD Client. It somewhat work iPad RD Client, but is not advertised there (yet). If you use an iPad, use the native Teams app for now. 

Of course, using full versions of Office and Teams on an iPad for collaboration on documents with Windows365 is sweet.

# Summary
Windows365 (Business Edition) is a nice and simple, no-headache service to offer Windows based Cloud-PCs to your users, especially if you have no existing infrastructure outside of AzureAD and Office365.

Be aware of performance and security-implications like the missing SecureBoot and TPM.

If you need more control over OS images, networking and location of VMs, on-prem connectivity - go to Windows 365 Enterprise (instead of Business) or use AVD.

