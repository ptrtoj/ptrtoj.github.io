---
title:  "Gentoo Linux Installation Guide"
date:   2017-11-05
url: 'gentoo'
---

## Preface

**This Gentoo Linux installation guide has completely changed direction.**\
It's now meant for people already familiar with the installation, for when consulting the Handbook is a hassle.\
If you're new to Gentoo Linux or not yet comfortable with Linux, please refer to the [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64).

As of December 2023, Gentoo officially supports binary packages.\
I've written a [separate post]({{<ref "gentoo-binary">}}) on how to install binary packages.

## Prerequisite

```
When you need to download a file from your own Git repository,

> Github
$ wget https://github.com/YOUR_USER_ID/YOUR_REPOSITORY_NAME/raw/main/FILE_DIRECTORY/SOME_FILE

> Gitlab
$ wget https://gitlab.com/YOUR_USER_ID/YOUR_REPOSITORY_NAME/-/raw/master/FILE_DIRECTORY/SOME_FILE
```

## Live USB

[Gentoo mirrors](https://www.gentoo.org/downloads/mirrors/)

```console
# wipefs --all /dev/sdb
# dd if=./Downloads/install-amd64-minimal-20211231T235901Z.iso of=/dev/sdb status=progress && sync
```

## Base

### Ping

```console
# ping
```

### Partition

Check for UEFI boot

```console
# ls /sys/firmware/efi
```

```console
# wipefs --all /dev/sda
# parted /dev/sda
# cfdisk /dev/sda
```

### Format

`/boot` partition

```console
# mkfs.vfat -F 32 /dev/sdaN
```

`swap` partition

```console
# mkswap /dev/sdaN
# swapon /dev/sdaN
```

`/` partition

```console
# mkfs.ext4 -j /dev/sdaN
```

foramt as `xfs` or `btrfs`

```console
# mkfs.xfs /dev/sdaN
# mkfs.btrfs /dev/sdaN
```

### Mount

If you're installing from another distribution, the `/mnt/gentoo` directory doesn't exist, so

```console
# mkdir -pv /mnt/gentoo
```

```console
# mount -v -t ext4 /dev/sda3 /mnt/gentoo

# mkdir -pv /mnt/gentoo/boot
# mount -v -t vfat /dev/sda1 /mnt/gentoo/boot
```

### TIME

```console
# ntpd -q -g
# hwclock --systohc --utc
```

### Stage 3 Tarball

```console
# cd /mnt/gentoo
# links https://www.gentoo.org/downloads/mirrors/
# tar xpf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner
```

### Edit make.conf

```console
# nano -w /mnt/gentoo/etc/portage/make.conf
```

### Mirror

```console
# mirroselect -i -o >> /mnt/gentoo/etc/portage/make.conf
```

### Copy repos.conf

```console
# mkdir --parents /mnt/gentoo/etc/portage/repos.conf
# cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
```

### Copy resolv.conf

```console
# cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
```

### Mount `/proc`, `/sys`, `/dev`

Only run the commands below with the `--make-rslave` option if you are installing `systemd`

```console
# mount --types proc /proc /mnt/gentoo/proc
# mount --rbind /sys /mnt/gentoo/sys
# mount --make-rslave /mnt/gentoo/sys # <--
# mount --rbind /dev /mnt/gentoo/dev
# mount --make-rslave /mnt/gentoo/dev # <--
# mount --bind /run /mnt/gentoo/run
```

### Chroot

```console
# chroot /mnt/gentoo /bin/bash
# source /etc/profile
# export PS1="(chroot) $PS1"
```

### Portage

```console
# merge-webrsync

# emerge -avuDN @world
# emerge -av gentoolkit eix genlop
```

### News

```console
# eselect news list
# eselect news read N
```

### Profile

```console
# eselect profile list
# eselect profile set N
```

### Timezone

```console
# ls /usr/share/zoneinfo
# echo "Asia/Seoul" > /etc/timezone
# emerge --config sys-libs/timezone-data
```

### Locale

```console
# nano -w /etc/locale.gen
```

```file
Uncomment 'en_US.UTF-8 UTF-8'
```

```console
# locale-gen
# eselect locale list
# eselect locale set N
```

### Update env

```console
# env-update && source /etc/profile && export PS1="(chroot) $PS1"
```

### Kernel

```console
// The genuine article
# emerge --ask sys-kernel/gentoo-sources pciutils sys-kernel/linux-firmware

// The real deal
# git clone https://github.com/torvalds/linux.git
# wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.14.1.tar.xz

// Look-alike 1 - binary [2020-09-15: Gentoo update]
# emerge -a sys-kernel/gentoo-sources-bin
# emerge -a sys-kernel/gentoo-kernel-bin

// Look-alike 2 - compiled
# emerge -a sys-kernel/gentoo-kernel
# emerge -a sys-kernel/vanilla-sources
# emerge -a sys-kernel/git-sources
```

```console
# cd /usr/src/linux

# make mrproper
# make defconfig
# make menuconfig

# make -jN
# make modules_install
# make install
```

When updating the kernel

```console
# cp /usr/src/linux/.config /usr/src/linux/NEW_KERNEL
# make olddefconfig
```

If you need initramfs

```console
# genkernel --install --kernel-config=/usr/src/linux/.config initramfs
```

```
Microcode (TIP: Enable 'initramfs' USE Flag)
ex) add 'sys-firmware/intel-microcode initramfs'
    inside '/etc/portage/package.use/package.use' file
```

```console
# emerge -av sys-firmware/intel-microcode
```

### Edit fstab

```console
# blkid >> /etc/fstab
# nano -w /etc/fstab
```

```file
# Inside file '/etc/fstab'
/dev/sda1    /boot    vfat    defaults,noatime    0 2
/dev/sda2    none     swap    sw                  0 0
/dev/sda3    /        ext4    noatime             0 1
/dev/sda4    /home    ext4    noatime    0 1

# UUID -> remove quotation(")
# PARTUUID -> remove quotation(")
```

### Hostname

```console
# nano -w /etc/conf.d/hostname
# nano -w /etc/conf.d/net
# nano -w /etc/hosts
```

### Netifrc

```console
# emerge --ask --noreplace net-misc/netifrc
```

```console
# nano -w /etc/conf.d/net
```

```file
config_enp3s0="dhcp"
```

```console
# cd /etc/init.d
# ln -s net.lo net.enp3s0
```

Below, use the name of your own network interface.

```console
# rc-update add net.enp3s0 default
```

```
TIP
emerge -av ifplugd
```

### Passwd

```console
passwd
```

### Edit rc.conf

```console
nano -w /etc/rc.conf
```

```file
rc_logger="YES"
rc_log_path="/var/log/rc.log"
```

```console
nano -w /etc/conf.d/keymaps
nano -w /etc/conf.d/hwclock
```

### Install logroate, sysklogd, cronie

```console
# emerge --ask app-admin/sysklogd cronie mlocate
# rc-update add sysklogd default
# rc-update add cronie default

# emerge --ask net-misc/dhcpcd
```

or

```console
# emerge -av networkmanger
# rc-update add NetworkManager default
```

### GRUB:2

```console
# emerge --ask --verbose sys-boot/grub:2
# grub-install --target=x86_64-efi --efi-directory=/boot
# grub-mkconfig -o /boot/grub/grub.cfg
```

### Reboot

```console
# exit
# cd
# umount -lR /mnt/gentoo
# reboot
```

### Useradd

```console
# useradd -m -g users -G wheel,audio,video,portage -s /bin/bash USER_NAME
# passwd USER_NAME
```

### Remove stage3*.tar.xz

```console
# rm /stage3*.tar.xz
```

## XORG

```console
# eselect profile list
# eselect profile set N
# emerge -avuDN @world

# env-update && source /etc/profile

# emerge --ask x11-base/xorg-server
# env-update && source /etc/profile
```

## PLASMA

Settings for Plasma

```console
# eselect profile set N
# eselect profile list
# emerge -avuDN --with-bdeps=y @world
```

System Dependencies

```console
# emerge --noreplace -av elogind udev dbus polkit udisks
# rc-update add dbus default
# rc-update add elogind boot
```

Install Packages

```console
# emerge -av plasma-meta kde-aps-meta
```

SDDM

```console
# emerge --noreplace -av sddm
# usermod -a -G video sddm
# emerge --renoplace -av gui-libs/display-manager-init
```

Enable `SDDM`

```console
# vim /etc/conf.d/display-manager
```

```file
CHECKVT=7
DISPLAYMANAGER="sddm"
```

```console
# rc-update add display-manager default
```
