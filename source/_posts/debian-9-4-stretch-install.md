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
question
/sys/kernel/debug/vgaswitcheroo/switch: No such file or directory
cat /sys/kernel/debug/vgaswitcheroo/switch address:https://bbs.archlinuxcn.org/viewtopic.php?id=2993 ,https://help.ubuntu.com/community/HybridGraphics,https://askubuntu.com/questions/648426/discrete-graphics-always-dynoff

ERROR radeon kernel modesetting for R600 or later requires firmware-amd-graphics.
add /etc/apt/sources.list deb http://ftp.us.debian.org/debian/ testing main non-free contrib deb-src http://ftp.us.debian.org/debian/ testing main non-free contrib
sudo apt-get update
sudo apt-get install firmware-amd-graphics
grub rescue
ls
2.ls (hd0,x)/boot/grub or (hd1,x)/boot/grub
set root=(hd0,5)
set prefix=(hd0,5)/boot/grub
set insmod /boot/grub
set insmod /boot/grub
sudo update-grub
sudo grub-install /dev/sda


