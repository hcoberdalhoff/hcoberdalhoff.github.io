---
layout: post
title: Windows 365 observations
date: 2021-09-10
author: Hans-Carl
categories:
    - Windows
    - RemoteWork
---
# Windows 365 - What are we talking about
I had the opportunity to play around with Windows 365, Microsoft's new Cloud-based Desktop-as-a-Service (DaaS). To be more specific, we are testing out Windows 365 "Business". 

Windows 365 allows to deploy a virtual Windows VM per user on Azure, but will not force you to manage the VM or Azure resources yourself.

Basically, you just assign it like a license to a user and it will just appear for the user.

![Cloud PC Dashboard](/images/2021-09-13-14-08-12.png)

Windows 365 "Business" is different from its bigger "Enterprise" counterpart, as 
- You can not choose a custom OS image.
- You can not integrate existing on-prem networks (no VPN/VNet-Attach)
- You can not choose the location of your VMs (the Azure datacenter is chosen for you)
- You can have a max. of 300 VMs 

We actively chose this as it has a wonderful upside: There is no need for an Active Directory Domain Controller. The VMs will natively join you tenant's AzureAD and automatically be picked up by your Intune/Endpoint Manager - if you [configured AzureAD/MEM accordingly](https://docs.microsoft.com/en-us/mem/intune/fundamentals/deployment-guide-enrollment-windows#windows-10-automatic-enrollment). Nice.

# Performance. 
Well - In short - If you push it with load, your notebook will be faster. If you just work casually, you will have a good experience.

A bit more nuanced; We chose a 2vCPU configuration. These are hosted on "general purpose" VMs and are perfectly fine for typical Office work. They do feel throttled when under load - which makes sense; MS is expected to protect other VMs on the same host from overly eager competition on resources like CPU and storage throughput. 

When at rest or used for normal office work (which is "spiky" load with a lot of nothing-happens inbetween) it is responsive and fast even with a 2 vCPU setup.

# Management
Windows 365 does not have central management from an admin perspective. No MEM Provisioning UI for exmaple, which you get with Windows 365 Enterprise. This actually makes sense, as you don't need it: You can't choose VM-images, Azure Datacenter locations etc. So, who needs a GUI for nothing? ;)

The only amount of central management is assigning the Windows365 license to a user.

A user on the other hand can self-service his VM. Basically do a "restart" if something hangs (or you actually managed to shut down your own VM). Or can destructively restore/recreate your VM, if you seriously messed up your VM.

![Self Service Options](/images/2021-09-14-11-43-07.png)

Sorry for the screenshot being in german, btw.

# Security and Longevity
No (v)TPM, SecureBoot, Bitlocker is skipped implicitely. No changes needed. But this might harm the longevity of these VMs - Windows 11 does require a TPM and Secure Boot.

Be aware if you "trust" compliant devices - no biometry. I imagine some security-officers have interesting questions about VMs, that are not encrypted, not in the right country and might (in many cases) be implictely trusted ("compliant") in regards to Conditional Access.

No SSO (saved PWs possible) in the clients. 

The SSO thingy and the URLs of the "feed" etc. strongly remind me of WVD/AVD. There seems to be a lot of shared code base.

Ipad. Fantastic! But not on a monitor - strange resolution, letterboxing. Mouse has an actualy "Windows" Pointer, not the ipad "blob".

It is "RDP-like" remote session - how will it handle in place upgrades? Win11 Beta fails probably because of that. Applications that do not work over RDP will be a problem here to. (Which are not maaaany anymore luckily).

Everyone gets the same image - Currently OS is deploying with region set to "US" and language en-us although I am in Germany also as usage location and my user has de-de as language. At least the VM seems to be in the UK, not in the US.

Teams-redirection on a Windows client seems to work reasonably well. But if you can, use a native Teams-Client for calling :)