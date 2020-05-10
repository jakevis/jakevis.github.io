---
title: "GL.Inet TTL Change "
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Blog
tags:
  - Routing
  - Linux
---

I use a [GL-X750] for internet access while I travel, and while I have an unlimited dataplan is seems some carriers in the us play funny buggers and "de priortise" data from secondary devices. I have found this work around to function for me.... we are changing the TTL of all connected devices, so its harder for the carrier to work out what device is what.

* TOC
{:toc}

## Step 1: Login to your device


![Login](../../assets/images/2020-05-10-GLinet-TTL-Change/1.png)


## Step 2: More Settings -> Advanced

![Advanced Settings](../../assets/images/2020-05-10-GLinet-TTL-Change/2.png)

## Step 3: Login again

Yes - the username will be root, the password will be your admin password. Your path will be ```https://x.x.x.x/cgi-bin/luci```

![Login 2](../../assets/images/2020-05-10-GLinet-TTL-Change/3.png)

## Step 4: Network -> Firewall

![Firewall](../../assets/images/2020-05-10-GLinet-TTL-Change/4.png)

## Step 5: Custom Rules -> Add entry

You want to click on custom rules, then go right to the bottom. Here add this line:

```
#Change TTL
iptables -t mangle -I POSTROUTING 1 -j TTL --ttl-set 65
```

![Firewall](../../assets/images/2020-05-10-GLinet-TTL-Change/5.png)

## Step 6: Restart

Once done, click on the restart firewall, and you should be set after a reboot.  This worked for me, your milage may vary.

[GL-X750]: https://www.gl-inet.com/products/gl-x750/



