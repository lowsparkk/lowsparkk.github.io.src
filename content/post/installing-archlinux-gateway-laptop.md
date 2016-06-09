---
author: Low Sparkk
date: 2016-06-09T12:12:34-07:00
description: Arch Linux Installation Instructions
tags:
- Arch Linux
title: Installing Arch Linux on my Gateway Laptop
topics:
- Linux
type: post
---

```
parted /dev/sda
mklabel msdos
mkpart primary ext4 1 MiB 290 GiB
set 1 boot on
mkpart primary linux-swap 290 GiB 298 GiB
quit

mkfs.ext4 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2

mount /dev/sda1 /mnt
vim /etc/pacmand.d/mirrorlist
pacstrap -i /mnt base base-devel
genfstab -U /mnt > /mnt/etc/fstab
arch-chroot /mnt /bin/bash

nano /etc/locale.gen ==> LANG=en_US.UTF-8
locale-gen

echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8

tzselect ==> America/Los_Angeles
ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime

hwclock --systohc --utc

pacman -S grub os-prober
grub-install --recheck --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

echo sienna > /etc/hostname
passwd
exit
umount -R /mnt
shutdown
```

Remove USB stick, reboot, login as root. Plugin ethernet cable

```
systemctl start dhcpcd@enp6s0.service
pacman -S iw wpa_supplicant dialog
systemctl stop dhcpcd@enp6s0.service
```

Pull out ethernet cable
```
wifi-menu -o wlp2s0
ping -c3 www.google.com

pacman -Syu
pacman -S zsh

useradd -m -G wheel,users -s /usr/bin/zsh rkk
passwd rkk
pacman -S sudo
visudo

# uncomment the following line
% wheel ALL=(ALL) ALL

pacman -Sy
pacman -S xorg-server xorg-server-utils # choose mesa-libgl
pacman -S xf86-input-synaptics
pacman -S xfce4 xfce4-goodies lxdm
systemctl enable lxdm.service
systemctl start lxdm.service
```

For the XPS desktop, choose the following ```UEFI-GPT``` partition instructions

```
mkpart ESP fat32 1 MiB 513 MiB
mkpart primary ext4 513 MiB 871 GiB
mkpart primary linux-swap 871 GiB 100% # approximately 64G of swap space
```
