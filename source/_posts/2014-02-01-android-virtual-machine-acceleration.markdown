---
layout: post
title: "Android Virtual Machine Acceleration"
date: 2014-02-01 14:15:29 -0500
comments: true
categories: [Android, Intel, Emulation]
---

I've dabbled in Android development in the past, but haven't done much with emulation. In my experience the Android emulators are too slow to use, and I've usually used my phone for testing/debugging.

Intel's [Hardware Accelerated Execution Manager](http://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager) (HAXM) uses CPU virtualization to speed up Android emulators. I wanted to give it a try. 

## CPU Compatibility
I have a pretty old desktop machine, so I wasn't even sure if my CPU supported hardware virtualization. Poking around a bit, there are a few Windows utilities that can be used to determine if virtualizaiton is enabled on your CPU.

* [http://www.intel.com/support/processors/sb/cs-030729.htm](http://www.intel.com/support/processors/sb/cs-030729.htm)
* [http://www.microsoft.com/en-us/download/details.aspx?id=592](http://www.microsoft.com/en-us/download/details.aspx?id=592)
* [https://www.grc.com/securable.htm](https://www.grc.com/securable.htm)

The Intel utility looks like this:

<img width="525px" height="384px" src="{{ root_url }}/images/android_virtual/vmx_enabled.jpg"/>

## Results
With virtualization verified and enabled, I followed [this](http://developer.android.com/tools/devices/emulator.html#acceleration) writeup to install everything and setup an x86 Android emulator.

So how did it work? It wasn't that much faster, but there was a noticeable difference. The HAXM emulator booted in 182 seconds, compared to 282 seconds for the ARM emulator. With both emulators open, I could click on a button (e.g., open the Settings app) and the HAXM emulator would finish first, usually by 1-5 seconds depending on the task. As a disclaimer, I was using a pretty old computer. I'm not sure what impact that had on my experience.

Regardless, while there was some speedup, it's nowhere close to testing/debugging on a real phone (as is to be expected). The emulator is nice for testing different phones/configurations, but in general I find it much nicer to just push my application to a real phone.