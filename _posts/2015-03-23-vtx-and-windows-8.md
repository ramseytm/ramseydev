---
layout: post
title: "VT-X and Windows 8.1: a short-lived frustration."
description: "An error you may run into with VirtualBox, VT-x on Windows 8 and the way to fix it."
author: Tyler Ramsey
date: 2015-03-23 10:07:10
twitter_card: summary
tags:
- troubleshooting
- VMs
- windows 8
- virtual box
---

So tonight I ran into a bit of frustration with Windows 8. Like most IT folks, I have a few VMs on my local machine that I run with [VirtualBox](https://www.virtualbox.org/ "Virtual Box") and as you may have noticed, the VM I use when editing this site has gone unused since, er, last November apparently (yikes!). Tonight I figured I should probably take a look and run an update or two when to my suprise I'm greeted to this wonderful error upon start up:

>VT-x is not available. (VERR_VMX_NO_VMX)

<!--excerpt-->
<a name="start" />

Well that's interesting, it *was* available last time I checked, er, five months ago (double yikes!). For those that don't know VT-x refers to [Intel's hardware virtualization technology](http://en.wikipedia.org/wiki/X86_virtualization#Intel_virtualization_.28VT-x.29 "VT-x"). It's required for taking full advantage of 64-bit hosts, such as my desktop PC, and running 64-bit guest VMs, such as the one I'm writing this post on. VT-x is usually presented as a setting in your bios screen that can be enabled or disabled.

Since this VM used to work I found it unlikely that VT-x was magically disabled without my knowing but I decided to be deligent, for once!, and check my bios settings before skipping on to other steps. After about 10 minutes of perusing my bios screen I finally found the setting, under overclocking settings of all things (thanks MSI!), and lo and behold it's **enabled**.

OK, with that out of way it was time to determine why VirtualBox was incorrectly detecting my virtualization settings. Since it used to work, I automatically blamed a Windows Update as I'm wont to do. Sure enough, a short Google search and I'm presented with a number of results mentioning [Hyper-V](https://technet.microsoft.com/library/hh831531.aspx "Hyper-V"), Microsoft's hypervisor, not playing nice with VT-x. Several articles mention VT-x being effectively disabled when Hyper-V is enabled on a host, so the logical next step was to take a look and see if Hyper-V got enabled.

Hyper-V is installed as a Windows Feature and can be found by navigating to the following:

>Control Panel -> Programs and Features -> Turn Windows features on and off -> Hyper-V

If it's checked it's installed and likely running, and of course it was checked on my machine. After unchecking the Hyper-V feature and restarting VirtualBox I was then able to start up the VM just fine<sup>1</sup>. So the whole process took less time than it took to write up this post but I found it frustrating none the less. Let's hope it's not another five months before I find my VM won't start!

<br />
<br />
<sup>1: I also had to reset my VM instance to be set to the 64-bit version of the OS as apparently VirtualBox automatically sets it to 32-bit when it's unable to detect VT-x ¯\\\_(ツ)\_/¯.</sup>
