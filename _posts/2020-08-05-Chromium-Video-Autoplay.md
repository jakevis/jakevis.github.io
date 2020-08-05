---
title: "Chromium Video Autoplay"
categories:
  - Blog
tags:
  - How-To
  - Linux
---

I use [Dakboard] for my personal status display at home, and at work. Its a great tool, lots of capability. I run it from a RPI with Chromium running in kiosk mode. Issue is, on newer versions of Chromium videos wont autoplay. So if you patch your RPI (like you should), autoplay breaks... The fix, add this to your command line:

```bash
--autoplay-policy=no-user-gesture-required
```

My full command line on autostart for dakboard is:

```bash
chromium-browser --autoplay-policy=no-user-gesture-required --noerrdialogs --incognito --kiosk https://dakboard.com/display/uuid/XXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

And if you are running the dakboard provided image, this is part of the script located at: ```/home/pi/startup/chromium.sh```


[Dakboard]: https://dakboard.com



