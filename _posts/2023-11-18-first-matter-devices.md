---
title: Onboarding my first Matter devices
header:
  teaser: /assets/images/post3/enable_multiprotocol.png
excerpt: Set up the homeassistant yellow for thread and matter and onboard devices
description: Set up the homeassistant yellow for thread and matter and onboard devices
tags: [homeassistant, matter, thread]
gallery:
  - url: /assets/images/post3/matter_onboard.png
    image_path: /assets/images/post3/matter_onboard.png
    alt: "onboard matter device"
    title: "onboard matter device"
  - url: /assets/images/post3/multi-admin2.png
    image_path: /assets/images/post3/multi-admin2.png
    alt: "link matter apps"
    title: "link matter apps"
  - url: /assets/images/post3/multi-admin.png
    image_path: /assets/images/post3/multi-admin.png
    alt: "add to home assistant"
    title: "add to home assistant"
---
### Setting up the HA yellow
The first thing to do is go to **Settings -> System -> Hardware** and configure to enable multiprotocol support.  This is going to use the built-in zigbee radio to also use `thread` since they operate on the same wireless frequencies.  Again `thread` is pretty much zigbee + IPV6.  With that enabled you should see a new add-on called "Silicon Labs Multiprotocol" in your add-ons.  

![enable-multiprotocol]({{ site.url }}{{ site.baseurl }}/assets/images/post3/enable_multiprotocol.png)  

Now we can go to integrations and add the `Matter` integration. You should see a few things in your integrations now.  The matter integration as well as the `thread` and openthread border router.  


![thread-integration]({{ site.url }}{{ site.baseurl }}/assets/images/post3/thread_integration.png)  

### Onboarding First Devices  
We should have everything we need now to start talking matter over thread.  For my first device I have selected the Eve motion sensor, so I can use both motion and illuminance to build an automated way to turn the lights on if I enter a dimly lit room.  As soon as I placed the battery into the sensor I almost immediately saw a pop up on my android phone.  I assumed this was google trying to be helpful, but I'm in charge here not google, I'm gonna manually onboard this myself via the home assistant companion app.  I fussed around with it a bit by going to devices and tried to manually add a device, but I backed out of the config because I was still researching to make sure I was doing this right.  I glanced down at my phone and again the pop up was there so I thought ok lets try this thing and see what happens.  Google did help invent this tech after all and onboarding is supposed to be easy so lets check it out.  

![device_detected]({{ site.url }}{{ site.baseurl }}/assets/images/post3/device_detected.png){: .post-image2}   

Low and behold after acknowledging the prompt I was able to select my homeassistant as a matter controller and was able to add the device.  Typical engineer I was trying to find the more complicated way to do something and not the consumer friendly way.  I officially had my first matter device complete with 3 entities: occupancy, illuminance and battery! 

![generic_device]({{ site.url }}{{ site.baseurl }}/assets/images/post3/generic_device.png){: .post-image2}   

![eve_motion]({{ site.url }}{{ site.baseurl }}/assets/images/post3/eve_motion.png)  

### Multi-admin fabric  
Ok cool, now whats a motion sensor without some lights to control.  Now in my kitchen and dining room all of the light ballasts are either open or with clear glass coverings, so the waifu really prefers the aesthetic of an edison style bulb.  These tend to be a little trickier to find that most smart bulbs, let alone newer matter enabled bulbs, but I was able to source some off of amazon from a company called linkind.  

For these lights we have grown accustom to using voice control to make adjustments.  I do have it on my to-do list as part of my "open source everything" project to migrate to an open source voice assistant like mycroft or rhasspy, and I have seen some great voice support recently for home assistant, but I'm not quite ready to move away from google home yet for voice support.  I had read about multi-admin/multi-fabric support for matter, where multiple matter controllers can control the same device, so lets give that a try.  

![multi_admin_infographic]({{ site.url }}{{ site.baseurl }}/assets/images/post3/multi-admin-graphic.png)  

I screw in the first bulb, get the pop up on my phone, this time I select google home as my matter controller and onboard the device just like any other google device, which is great because still no app bloat, no need to get a vendor app and link the integration to google, so I am ok with this. *(NB these bulbs had the QR code directly on them and not on a paper insert, if you can't read the QR with your camera with the bulb installed, you can take the bulb out, scan the QR, then screw it back in and finish onboarding)*   

Now the neat part: in google home go to the device settings and you can see an option to link matter enabled service.  Clicking into that you can select home assistant and it will then add the device to home assistant as well! We are multi-admin matter now baby! 

{% include gallery caption="Multi-admin device matter onboarding" %}  

**One warning** To make sure I don't lose track over which bulb is which, I go into home assistant and give the device a friendly name as well as give the entity a friendly name.  One time when I renamed I followed the prompt to also rename the entities, and worse yet I accidentally also edited the entity_id (not the friendly name).  I somehow ended up with two entities one that was associated with the device and broken, and one entity that was orphaned. I had to fight home assistant to get it cleaned up, so I recommend A) Only sticking with editing friendly names and not entity id's, and B) not renaming entity id's at all, just stick with the friendly name and let HA pick what it wants for the ids.
{: .notice--danger}  

![rename_warning]({{ site.url }}{{ site.baseurl }}/assets/images/post3/rename_warning.png){: .post-image2}  

**Sweet!** Now I have full voice control over my lights with google home **AND** all the automation and control with home assistant **and** I didn't need any vendor proprietary apps to do it!  Once the lights were functional I promptly uninstalled the Solana Bulbrite app from my phone and unlinked the integration from google.  See ya!  In my next post i'll be testing onboarding a few different light vendors.  

Cheers! 🍻