---
title: "VLAN Interface on Windows Server"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Windows
tags:
  - Networking
  - How-To
---

Every needed to add a seondary interface on a Windows server? This will allow you to do it.
Make sure you have hyper-v installed - and then add the interfaces. This will add an interface with tagged VLAN 1, called ClientNet.

```powershell
Add-VMNetworkAdapter -ManagementOS -Name ClientNet
Set-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName ClientNet -Access -VlanId 1
```

If you need to stop your machine from registering this IP in DNS (with AD), machine sure you open up the adaptor, got to TCP/IP settings, advanced, DNS and uncheck the register with DNS.
