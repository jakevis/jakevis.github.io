---
title: "Update on-Prem DNS for Hidden Master"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Blog
tags:
  - DNS
  - Windows
---

I use [CloudNS] for most of my DNS hosting; however I do also have a local DNS server as part of my AD infastructure. THere is a few cases where I want this zone also published publicly.

To support this you need to setup transfers.. and I was sick of entering it all by hand - so these are the commands needed. If your using [CloudNS] these IPs should work for you, if not, just swap them out.

``` powershell
dnscmd ad1 /zoneresetsecondaries woofy.io /securelist 109.201.133.111 209.58.140.85 54.36.26.145 185.206.180.104 185.136.96.66 185.136.97.66 185.136.98.66 185.136.99.66 185.206.180.193 /notifylist 109.201.133.111 209.58.140.85 54.36.26.145 185.206.180.104 185.136.96.66 185.136.97.66 185.136.98.66 185.136.99.66 185.206.180.193
```

I also dont have AD crease NS records for me - I do that manully - to stop windows from autocreasing them do this:

```powershell
Dnscmd /config /DisableNSRecordsAutoCreation 1
```


[CloudNS]: https://www.cloudns.net



