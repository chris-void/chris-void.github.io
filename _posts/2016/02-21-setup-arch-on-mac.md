
---
layout: post
title: HOWTO setup arch on Mac Virtual box
tags:
 - howto
---

# reference
[Setting up an Arch Linux VM in VirtualBox](http://www.cs.columbia.edu/~jae/4118-LAST/arch-setup-2015-1.html)

Sadly there seems to be a problem.

### Solution:

I had the exact same issue. I think I made a typo (or maybe not) typing the line that says:

grub-mkconfig -o /boot/grub/grub.cfg

I think I might have put /grub.config at the end. But maybe not, who knows? I fixed it (and you can too) by booting from the live CD or live USB again. When you get the root command prompt enter the following commands. Remember to change sda1 and sda3 to the numbers corresponding to your OWN partitions as you generated them on first install.

If you have separate root and home partitions, then:

mount /dev/sda1 /mnt #sda1 is `root` partition
mount /dev/sda3 /mnt/home #sda3 is `home` partition

But if you have only a root partition then:

mount /dev/sda1 /mnt #sda1 is the 'root' partition

Then in all cases do these next:

arch-chroot /mnt
pacman -S os-prober
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

(IF IN A VIRTUAL MACHINE ALSO THIS LINE) pacman -S --needed virtualbox-guest-utils virtualbox-guest-modules

Then just:

exit
reboot

Make sure you remove the live CD or USB while rebooting so you don't boot the live environment again. You should find it boots now. It worked for me. :)

http://www.linuxveda.com/2015/02/18/arch-linux-tutorial-updated-feb-2015/12/
https://www.pckr.co.uk/arch-grub-mkconfig-lvmetad-failures-inside-chroot-install/

https://bbs.archlinux.org/viewtopic.php?id=168861
