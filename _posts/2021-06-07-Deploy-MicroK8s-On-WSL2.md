---
title: "Deploying Microk8s on WSL2"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Blog
tags:
  - Linux
  - Windows
  - WSL2
  - microk8s
  - kubernetes
---

I am running Windows 11, and have enabled WSL2 with Ubuntu via the standard GUI methods (install Ubuntu from the Microsoft Store). However, all credit goes to https://wsl.dev/wsl2-microk8s/ for the technicals here.

* TOC
{:toc}

## Step 1: Install Fonts, and set as default for WSL

Seems stupid.. but yes, for things to look ok and everything to render install these:

https://github.com/microsoft/cascadia-code/releases/download/v1911.21/CascadiaMonoPL.ttf
https://github.com/microsoft/cascadia-code/releases/download/v1911.21/CascadiaPL.ttf

Once downloaded, double click, install.

Right click on your WSL window, select properties, fonts and set it to Cascadia Mono PL.

![Fonts](../../assets/images/2021-06-07-Deploy-MicroK8s-On-WSL2/1.png)

## Step 1: Enable SystemD

Install required software (a different user is not required)

```bash
apt install -yqq fontconfig daemonize
```

### Create the wsl.conf file
```sudo vi /etc/wsl.conf``` and add this content. Make sure you change the default user to your username (```jake``` for me). Also run ```id``` first to make sure ```1000``` is the correct uid/gid. Exit vi with ```wq```

```
[automount]
enabled = true
options = "metadata,uid=1000,gid=1000,umask=22,fmask=11,case=off"
mountFsTab = true
crossDistro = true

[network]
generateHosts = false
generateResolvConf = true

[interop]
enabled = true
appendWindowsPath = true

[user]
default = jake
```
### Create the SystemD startup script

``` vi /etc/profile.d/00-wsl2-systemd.sh```
And add:
```
SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')

if [ -z "$SYSTEMD_PID" ]; then
   sudo /usr/bin/daemonize /usr/bin/unshare --fork --pid --mount-proc /lib/systemd/systemd --system-unit=basic.target
   SYSTEMD_PID=$(ps -ef | grep '/lib/systemd/systemd --system-unit=basic.target$' | grep -v unshare | awk '{print $2}')
fi

if [ -n "$SYSTEMD_PID" ] && [ "$SYSTEMD_PID" != "1" ]; then
    exec sudo /usr/bin/nsenter -t $SYSTEMD_PID -a su - $LOGNAME
fi
```
### Optional: set NOPASSWD for sudo 

```bash
sudo vi /etc/sudoers
```
Edit line:
```%sudo   ALL=(ALL:ALL) :ALL``` to read: ```%sudo   ALL=(ALL:ALL) NOPASSWD:ALL``` (and exit vi with ```!wq```)

## Setup forwarding

```bash
echo 'net.ipv4.conf.all.route_localnet = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf
```

## Restart WSL
From an elevated Powershell terminal
```powershell
wsl --shutdown
```
That should close your linux window..

## Install Microk8s

You should now be able to execute snap

```bash
snap list
```

check versions:
```bash
snap info microk8s
```

and install latest:
```bash
sudo snap install microk8s --classic
```

After its complete - check its all running:
```bash
sudo microk8s.status
sudo microk8s.kubectl version
sudo microk8s.kubectl cluster-info
```