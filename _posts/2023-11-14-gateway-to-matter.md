---
title: Gateway to Matter
header:
  teaser: /assets/images/HA-yellow.jpg
excerpt: The device that brings it all together, home assistant yellow
#classes: wide
tags: [homeassistant, matter]
---

My two main pain points with commodity "Smart Home" products: 
- app sprawl
- internet latency/dependency

Every single vendor has its own app that you need to use for onboarding that is going to do something wonky with bluetooth or try to setup a local wifi which is always a pain, then you have to join those things to wifi and they typically only support 2.4Ghz and my spectrum is just clogged with chatty junk.  Then you want to integrate it into you home ecosystem and whether its goolgle, alexa, or whatever now that app is making API calls back and forth out to through the internet just to turn the lights on right next to it.  I've already had a couple of random little "smart stuff" vendors go belly up and now i got brick because they forgot to pay the AWS lorawan bill.  Or worse yet my girlfriend buys some remote something for the cats and its not working because i'm firewalling china and guess where they are sending their data to?  

Now i know zigbee+home assistant has already solved a lot of those problems, but the research I've done on matter and thread got me excited about the new standards and piqued my interest enough to want to dip my toes in the water.  

**What even is matter and thread?** A bunch of IoT OEMs got together and said "y'all we need to standardize this shit" and developed a set of control protocols known as `matter`.  The matter devices use a communication protocol known as `thread` which is pretty much zigbee plus ipv6.
{: .notice--info}

Typically any services i want to deploy for the house are going to be either containerized or virtualized, but because i'm wanting to move away from the tech giants, i didnt want to use a one of the existing thread border routers and thats when i stumbled across the home assistant yellow and it was checking a lot of my boxes: 

- [x] Open Source hardware and software
- [x] Matter Controller
- [x] Thread Border Router
- [x] Add on storage with nvme
- [x] Bring your own compute with raspi CM 4

![homeassistant-yellow]({{ site.url }}{{ site.baseurl }}/assets/images/HA-yellow.jpg)  
  

Here is my home assistant yellow kit fitted with my raspi compute module 4 under the heatsink and tiny nvme drive i had leftover from when i upgraded my steamdeck.  Wasnt sure what i was gonna use that SSD for but this seems like the perfect use case.  Now if you guys are like me you have been buying raspberry pi's since gen1, but i somehow missed the mark on this compute module line.  Its just the pi4 compute components without any of the i/o.  Look at this tiny little thing (full size sd card for scale)

![rpicm4]({{ site.url }}{{ site.baseurl }}/assets/images/rpicm4.jpg)

Neat huh? Those compute modules can be tricky to locate so I recommend using something like rpilocator.com to source one. And y'all theres some kinda magic built into this thing, because the instructions just told me to flash a usb drive with the image and plug it in and wait for the lights to stop blinking and then reboot.  There is no video out, but you can use the usb-c as a serial port.  I was fully preparing to need the serial port for troubleshooting because there was going to be some issue with the image or some issue with the compute or the nvme or something, nothing ever works the first time right?  Not the case with the fine folks at home assistant, they got this shit figured out.  I RTFM'd and sure enough it came right up.  Set a static ip address and put a DNS A record in pi-hole and we were up and running with a new home assistant.  
  
##### ***Next time on Craig's Blog***:  Setting up matter and thread and onboarding our first matter device
See you then. Cheers! 🍻

