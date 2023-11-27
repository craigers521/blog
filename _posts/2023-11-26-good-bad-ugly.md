---
title: Matter and Thread - The Good, The Bad and The Ugly
header:
  teaser: /assets/images/post5/cowboybulb.png
  excerpt: My experience so far converting to matter and thread 
description: My experience deploying matter and thread devices from tapo, nanoleaf, eve and others into home assistant yellow
tags: [homeassistant, matter, thread]
---
![cowboy lightbulb]({{ site.url }}{{ site.baseurl }}/assets/images/post5/cowboybulb.png)  

#### The Good  
So I have replaced all the major lights we use in the house with matter enabled bulbs.  I have multi-admin with google home for voice control (possible project in the future to replace with voice control with rhasspy or mycroft) and home assistant handling all the automation tasks like motion triggers and schedules. I still love that I don't need multiple vendor apps and everything is just native so that experience has been really great.  But not perfect...  

![matter homeassistant]({{ site.url }}{{ site.baseurl }}/assets/images/post5/matter_devices.png)

#### The Bad  
So onboarding when I get the automagic prompt on my phone that a matter device has been detected is mostly seamless.  I have found however that sometimes a) I don't get the prompt and need to add matter device manually which is sometimes touch and go if it works and more importantly b) for devices I want home assistant to control exclusively, I have had mixed results if the device gets added successfully or not.  If however I add the device to google home first then link matter services to home assistant it works much more consistently.  Now I'm not sure what this means for the actual thread network, do I have multiple thread networks? One talking to google and one talking to HA? Or is it the same thread network and google is the "primary" controller and shares info with HA? The multi-admin setup is kind of confusing in that regard.  

Also I did have another tapo device, the smart plug.  Similar experience as with the bulb:  I had trouble getting it commissioned and when I did finally it just won't stay online.  Tapo is wifi based not thread, and I have an AP in the same room... but this thing just won't stay online.  I've ordered some eve plugs to replace the use case I had. Will report back on that.  

#### The Ugly

![angry computer meme]({{ site.url }}{{ site.baseurl }}/assets/images/post5/angry_computer.png)  

So on the topic of thread... I have no way to visualize or troubleshoot my thread network.  I have one bulb in the far corner of the house and I've noticed sometimes it just becomes unresponsive.  I plan on getting a "Full Thread Device" positioned near it to try to help with the mesh bridging.  TBD on that I will report back.  Eve allegedly has this built into their app to see and troubleshoot the thread network, they are the only one that I am aware of, but of course if you want their app that's iOS only.  I did have an extra eve motion sensor I planned to setup for the basement lights, and I have an apple tv that can act as a hub for homekit (thats the next project, moving that to a raspberry pi), so I thought well let's just try to add this eve sensor to homekit and see what I can see with the thread network.  After probably an hour of screwing around with icloud and homekit I just gave up.  Eventually I got as far as "eve needs a thread border router" which my apple 4k tv allegedly is, but not worth the fight to setup a 3rd TBR that I want to replace, that just sounds like more trouble than it's worth.   

**FTD vs MTD** FYI a FTD or full thread device is usually on house power and can act as a thread router, so other endpoints can connect to it and it can relay back to the thread border router/matter controller.  Whereas MTD or minimal thread devices are usually battery powered endpoints, so they save energy by not participating in the full mesh and just connect to their their closest thread router.  A really nice write up on that [HERE](https://www.nordicsemi.com/Products/Thread/What-is-Thread)
{: .notice--info}  

**TL;DR:**  troubleshooting thread with homeassistant is not easy.  I turned up logging and can see some ipv6 back and forth but I don't really know what to make of it.  

Another word of warning:  After I got the rest of the nanoleaf bulbs in downstairs.  I thought I'd show off some fun with home assistant and the fact that I now have RGB and create a fun little automation to flash all the bulbs different colors.  So I setup a few different scenes with different color arrangements then created an automation that I could manually trigger to play through the different scenes.  I wanted them to cycle quickly between each scene, so I had set a delay of 300ms.  I played the automation and the bulbs freaked out.  Apparently they do **NOT** like being color cycled rapidly.  I tried to just go back to my normal scene but even now a couple of the bulbs are like, in a weird dim flickery state.  I am hoping they are not permanently hosed and I can factory reset them and re-onboard them to get them happy again.  But of course that's a ton of work to remove the devices/entities, factory reset, re-onboard.  So word to the wise: 

**DO NOT RAPIDLY PLAY DIFFERENT SCENES ON YOUR BULBS**
{: .notice--danger}

#### Whats next:  
I am going to keep playing with matter and thread and home assistant, but I have a lot more projects to work on in my open source migration.  Here's what I am thinking in no particular order:  
- move tv set top/stream box to raspberry pi
- add ratgdo to garage for homeassistant control since myQ sucks
- open source voice assistant
- unified chat application using matrix (looking at beeper self hosted)
- looking at better hvac control, maybe with some automated dampers on registers to create pseudo mutli-zone
- open phone? Nothing phone? LineageOS? 
- move gaming PC to linux (oof this is gonna suck)

Low hanging fruit right now is going to be the tv box, so probably going to tackle that next.  Let me know your thoughts in the comments below! Would love to hear about everyone's experience with matter and thread in homeassistant.  (comments are using github discussions via API integration called giscus, pretty neat!)  
Cheers!  🍻 




