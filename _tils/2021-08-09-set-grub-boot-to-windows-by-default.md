---
title: "Set Grub boot to Windows by default"
---
Find `menuentry` in `/boot/grub/grub.cfg`, like this: `menuentry 'Windows Boot Manager (on /dev/...)'`
Copy the text in single quotes and paste into the line of `/etc/default/grub`
```
GRUB_DEFAULT="paste here"
```
Then
```sh
sudo update-grub
```
