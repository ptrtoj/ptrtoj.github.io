---
title:  "젠투 리눅스 설치 가이드"
date:   2017-11-05
url: 'gentoo'
---

## Preface

**젠투 리눅스 설치 가이드는 문서 방향성을 전면 수정합니다.**\
설치에 익숙한 분들이 핸드북을 참조하기 번거로울 때 활용할 수 있도록 수정합니다.\
젠투 리눅스가 처음이신 분들이나 리눅스가 익숙하지 않으신 분들은 [핸드북](https://wiki.gentoo.org/wiki/Handbook:AMD64)을 참고하시기 바랍니다.

23년 12월부로 젠투 측에서 바이너리 패키지를 공식 지원합니다.\
바이너리 패키지 설치 방법을 위해 [별도 포스트]({{<ref "gentoo-binary">}})를 마련했습니다.

## Prerequisite

```
본인 Git에서 필요한 파일을 다운받아야 할 때,

> Github
$ wget https://github.com/YOUR_USER_ID/YOUR_REPOSITORY_NAME/raw/main/FILE_DIRECTORY/SOME_FILE

> Gitlab
$ wget https://gitlab.com/YOUR_USER_ID/YOUR_REPOSITORY_NAME/-/raw/master/FILE_DIRECTORY/SOME_FILE
```

## Live USB

[젠투 위키의 미러](https://www.gentoo.org/downloads/mirrors/)

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

UEFI 부팅 확인

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

다른 배포판 환경에서 설치하는 경우, `/mnt/gentoo` 디렉터리가 없으므로,

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

아래 명령어중 `--make-rslave`옵션은 `systemd` 설치하실 분만 진행합니다

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
'en_US.UTF-8 UTF-8' 주석 제거
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
// 정품
# emerge --ask sys-kernel/gentoo-sources pciutils sys-kernel/linux-firmware

// 진품
# git clone https://github.com/torvalds/linux.git
# wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.14.1.tar.xz

// 유사품 1 - 바이너리 [2020-09-15: Gentoo 업데이트]
# emerge -a sys-kernel/gentoo-sources-bin
# emerge -a sys-kernel/gentoo-kernel-bin

// 유사품 2 - 컴파일
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

아래엔 본인 드라이버의 이름을 넣어야 합니다.

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
