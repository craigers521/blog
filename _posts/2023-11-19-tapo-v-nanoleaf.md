---
title: Tapo vs Nanoleaf
header:
  teaser: /assets/images/post4/tapo_vs_nanoleaf.jpg
excerpt: Testing two different matter enabled smart light vendors
description: Compare onboarding and performance of tapo and nanoleaf matter thread smart home assistant
tags: [homeassistant, matter, tapo, nanoleaf]
gallery:
  - url: /assets/images/post4/nanoleaf_onboard.png
    image_path: /assets/images/post4/nanoleaf_onboard.png
    alt: "added nanoleaf device"
    title: "added nanoleaf device"
  - url: /assets/images/post4/tapo_onboard.png
    image_path: /assets/images/post4/tapo_onboard.png
    alt: "added tapo device"
    title: "added tapo device"
  - url: /assets/images/post4/rgb_light.png
    image_path: /assets/images/post4/rgb_light.png
    alt: "light control panel"
    title: "light control panel"
---

#### Let's add some more matter devices!

So far so good with the edison bulbs, but in the arena of "normal" bulbs we have a lot more vendors to choose from (search amazon for matter enabled smart light and you will see what I mean).  

One of the most prominent players in the matter lights space is nanoleaf, and they have quite a few form factors to choose from, so I definitely want to check those guys out.  TP-link has been playing in the IoT space for a while now with their kasa brand, I found another brand called "Tapo" that has a few different matter enabled products (like a smart plug I want to check out), so I snatched up one of their lights as well to kick the tires on it.  

![tapo_v_nanoleaf]({{ site.url }}{{ site.baseurl }}/assets/images/post4/tapo_vs_nanoleaf.jpg)  

#### Onboarding Experience

So I screwed in the nanoleaf bulb and the process was identical to onboarding the other lights: Prompt on my phone, add the device, link the fabric to HA, bobs your uncle.  The one nice addition of the nanoleaf bulb compared to the linkind edison's is now i have a little sticker insert with the QR code to scan, no trying to get a weird angle and find the QR code, so its even easier.  

The tapo bulb on the other hand I had to fight with a bit.  It wasn't auto-sensing the device with my phone, so I tried to manually add via home assistant.  Here the onboarding is still a bit clunky.  It seemed I had added the device correctly. But then I got dropped back off at the add device screen in the HA app, and when I backed out to my device list it was no where to be found.  I tried unsuccessfully to re-add it a few times but it was kind of stuck.  I was able to factory reset the device by power cycling the bulb rhythmically a few times, which seems to be the de facto method for most smart bulbs, glad to see it still works.  Eventually I was able to manually add the device to google and then link to HA, but yeah way less smooth onboarding.  

I'm not sure if it was quirky behavior with the Tapo or with HA or if it was just a complete fluke.  Either way based on the experience, their market presence, and the overall vendor agnostic design of matter, I think I'm going to be sticking with nanoleaf products for the time being, and i'll be outfitting the rest of my house with those.   

On the plus side, both products included QR code inserts in the packaging for easy scanning, and both entities show up with full brightness, temperature, and RGB controls in home assistant, which is nice.  Oh and the onboarding process even includes some detection of the device manufacturer and type which is kind of fun and cool.  Didn't see that with the Eve or Linkind. 

{% include gallery %}

#### Next Time

Let's get the rest of the house retrofitted and build some fun automations.  

Cheers! 🍻