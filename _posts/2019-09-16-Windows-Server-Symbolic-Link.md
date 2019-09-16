
---
title: "Windows Server Symbolic Link"
#date: 2019-08-14T15:30:00-06:00
#last_modified_at: 2019-08-14T15:30:00-06:00
categories:
  - Windows
tags:
  - Fileshare
---

Need to mount a network drive/share "within" a windows filesystem? Run this command from an elevated command prompt.

```
mklink /d "c:\data\network docs" "\\server\shareddata\"
```