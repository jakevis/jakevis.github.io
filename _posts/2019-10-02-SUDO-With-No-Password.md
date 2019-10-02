---
title: "SUDO With No Password"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Linux
tags:
  - How-To
---

I use SSH Keys for everything... and generally dont know my password for a linux machine once built. Hence I need to set sudoers correctly.. This is how I do that (for reference).

```bash
vi /etc/sudoers
```

Then make the block look like:

```bash
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

(Your just adding ```NOPASSWD:``` right before the last ALL)
