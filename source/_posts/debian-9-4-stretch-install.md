title: debian 9.4(stretch) install
date: 2018-05-17 23:39:39
categories: linux
tags: 
---
## debian 9 install questions

Distributor ID | Desc | Third Realease
------------ | ------------- | ------------
Debian | CDebian GNU/Linux 9.4 (stretch)  | 9.4

Command | showinfo | other
------------ | ------------- | ------------
uname -r | 4.9.0-6-amd64  | none
cat /proc/version | Linux version 4.9.0-6-amd64  | (gcc 6.3.0 20170516(Debian 6.3.0-18+deb9u1)

## question
### /sys/kernel/debug/vgaswitcheroo/switch: No such file or directory
#### *answer*
#### 1. cat /sys/kernel/debug/vgaswitcheroo/switch address:https://bbs.archlinuxcn.org/viewtopic.php?id=2993,
#### 2. https://help.ubuntu.com/community/HybridGraphics,
#### 3. https://askubuntu.comquestions/648426/discrete-graphics-always-dynoff

## question
### ERROR radeon kernel modesetting for R600 or later requires firmware-amd-graphics.
#### *answer*
#### 1. add /etc/apt/sources.list deb http://ftp.us.debian.org/debian/ testing main non-free contrib 
#### 2. deb-src http://ftp.us.debian.org/debian/ testing main non-free contrib
#### 3. sudo apt-get update
#### 4. sudo apt-get install firmware-amd-graphics
## question
### grub rescue
### *answer*
#### 1. ls
#### 2. ls (hd0,x)/boot/grub or (hd1,x)/boot/grub
#### 3. set root=(hd0,5)
#### 4. set prefix=(hd0,5)/boot/grub
#### 5. set insmod /boot/grub
#### 6. set insmod /boot/grub
#### 7. sudo update-grub
#### 8. sudo grub-install /dev/sda


