---
layout: post
title: 'Install Arch Linux'
date: '2019-02-07 01:25:00'
categories: os
---

*<u>This tutorial assumes that you have a working knowledge of the linux system.</u>*

1) Download the Arch Linux ISO File**

First download the Arch .iso from [here](https://www.archlinux.org/). 

**2) Prepare the installation media**

Make a bootable USB drive:

```bash
# dd bs=4M if=/home/user/archlinux-2019.02.01-x86_64.iso of=/dev/sdc status=progress && sync
```

You should replace the *sdc* by the value corresponding to your usb key and the Arch Linux path corresponding to yours.

**3) The installation process**

Now boot the image and lets begin the install process.

Choose option one: **Boot Arch Linux (x86_64)**

At the command prompt run:

```bash
# nano install.txt
```

This will show you a document listing the install process.

**4) Verify the boot mode**

It is possible to have the boot configured for the **MBR** or **EFI** mode. Most new machines support **EFI** mode. We need to check this anyways. You can check this in the **BIOS** but we will verify it here. Use the following command:

```bash
# ls /sys/firmware/efi/efivars
```

If you get no result it means that you don't have an existing **EFI** folder. If you get a result then you have **EFI** mode.

**5) Configure the network**

Check to see if you are connected to the internet:

```bash
# ping google.com -c 3
```

If you get a TTL response in milliseconds then you have an active network connection. If not then follow theses steps:

```bash
# ip link
```

This will list all of your active network devices. Find the one that you use, usually listed at ```enp1s0``` for cable connections and ```wlo1``` for wi-fi connections.

```bash
# wifi-menu -o wlo1
```

This will bring up the wi-fi configuration menu. After connecting try pinging google again.

```bash
# ping google.com -c 3
```

**6) Format the disk**

You can check the disk with the command below:

```bash
# fdisk -l
```

There are several options that can be used to partition and format the disk, we will be using ```parted```.

First wipe the existing data from the disk:

```bash
# sgdisk --zap-all /dev/sdX
```

Now format the disk with ```parted```:

```bash
# parted /dev/sda
```

Label the filesystem:

```bash
# mklabel msdos
```

Lets make the boot partition:

```bash
# mkpart primary ext4 1MiB 512MiB
```

```bash
# set 1 boot on
```

And now make the swap partition, I will be using 16GiB but you can adjust this to your liking:

```bash
# mkpart primary linux-swap 512MiB 17GiB
```

Now make the primary partition using the remaining space on your disk:

```bash
# mkpart primary ext4 17GiB 100%
```

Lets check to see if everything wrote correctly:

```bash
# print
```

And quit ```parted```:

```bash
# quit
```

Now its time to write the filesystem, we will be using ```ext4```:

```bash
# mkfs.ext4 /dev/sda1
```

```bash
# mkswap /dev/sda2
```

```bash
# swapon /dev/sda2
```

```bash
# mkfs.ext4 /dev/sda3
```

```bash
# mkdir -p /mnt/boot
```

```bash
# mount /dev/sda1 /mnt/boot
```

```bash
# mount /dev/sda3 /mnt
```

**7) Select the mirror**

Choose the closest mirror for the fastest download speed. You can edit the mirror list by editing the file `/etc/pacman.d/mirrorlist` to choose the mirror you want to use:

```bash
# nano /etc/pacman.d/mirrorlist
```

If you are using Wi-Fi then you will need to install the following packages before you reboot otherwise you will loose connectivity:

```bash
# pacman -S iw wpa_supplicant dialog wpa-actiond
```

This will install the Wi-Fi menu, the software that allows you to automatically connect to known networks.

**8) Install the Arch Linux base system**

Now that you have chosen a mirror lets install the base system packages:

```bash
# pacstrap /mnt base base-devel
```

**9) Generate the fstab file**

The ```fstab``` file is used to control what file systems are mounted automatically when the system boots. Lets check our current ```fstab``` file:

```bash
# cat /mnt/etc/fstab
```

You can see the there is no entry in the file. We can generate this file with the following command:

```bash
# genfstab -U /mnt >> /mnt/etc/fstab
```

**10) Change to the root system**

```bash
# arch-chroot /mnt
```

**11) Change the timezone / set the clock**

I will be using Vancouver as an example, you will need to change this for your location.

```bash
# ln -sf /usr/share/zoneinfo/America/Vancouver /etc/localtime
```

It is important to set the hardware clock to UTC:

```bash
# hwclock --systohc
```

**12) Edit the ```locale``` file / set your language**

Open the ```locale.gen``` file and uncomment your language:

```bash
# nano /etc/locale.gen
```

Now lets generate the file:

```bash
# locale-gen
```

Set your language:

```bash
# echo LANG=en_US.UTF-8 > /etc/locale.conf
```

```bash
# export LANG=en_US.UTF-8
```

**12) Set ```hostname```**

Now lets set the system hostname:

```bash
# nano /etc/hostname
```

The file should be blank so go ahead and type in the name that you want to use for your system, this can always be changed later by editing the ```hostname``` file.

**13) Set the ```hosts``` file**

```bash
# nano /etc/hosts
```

```bash
127.0.0.1		localhost
::1				localhost
127.0.1.1		systemname.localdomain	systemname
```

**14) Install GRUB**

We will also install ```os-prober``` and ```bash-completion```

```bash
# pacman -S grub os-prober bash-completion
```

```bash
# grub-install --recheck --target=i386-pc /dev/sda
```

```bash
# grub-mkconfig -o /boot/grub/grub.cfg
```

**15) Set the system password**

```bash
# passwd
```

**16) Add a user**

```bash
# useradd -m -G wheel,users -s /bin/bash username
```

```bash
# passwd username
```

Now enter the following command:

```bash
# pacman -Syu
```

**17) Set the default DNS**

We will be using ```Google DNS```, however you can use whatever you like.

```bash
# nano /etc/resolv.conf
```

Enter the following at the end of the file and save:

```bash
8.8.8.8
8.8.4.4
```

**18) Sudo**

```bash
# EDITOR=nano visudo
```

Find the line that says:

```% wheel ALL=(ALL) ALL ```

and uncomment it.

**19) Install Network packages and tools**

```bash
# pacman -S iw dialog networkmanager network-manager-applet gnome-keyring wireless_tools wpa_supplicant wpa_actiond
```

**20) Set network connection**

Find your network card

```bash
# ip link
```

Now open a file with your network card id:

```bash
# nano /etc/systemd/network/enp2s0.network
```

Add the following to the file:

```bash
[Match]
name=en*

[Network]
DHCP=yes
```

Now enable the network ```daemon```:

```bash
# systemctl enable systemd-networkd
```

**21) Edit pacman.conf**

```bash
# nano /etc/pacman.conf
```

Find and uncomment the line that says```multilib```

```bash
# sudo pacman -Syy
```

We are finished installing all the necessary packages, now it's time to unmount the filesystem and reboot. You will land at a command prompt if everything went correctly.

```bash
# exit
# umount -R /mnt
# reboot
```

**22) Install Xorg**

Now we need to install a ```GUI```, so let's begin with installing the necessary ```xorg``` packages.

```bash
# sudo pacman -S xorg xorg-server xorg-apps xorg-server-devel xorg-xrandr xorg-xinit xorg-twm xorg-xclock xterm

# startx
```

You will be brought to an ```xorg``` screen with several terminal windows, you can continue installing using the terminals.

**23) Install login manager**

We will be using ```lxdm``` which is a lightweight login manager.

```bash
# sudo pacman -S lxdm
```

**24) Install GUI**

There are several different ```GUI's``` that we can use, for example ```xfce4``` is one of the most popular and it's fairly user friendly so that's what we will be using. If you want to use something else like ```Gnome```, ```KDE```, or ```MATE``` then you can ```Google``` any of those for install instructions.

```bash
# sudo pacman -S xfce4 xfce4-goodies xfce4-devel
```

**25) Enable services**

You must enable ```lxdm``` using the following command:

```bash
# systemctl enable lxdm.service
```

And start ```lxdm```:

DO THIS LAST!!! (At the end of installation)

```bash
# systemctl start lxdm.service
```

Enable and start ```NetworkManager```:

```bash
# systemctl enable NetworkManager.service
# systemctl start NetworkManager.service
```

Enable and start ```DHCP```:

```bash
# systemctl enable dhcpcd.service
# systemctl start dhcpcd.service
```

Start network:

```bash
# systemctl start systemd-networkd
```

**26) Install various tools**

```bash
# pacman -S file-roller unrar p7zip pulseaudio alsa-tools alacarte-xfce lxrandr menu-libre
```

You are done! Now just run the following:

```bash
# systemctl start lxdm.service
```

If everything went correctly you will arrive at the ```lxdm``` login screen, choose ```xfce``` and then login.