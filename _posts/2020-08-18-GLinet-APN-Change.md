---
title: "GL.Inet APN Change "
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Blog
tags:
  - Routing
  - Linux
---

I use a [GL-X750] for internet access while I travel, and recently wanted to try out verizon to see if they have better coverage, and I wanted to do it without a major commitment of funds, so I went and got a Visible SIM. Issue is - it seems that the [GL-X750] does not recognize this SIM, or know the APN.

This is quite a simple fix, log into your device and you want to click "Manual Setup" for the Modem.

You then want to select **/dev/ttyUSB3** and enter the appropriate APN. ```VSBLINTERNET``` for me.


![Login](../../assets/images/2020-08-18-GLinet-APN-Change/1.png)

Once you are done, apply, and you should connect and be good to go.

This worked for me, your milage may vary. Visible is designed for mobile phones, not hotspots/routers.

[GL-X750]: https://www.gl-inet.com/products/gl-x750/



