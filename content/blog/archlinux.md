---
title: "Manual Arch linux boot & install(No GUI)"
date: 2025-07-16
draft: false
---

Let us know few things about Arch Linux

## What is Arch linux?

It is a minimalist, highly customizable, and independent Linux distribution designed for users who prefer "do it yourself" approach.

**Rolling release:** instead of major version updates, Arch linux recieves continuous updates, ensuring always have to access to latest software versions.

**Package manager:** Arch uses its own package manager **pacman** for installing, removing and updating software.

**pacman -Sy:** Updates local package database with information from remote repositories.

Detailed:

**pacman:** Command-line package manager for Arch-linux.

**-S(or --sync):** used for installing,removing or upgrading packages.

**-y(or --refresh):** for refresh

It is not **pre-configured** and not **beginner friendly**. Need to configure everything themselves.

## Create a Bootable USB with UEFI + Gpt(based on system boot mode)

Okay, now lets dive into boot & installation process:

- Download arch linux from their official website: **https://archlinux.org/download/**
- load it to drive using rufus,(Select patition schema based on your system)

First part of work is done.

## Manual Arch Linux Boot & install

- Enter boot menu using function key based on laptop brand.In my case it F2 or F12.
- Select iso file

Connect to internet to install other packages.
For wired:

```
// just check ping
ping archlinux.org
```

For Wi-Fi:

```
//use this commands
iwctl
// inside iwctl
station wlan0 scan
station wlan0 get-networks
station wlan0 connect yourSSID
```

## Steps

**Partition the SSD Manually:**
Use fdisk or cfdisk

Create:

- EFI partitioin: 512M
- Root partition: rest of the disk

Next we need to format and mount the partition

**Formatting:**

```
mkfs.fat -F32 /dev/nvme0n1p1
mkfs.ext4 /dev/nvme0n1p1
```

**Mounting**

```
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot/efi

```

**Proceed with Arch install**

```
pacstrap /mnt base linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

**Then GRUB for UEFI**

```
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB

```

Finally setup is done.

Now install Desktop environment based on requirement.

## Popular desktop environment for Arch Linux

| Desktop Environment | Highlights                  | Package Name |
| ------------------- | --------------------------- | ------------ |
| GNOME               | Modern,Clean,touch-friendly | gnome        |
| KDE Plasma          | Highly customizable,elegant | plasma       |
| Xfce                | Lightweight,stable,classic  | xfce4        |
| LXQt                | lightweight,Qt-based        | lxqt         |

source: https://wiki.archlinux.org/title/Desktop_environment

I am using GNOME as my desktop environment.

Thanks for reading!
