---
title: "Touch Window Display at Selfridges for Project Ocean"
tags:
  - .Net
  - NUI
---

At <a href="http://www.clusta.com" target="_blank">Clusta</a>, we've recently just completed a project for Selfridges that comprises of a massive touch screen window display, a website and a iPhone app to support the charity - ZSL. Across each of the channels the user is encouraged to donate a small amount of money to the good cause and in return is rewarded with their very own sustainable fish within the virtual ocean we've created. The user can donate in a number of ways but to be granted a fish they must either SMS or visit Just Giving via the <a href="http://www.selfridges.com/projectocean" target="_blank">Project Ocean website</a>

The main attraction is the virtual ocean that appears on the website and the at the front entrance at Selfridges in London and was even visited by HRH The Prince of Wales. When a user donates via SMS, they can witness their fish being "born" in real time and is labelled with their name. The fish that gets assigned to the user is random but some fish are more common than others. This indirect interactivity helps boost donations be encouraging users to donate more than once. Once their fish has been born the user can interact directly by touching the ocean scene.

If the user touches a fish a bubble appears with the donators name along with the species of fish they have been given. In addition, the user can interact by touching and attracting the fish or dragging erratically to scare the fish away. Some fish don't scare easily (like the shark), but others especially the ones that join and swim in shoals quickly disperse. After a user has donated, they are sent a deep link to their very own fish which they can visit online at anytime and then share to their friends via Facebook or Twitter. Depending on where the user has donated from (determined by the keyword they txt), they will be prioritised within the birthing queue. Therefore people standing on Oxford Street will not have to wait for web donations to be born first.

Due to the size of the window (around 4m x 3m) we had many challenges and problems to solve, also we wanted the display to work in full daylight even on a sunny day so therefore certain technology was inappropriate. At the planning stage we looked into using Microsoft Kinect to interact with display but due to it working with infrared it wasn't suited to daytime/outdoor use. In fact most gesture based or massively multi-touch technologies work using infrared or primitive motion detection. In the end we settled for a large capacitive touch foil and 4 super high powered projectors which look great and work really well.

## The Technology ##

The website was built on Microsoft ASP.NET MVC 3 (C# & Razor), Adobe Flash, jQuery and is hosted on Microsoft Azure with SQL Azure for the database. The website is fully load balanced and scales up and down automatically based on key performance metrics that are being monitored constantly. The native iPhone app syncs with the website database via an API to ensure the app can be used offline if required.

## Behind the Scenes ##

{% include video id="23686251" provider="vimeo" %}

## Useful Links ##

 - <a href="http://www.selfridges.com/projectocean" target="_blank">Project Ocean Website</a>
 - <a href="http://itunes.apple.com/us/app/selfridges-fish-guide/id434679360?mt=8" target="_blank">Project Ocean iPhone App</a>
 - <a href="http://www.facebook.com/SelfridgesProjectOcean" target="_blank">Project Ocean Facebook Page</a>
 - <a href="http://twitter.com/#!/SelfridgesOcean" target="_blank">Project Ocean Twitter Page</a> 
