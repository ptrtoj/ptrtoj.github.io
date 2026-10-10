---
title:  "Arch Linux Installation Guide"
date:   2017-11-07
url: 'arch'
---

## Preface

I often see people asking why they should use Arch Linux. I can't objectively and crisply explain it as 'these are the strengths, these are the weaknesses, but this is what makes it attractive' either (of course, the strengths and appeal I feel are clear to me, but they're very subjective).

What I do know clearly, and can share with you, is that '**installing Arch Linux isn't nearly as bewilderingly hard as it seems**'.

Ubuntu is usually introduced as an easy distribution to new users from the Windows or Mac camp who want to try Linux. That's perfectly understandable. The installation process is presented as a GUI, so it's very convenient. But I have my doubts about whether it's right to call clicking 'Next' without understanding what each step does 'easy'.

Arch Linux's installation is command-line based (CLI). You have to install everything by typing commands on the keyboard. But thanks to that process, you get a precise understanding of how Linux is built, and it puts you in a good position to know how your system is put together (a deeper understanding can be achieved through [Gentoo]({{< ref "gentoo" >}}), [CRUX](https://crux.nu/), or [LFS](https://www.linuxfromscratch.org/)).

When I first became interested in Arch Linux, the installation process was uncomfortable for me too. By 'uncomfortable', I mostly mean the lack of translated material. Having to install by relying **only on material in English** was hard. At the time, I keenly felt that 'good-quality material in Korean is needed'.

You might recommend the installation guide on the Korean Arch Linux wiki. It's very well organized, and the installation process introduced below is based on that page. However, I couldn't agree that the page alone is enough for beginners. My focus was on helping Arch Linux spread further in Korea, and in particular, on '**greatly helping beginners with the installation**'.

![Arch Linux logo](/arch-logo.png)

KISS: Keep it Simple, Stupid! This isn't just the orthodox philosophy of the Unix family; it's also a core philosophy pursued by the Arch Linux camp. Arch Linux pursues simplicity. What's interesting is how much complexity has to be condensed to achieve that simplicity. You may be taken aback when you look at the scroll bar on the right. You might ask, 'You claim to have written this for beginners, so why is it so detailed?'

The twist is that, as you read down this scroll bar, the number of packages you have to install yourself has actually been simplified to five. Of course, this means packages you install by typing them manually (*excluding dependencies*). Those five are `base`, `xorg-wayland`, `gnome`, `google-chrome`, and `ibus-hangul`.

As you can see, one of those five isn't actually required, but it's installed (`google-chrome`) to pass on knowledge that will be useful when you use Arch Linux (how to use the AUR).

Through this one page, readers can learn about the areas involved in **building a base system** (partitioning, formatting, the file system table, the boot process and the role of the bootloader), building a **desktop environment**, **learning the AUR** by installing a web browser, and feel how easy it is to use **Korean** on Arch Linux.

**Paradoxically, to achieve that simplicity, I had no choice but to include quite a lot of content.**

## **Before Installing**

**There is [only one](https://wiki.archlinux.org/index.php/Installation_guide) official Arch Linux installation guide.**

Every other guide, including this page, is edited for a **specific purpose** (user convenience/author convenience/teaching). So I recommend first understanding the content of the official wiki, then adjusting and applying it to your taste while reading this guide.

If you understand everything explained in this post, you may find installation easier than before with the installation helper program distributed since April 1, 2021 under the name [Arch Install](https://wiki.archlinux.org/title/Archinstall)!

## Preparing for Installation

Here's how to download the installation image file (`iso`).
(Reference: [KAIST mirror](http://ftp.kaist.ac.kr/ArchLinux/iso/latest/))

```
Go to the KAIST mirror > in the list > click archlinux-YYYY.MM.DD-x86_64.iso to download
```

### On Windows

Create it with [Rufus](https://rufus.ie/en/). I'll skip the details.

### On Linux

After mounting the USB drive on a Linux system, run the command below.

```console
# lsblk
```

> **Command explanation**\
> `lsblk`: **l**i**s**t **bl**oc**k** device → prints a list of block devices.

From the output, find the device name of your USB drive.

Usually, you can tell by its capacity.
It will show a name that varies by user, such as `sdb` or `sdc`.

Use the command below to format the USB drive and prepare it for writing the new iso.

In the example command's `/dev/sdb`, you must change the `sdb` part to match your environment.
Put in the name of your USB device that you found above.

> **The command below deletes every file on the USB drive.**\
> **Be sure to back up any important files, or files you can't afford to lose, before proceeding.**

```console
# sudo wipefs --all /dev/sdb
```

Use the command below to burn the iso file onto the prepared USB drive.

After `if`, enter `(iso download location)/archlinux-(date)-x86_64.iso` to match your environment.

If there's only one Arch Linux iso file in that path, you can type the beginning, like `archli`, and auto-complete with the `Tab` key.

However, before pressing `Enter`, **be sure to check that the auto-completed file name is what you want**.

In the command below, after `of`, enter the USB path you prepared above, **the one that matches your environment**.

```console
# sudo dd bs=4M if=~/Downloads/archlinux.iso of=/dev/sdb status=progress && sync
```

> **Command explanation**
>
> - `dd`: Uses a program called `dd`. (Back in the old UNIX days, it had the nickname '**D**isk **D**estroyer', meaning 'a program that makes your disk unusable if you use it wrong'.)
> - `bs` (**b**lock **s**ize): the block size is 4M (optional)
> - `if` (**i**nput **f**ile): the input file (here, the Arch Linux iso file)
> - `of` (**o**utput **f**ile): the path of the output device to write to

Now reboot.

While rebooting, use the BIOS/UEFI entry key for your motherboard.
For me, it's `F2`. It's usually `F2`, `F10`, `F11`, `Del`, or similar.

Using the BIOS/UEFI entry key, find and select the USB drive you just made among the boot media.

If the system boots normally from this USB drive, on the selection screens during and after the Linux boot, you can generally boot with whatever is selected by default (something including `Boot Arch Linux` or `x86_64`).

You may also be asked to choose a keymap; the default is often `US`, so just press `Enter`.

> **If the installation USB won't boot at all!**
>
> If you can't even get to the installation screen, it's most likely a BIOS/UEFI settings problem.\
> **Check that `secure boot` is set to `Disabled`.**\
> It could also be an error caused by `fast boot` or `CSM mode` (`LEGACY/BIOS MODE`).\
> **I recommend setting all of them to `Disabled`.**

## Installing the Base System

### Checking for UEFI

First, let's check whether you need to install with UEFI, and whether installing with UEFI is possible.

As I'll mention again in the partition planning part below,
the partitions differ depending on whether you booted with UEFI and plan to install with UEFI,
or not (if you booted with BIOS).

For UEFI, the recommended installation setup requires the disk type to be `GPT` and requires an `ESP partition`.

Since most ordinary users' boot environment is UEFI, this guide only covers partitions for a UEFI installation.

You can check with the command below.

```console
# ls /sys/firmware/efi
```

> **Command explanation**
>
> `ls`: **l**i**s**t. Shows the files inside the given path (`/sys/firmware/efi`)

If you see a directory called `efivars` in the result, you can install with UEFI.

### Checking the Internet Connection

Check your internet connection.

```console
# ping -c 3 www.google.com
```

> **Command explanation**
>
> - The `ping` command sends pings to the server specified after it.
> - You can change the number 3 after `c` to another number.
> - If you left out the `c` option or forgot the number and `ping` keeps running, don't panic: you can stop it with `ctrl + c` (`SIGINT`; the interrupt signal)!

For wireless, the Arch Linux installer includes the [iwctl](https://wiki.archlinux.org/title/Iwd#iwctl) utility.

```console
#  iwctl
[iwd]#device list
// Check your device name

[iwd]#station YOUR_DEVICE scan
[iwd]#station YOUR_DEVICE get-networks

// For an open network (no password)
[iwd]#station station YOUR_DEVICE connect NETWORK_SSID

// For a private network whose password you know, exit with 'ctrl+d' and run
#  iwctl --passphrase NETWORK_PASSWD station YOUR_DEVICE connect NETWORK_SSID

```

After connecting to the network through the steps above, check the connection with `ping` to make sure it works.

### Planning Partitions

This step is about **dividing the drive into sections and making a plan**; the formatting that matches the plan happens in the next step.

Partitioning is the step where you decide how to divide a large storage device into sections.\
It's usually divided into three parts:\
a `section for booting`, a `section where data is stored`, and a section used as `Linux swap`.

First, regarding the `partition for booting`: if you booted with UEFI and want to install that way, you need to plan an **E**FI **S**ystem **P**artition, called the `ESP`. Without an ESP, setting up booting in a UEFI environment gets very tricky.

The ESP is a partition set to ‘**type**: EFI System Partition’ with ‘**format**: FAT file system’. The ESP is mounted on the `/boot` directory.

If you're more deeply interested, it's worth looking up the difference between mounting it on `/boot` and on `/efi` [[further reading](https://wiki.archlinux.org/title/EFI_system_partition#Typical_mount_points)].

The second section, the data storage section, will simply use the `Linux File System` type. Think of this second partition as the area mounted on `/`, the `root` directory, where all data will be stored.

The last, the swap partition, gets the `Linux Swap` type, as the name suggests.

(Feel free to plan additional partitions to suit your environment or taste, such as a separate `/home`.)

> **Do I really need `SWAP`? And that big?**
>
> Linux swap means loading data into the part of the hard disk set up as swap and using it like RAM when RAM runs short.
>
> Some users don't use swap at all, saying "the moment swap is used, you're already short on RAM, so I'd rather upgrade the hardware than use swap".
>
>This view is common among LFS users. The official manual even says so: [LFS official manual](http://www.linuxfromscratch.org/lfs/view/stable/) > 2.4.1.2. The Swap Partition > “Swapping is never good. (rest omitted)”
>
> On the other hand, there are also many [cases](https://opensource.com/article/19/2/swap-space-poll) where systems set aside around 4G of swap regardless of actual RAM size, even with more than 4G of RAM.
>
> **There's also the [opinion](https://forums.gentoo.org/viewtopic-t-1069006.html) that you must have a swap partition as large as your RAM for features like `hibernation`** (putting a device such as a laptop to sleep by closing the lid).
>
> This document goes with the most widely used setup.

Let's check which drives you have with the command below.

```console
# lsblk
```

In the output, you'll see large storage devices such as `sda`, `sdb`, or `hda`.\
If there are several large devices, they're listed in the order `sda`, `sdb`, `sdc`.\
`hda` is a hard disk connected via the older IDE interface, and `sda` is an HDD or SSD connected via the more recent SCSI/SATA interface.

**Make absolutely sure it isn't the USB drive you mounted for booting.** Checking by capacity usually works fine.

Let's assume it's `sda`, the most common case, and make a plan.\
In this installation, we'll split it into three sections in total.\
Once split, they become `sda1`, `sda2`, and `sda3`.\
If it were `sdb`, they'd be `sdb1`, `sdb2`, `sdb3`, right?

Paths are usually written like `/dev/sda1`, `/dev/sda2`, where the leading `dev` stands for 'device'.

Let's plan it like this.

> Plans may differ from person to person, but you must remember clearly **which number will be used for what** among your partitions. In particular, get the concepts of **boot partition/swap partition/root partition** straight, and remember which of **1, 2, 3** each will be before following the examples below.

| Partition   | Type                 | Code     | Size           | Purpose          |
| ----------- | -------------------- | -------- | -------------- | ---------------- |
| /dev/sda1   | EFI System Partition | EF00     | 512M           | ESP(/boot)       |
| /dev/sda2   | Linux Swap           | 8200(82) | 4G             | Swap partition (swap) |
| /dev/sda3   | Linux FileSystem     | 8300(83) | Remaining space | Root partition (/) |

> **Partition sizes?**
>
> I'm well aware that there's no right answer for the size of the ESP, the partition commonly mounted as the 'boot directory', or for the swap size, and that recommendations all differ. However, **to proceed without confusion from a beginner's point of view, let's first go with the most basic sizes, as above**. If you're already familiar with various advanced setups, feel free to adjust them as you install.

### **cfdisk**

(I've removed the part using `gdisk`. If you want to use a program other than `cfdisk`, please refer to other documentation.)

> **The commands below make every file on the hard disk unusable.
>Be sure to back up any important files, or files you can't afford to lose, before proceeding.**

The command below includes a disk path; enter **the one that matches your environment**.

```console
# cfdisk /dev/sda
```

For a UEFI installation, check that the label at the top says `GPT`. It must not be `MBR` or `DOS`!

If it asks at the start, choose `GPT`.

If asked, proceed with `Primary`.

If existing partitions are shown in the table, go down the list one row at a time and `DELETE` the existing partitions. (Use the arrow keys on your keyboard.)

Then, once all the space is `FREE SPACE`, we'll create the planned partitions.

The first partition to create is `/dev/sda1`, EFI System Partition, 512MB.

So,

```
Select NEW (Enter)
Enter the size: 512M
If asked, for this and every partition after it: select Primary
Move the cursor to TYPE with the right arrow key: Enter
Select EFI System, usually at the very top
```

The second partition to create was `/dev/sda2`, Linux swap, 4GB.

Move down with the arrow key to point at `free space`, and you'll see the `NEW` button enabled again at the bottom.

```
NEW
Enter the size: 4G
TYPE: find and select Linux Swap
```

Now let's create `/dev/sda3` too.

```
NEW

Check that the default size shown is the maximum remaining space: Enter

No need to choose TYPE again;
it's probably Linux FileSystem automatically,
but double-check in the table just in case.
Finally, select WRITE.
Select QUIT to exit.
```

Since it's written according to plan, let's WRITE, in the sense of saving, and exit.

```
Move to WRITE with the arrow keys, select with ENTER
Select QUIT to exit
```

Now move on to the formatting step!!

### Formatting

Each partition is formatted with the file system that matches its plan.

In a UEFI environment, the ESP, that is, **the boot partition, wants the FAT32 file system**. So `sda1`, which we planned as the EFI System partition, will be formatted as `fat32`, and `sda3`, which we decided to use as the main file system, will be formatted with `ext4`, the most widely used file system.

> **The swap partition is formatted in the next step.**\
> Be careful not to format `/dev/sda2`!

`vfat` finishes quickly since we gave it 512MB, while `sda3`, to be formatted as `ext4`, normally takes a moment depending on the size you gave it.

The command below includes disk paths; enter **the ones that match your environment**.

```console
# mkfs.vfat -F32 /dev/sda1
# mkfs.ext4 -j /dev/sda3
```

> **Command explanation**\
> `mkfs`: **M**a**K**e **F**ile **S**ystem

### Swap

Let's create the swap drive.

The swap drive goes through the same formatting as above, plus an extra step to turn it on.

The command below includes a disk path; enter **the one that matches your environment**.

```console
# mkswap /dev/sda2
# swapon /dev/sda2
```

> **Command explanation**\
> `mkswap`: **M**a**K**e **Swap**

### Mounting

It's easiest to think of mounting as connecting each directory of the [Linux file system hierarchy](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/index.html) (the file tree branching out from `/`, the root directory) to a particular partition of a particular drive.

If you're following the plan introduced in this guide, you only need to mount the root directory (`/`) and the boot directory under root (`/boot`).

Naturally, since `/boot` exists as a subdirectory of `/`, you must mount the `/` partition, which was third in the partition plan, first; only then can you create the `/boot` directory and mount the drive there. The guide below is written for this scenario, so it will work fine even if you just follow along without worrying.

Of course, depending on the plan you came up with in the partition planning step, you can also mount the home directory (`/home`) and others on a separate drive (for example, `/dev/sda4`).

**The directory structure under `/mnt` during installation becomes the directory structure under `/` once you finish installing and reboot.**

> **The role of the installation USB**
>
> You mount or create all the new system's directories under `/mnt`, install the essential software (`pacstrap`), then enter that system (`chroot`).
>
> In other words, the installer does nothing more and nothing less than provide a live Linux system with the tools to create another new Linux environment under `/mnt`.
>
> That's exactly why you can install Linux distribution B from Linux A's iso or live USB!

First, let's connect the Linux FileSystem partition (`/dev/sda3`), to be used as the root directory, to `/mnt`.

The command below includes a disk path; enter **the one that matches your environment**.

```console
# mount /dev/sda3 /mnt
```

Once you've done this, `/mnt` will become `/` after you finish installing and reboot.

Then create the boot directory under `/mnt`.

```console
# mkdir /mnt/boot
```

> **Command explanation**\
> `mkdir`: **M**a**K**e **Dir**ectory

Mount `/dev/sda1`, which we prepared for mounting, on `/mnt/boot`.

```console
# mount /dev/sda1 /mnt/boot
```

### Setting the Time

You can sync with server time using the `#timedatectl set-ntp true` command. I especially recommend doing this when installing Linux for the first time on a computer that ran Windows, since the two use different time standards, localtime and UTC respectively.

A more classic way is to enter the correct current time yourself with the `#date` command. Check the current UTC time by searching Google for 'current utc time' on another device such as your phone, then enter that time with the `#date` command. The order is 'month day hour minute year'. For example, 14:56 on December 31, 2021 is entered as `#date 123114562021`. Knowing this method may seem pointless, but commands like `#timedatectl` only exist on systems that use `systemd` as their init system (sure, nine out of ten distributions use `systemd`..). So knowing another way can help in unexpected situations.

If the above is too much of a hassle, or you don't mind syncing the time automatically over the network, you can skip it with `#ntpd -q -g`. People sensitive about privacy dislike automatic server time sync because, among other things, it exposes your device's IP to the ntp server. Also, `ntp` isn't included in the Arch Linux installation iso. If you go this route, install the `ntp` package with `#pacman -S ntp` after `#pacman -Syu`, then use it.

```console
// Pick whichever is convenient
#  timedatectl set-ntp true # or
#  date 123114562021        # or install ntp and
#  ntpd -q -g
```

After setting the time, sync the hardware clock with the system time using the command below.

```console
# hwclock --systohc --utc
```

> **Command explanation**\
> Syncs the hardware clock with the current system clock, based on utc
>
> - `systohc`: from the system time (**sys**tem time) to the hardware time (**h**ardware **c**lock)
> - `utc`: states that `utc` is used as the reference, not local time (`localtime`)
> - `hctosys`: pushes the hardware time to the system time instead.

### mirrorlist

> You can follow the original guide, but **recent isos already write a suitable `mirrorlist` when the network is connected.** So it turns out **you can just use it as-is**, with no changes needed.
>
> There's some extra information here that may help, so it's worth reading once, but **if you want to skip it, continue from the `pacstrap` section, one chapter down.**

The mirror list is a list of servers Arch uses as its base database when installing packages. For installation and everyday use, the servers in the mirror list should be ordered so that those closest to you, or fastest to connect to, come first.

So let's edit it. We'll open the file with the nano editor.

```console
// Backing up the original file is a must
#  cd /etc/pacman.d
#  cp mirrorlist mirrorlist.bak

// Open the file to edit it
#  nano -w /etc/pacman.d/mirrorlist
```

If you want to delete everything and write it from scratch, move the cursor to the line where you want to start deleting and press `Alt+Shift+T` to delete through the last line.

This is the result generated on the [Arch Linux mirrorlist page](https://archlinux.org/mirrorlist/?country=KR&protocol=http&protocol=https&ip_version=4&ip_version=6).

```file:/etc/pacman.d/mirrorlist
##
## Arch Linux repository mirrorlist
## Generated on 2024-01-11
##

## South Korea
Server = http://kr.mirrors.cicku.me/archlinux/$repo/os/$arch
Server = https://kr.mirrors.cicku.me/archlinux/$repo/os/$arch
Server = http://ftp.kaist.ac.kr/ArchLinux/$repo/os/$arch
Server = http://mirror.funami.tech/arch/$repo/os/$arch
Server = https://mirror.funami.tech/arch/$repo/os/$arch
Server = https://seoul.mirror.pkgbuild.com/$repo/os/$arch
Server = http://ftp.harukasan.org/archlinux/$repo/os/$arch
Server = https://ftp.harukasan.org/archlinux/$repo/os/$arch
Server = http://ftp.lanet.kr/pub/archlinux/$repo/os/$arch
Server = https://ftp.lanet.kr/pub/archlinux/$repo/os/$arch
Server = http://mirror.morgan.kr/archlinux/$repo/os/$arch
Server = https://mirror.morgan.kr/archlinux/$repo/os/$arch
Server = http://mirror.premi.st/archlinux/$repo/os/$arch
Server = https://mirror.premi.st/archlinux/$repo/os/$arch
Server = http://mirror.siwoo.org/archlinux/$repo/os/$arch
Server = https://mirror.siwoo.org/archlinux/$repo/os/$arch
Server = http://mirror.yuki.net.uk/archlinux/$repo/os/$arch
Server = https://mirror.yuki.net.uk/archlinux/$repo/os/$arch
```

> **How to save and exit after editing with the nano editor**\
> `Ctrl+X`→`y`→`Enter`
>
> - `ctrl+x` : exit
> - Then it asks: there are modifications; save them? `y`
> - What file name? (the default is the original file name) `Enter`

> **How to save and exit after editing with the vim editor**\
> `Esc`→`:wq`→`Enter`
>
> - Use `esc` to leave `insert` mode and return to `normal` mode.
> - Use `:` to get ready to enter a command. You'll see a colon at the bottom of the screen.
> - `w`rite, `q`uit

If you know what [reflector](https://wiki.archlinux.org/title/reflector) is and are comfortable using it, feel free to install it at this stage and regenerate the list by speed (`--rate`) and country code (`KR`), or to suit your environment. Moreover, once installed, you can set it to update the mirror list at every boot via a `systemd` service. Write the options you want in `/etc/xdg/reflector/reflector.conf` beforehand, then enable it with `# systemctl enable reflector.service`.

### pacstrap

Now, using the `pacstrap` command, install the essential programs into `/mnt`, which will become the new system.

The command including what you must install is below, **but be sure to read the paragraph below the command!**

```console
# pacstrap /mnt base linux linux-firmware networkmanager
```

In addition, the items below are worth adding after `linux-firmware` above.

They're listed from the packages I expect you'll most likely need to those you probably won't.

```
# At least one editor program
nano
vim
emacs

# Needed by most users
base-devel
man-db
man-pages

# From here on, if you don't know what the package is, you don't need it.
# But install it if your boot setup needs it.
# Of course, you can also install it with pacman after arch-chroot
mdadm
lvm2
```

See the links for the [base package list](https://archlinux.org/packages/core/any/base/) and the [base-devel package list](https://archlinux.org/groups/x86_64/base-devel/).

### genfstab

`/etc/fstab` is the file system table (**F**ile **S**ystem **Tab**le) file that tells the system which drive should be mounted on which directory at boot.

The step below uses a script called `genfstab` to generate that file system table file (`/etc/fstab`) automatically. (Systems that require even more tedious manual work, such as Gentoo and LFS, don't provide a utility like `genfstab`, so you write it by hand.)

```console
# genfstab -U /mnt >> /mnt/etc/fstab
```

> **Command explanation**
>
> - `genfstab` : **gen**erate **F**ile **S**ystem **tab**le
> - `U` : uppercase U (means it will be written using `UUID`; search: `UUID vs PARTUUID`)
> - takes the drives currently mounted under `/mnt` as they are
> - `>>`: writes in addition (append) to the file name that follows. With a single angle bracket (`>`), it becomes an overwrite command: even if the file already exists, its contents are erased and written anew.
> - `/mnt/etc/fstab`: the file name to write the contents to

You can also check what was written with the `# cat /mnt/etc/fstab` command.

> **Command explanation**\
> `cat`: short for con**cat**enate

It's often used to show the contents of the files given as arguments, or to feed them into another file using a pipe (`|`→`Shift+\`).

## Entering Arch Linux

### Entering Root

```console
# arch-chroot /mnt
```

> **Command explanation** \
> `arch-chroot`: **arch-ch**ange **root**, enters an environment that treats `/mnt` as `/`.

We're now inside the system being installed! Let's continue with additional system setup.

Software you install here will stay installed after you reboot later. For example, if you want to use the vim editor and install it with `# pacman -S vim` after `# pacman -Syu`, the package will be installed when you boot into the Arch environment after rebooting.

> If it won't boot after installation, you can fix errors using `# arch-chroot /mnt`.
>
> ```console
> Installation finished, rebooted, and there's a problem.
> Boot from the Live USB again
> # mount -v -t ext4 /dev/sda3 /mnt
>
> Check that the directory structure exists properly on /dev/sda3 mounted at /mnt
> # ls /mnt
>
> If it looks fine, mount the remaining drives too (if needed)
> # mount -v -t vfat /dev/sda1 /mnt/boot
>
> Enter the system again
> # arch-chroot /mnt
>
> Do the necessary repairs
>
> Exit the same way as when finishing the installation
> # cd
> # umount -lR /mnt
> # reboot
> ```

### Checking the Internet

```console
# ping -c 3 www.google.com
```

### Setting the Password

```console
# passwd
```

The 'password to set' that you type is **not shown on screen by design**. Don't be alarmed!!

Entering it **twice in total** for confirmation is expected. Don't be alarmed!!

### Generating Locales

```console
# nano -w /etc/locale.gen
```

When the file opens, use the arrow keys to go down and find `en_US.UTF-8`. (vim users → `/en_US.UTF`, `Enter`, `n` until you reach the right spot, then `x` on that line's comment character)

**Remove only the leading comment character (`#`).**

`/etc/locale.gen` determines which locales are generated. That is, you need to uncomment `en_US.UTF-8` and `ko_KR.UTF-8` here so that whichever of the two you write in `locale.conf` below can actually be used.

If you won't use `ko_KR.UTF-8`, just uncomment `en_US.UTF-8` and generate.

Generate according to the configuration file.

```console
# locale-gen
```

### Setting the Language

The `locale.conf` file manages the system-wide language. We'll write `en_US.UTF-8` in it now; if you'd rather have Korean directory names, Korean error messages in the terminal, and so on, enter `ko_KR.UTF-8` instead.

```console
# echo LANG=en_US.UTF-8 > /etc/locale.conf
```

> **Command explanation**
>
> `echo` and `>` mean **overwrite** the `/etc/locale.conf` file with the text `LANG=en_US.UTF-8` as-is. If the file doesn't exist, it's created. You can also use `>>` to append while leaving the existing content in place.

As before, you can check this with `# nano -w /etc/locale.conf` or `# cat /etc/locale.conf`.

> Instead of the command above, you can also apply it with something like `# localectl set-locale LANG=en_US.UTF-8`. That only works with the `systemd` init system, so if you use a variety of systems, I think the command explained in the main text will serve you better.

(Optional) The command below declares the language as `en_US.UTF-8` in **the shell environment you're currently working in** as well.

```console
# export LANG=en_US.UTF-8
```

### Setting the Hostname

Use the command below to **set the hostname you want, that is, the PC's name**

```console
# echo YOURHOSTNAME> /etc/hostname
```

### Setting the Local Time

We set the time to UTC above, so now we set it to Korean time.

```console
# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

> **Command explanation**
>
> - `ln`: **l**i**n**k
> - Options
>   - `-s`: **s**ymbolic
>   - `-f`: **f**orce
> - Links `/etc/localtime` to `/usr/share/zoneinfo/Asia/Seoul`

This makes any future reference to `/etc/localtime` actually point to `/usr/share/zoneinfo/Asia/Seoul`.

> It's easiest to think of a symbolic link, or soft link, in Linux as similar to creating a shortcut on Windows. That is, `ln -s A B` plants a signpost saying 'go to A' whenever B is referenced. ~~(Like `int* B = &A`)~~
>
> The argument order is really confusing, but just memorize that on Unix you write ‘what exists → what doesn't exist yet’. You put up signpost B, which doesn't exist yet, pointing to file A, which already exists. `mv` is also what exists → what doesn't, and `cp` is also what exists → what doesn't.

### Adding a User

In the command below, enter **the name you want to use** in place of `SOMEUSERNAME`.

```console
# useradd -m -g users -G wheel -s /bin/bash SOMEUSERNAME
```

> **Command explanation**
>
> - `m` : creates a home directory named after the user. It becomes the user's home.
> - `g`: lowercase g means the primary group, so the user is included in the `users` group.
> - `G`: sets additional groups to belong to (see: [Arch Linux group list](https://wiki.archlinux.org/index.php/Users_and_groups#Group_list))
> - `wheel`: the administrators group (the group that can use the `su` command)
> - `s`: option for using a non-default shell (when you want to use `zsh` or `fish`)
> - `/bin/bash`: since `s` says we'll use another shell, this specifies bash as that shell

Set the password for the newly created user.

In the command below, you must **change `SOMEUSERNAME` to the username you chose**.

```console
# passwd SOMEUSERNAME
```

### sudo

> **sudo? doas? su -l?**\
> Recently there's been some minor controversy about `sudo` on international forums.
>
> The main arguments are the opinion, often seen in the BSD camp, that 'you can take a more minimal approach with OpenBSD's `doas`',
> and, considering that `sudo` was originally developed for use on multi-user computers, that 'in a single-user environment, you need neither `sudo` nor `doas`, and becoming root directly with `su -l` or similar is more appropriate security-wise'.
>
> The judgment is yours.
>
> Since this is a guide for beginners, we'll install `sudo`, which you'll run into most often.

If sudo isn't installed (check with `# which sudo` → if a path such as `/bin/sudo` appears, it's installed), install it with `# pacman -S sudo`.

If you're comfortable with the vim editor, you can just run `# visudo` (if the `EDITOR` environment variable is set to `vi`) or `# EDITOR=vim visudo`.

```console
# EDITOR=nano visudo
```

> The `sudoers` file can also be edited with the `# nano /etc/sudoers` command, but **in principle, it should be edited through `# visudo`**. Since `visudo` checks the `sudoers` file for syntax errors and locks it, editing it directly with `# nano /etc/sudoers` is not recommended.

Let's add `wheel`, the extra group of the user we just created, to the groups allowed to use the `sudo` command.

```file
##
## User privilege specification
##
root ALL=(ALL) ALL

## Uncomment to allow members of group wheel to execute any command
#  %wheel ALL=(ALL) ALL   // It was commented out like this;
%wheel ALL=(ALL) ALL      // remove the '#' to uncomment it, like this.

## Same thing without a password
#  %wheel ALL=(ALL) NOPASSWD: ALL
// If you uncomment the setting above,
// users in the wheel group can run commands
// just by prefixing sudo, 'without even needing a password'.
// I strongly advise against it for security reasons.
// Only uncomment it if you know exactly what change you're making.
```

### Bootloader

Let's install the bootloader. You can choose one of two.

1. `grub` is used by most distributions, so learning it will keep you comfortable when you deal with other distributions. However, alongside `grub`, you'll install additional programs such as `efibootmgr`.
2. `systemd-boot` boots using a feature built into `systemd`. You may find the first installation a little trickier, but I think it fits very well with the Arch distribution's philosophy of minimizing additional installs. (Explained after the `grub` part.)

> If you want my recommendation, I recommend `systemd-boot`, but if you've never heard of either, or even if you have but don't know what other bootloaders like `{e,}lilo` are, please use `grub`.
>
> Since a bootloader's core job is booting the system, unless you have enough experience using various bootloaders or modifying them yourself, I think it's right to use the package that many distributions use and for which documentation and support are easy to find.

> **grub '2'? gummiboot?**
>
> `grub` is currently `grub2`. Known as `grub-legacy`, it's excellent software that's been used since the old BIOS days. However, because it provides so many features, it includes features that are meaningless to users who only want to boot in a UEFI environment. Some take that to mean the software may be more prone to unexpected conflicts or problems.
>
> `systemd-boot` is also called `bootctl` or `gummiboot`, because that was its name when it was external software, before `systemd` absorbed it. Now it's fully merged with `systemd` and only assists with UEFI booting (of course, you can install it for BIOS booting, but I don't recommend it). It's also a program already installed by default during the Arch Linux installation.

### **grub**

Install it. Before installing, update and sync the local repository (information about the software the computer being installed has) against the repository (the Arch Linux package store).

```console
# pacman -Syu
```

> **Command explanation**
>
> - `S`: sync with the remote repository
> - `y` (`--refresh`): an option usable with `S` that downloads a fresh copy of the remote repository's package DB
> - `u` (`--sysupgrade`): an option usable with `S` that upgrades system packages to match the downloaded DB

Now let's fetch and install the packages we'll use.

```console
# pacman -S grub efibootmgr
```

Now let's install grub so it can boot the system.

> Many people find this step confusing: it's 'installing the bootloader onto the system'. That is, it's the step where `grub`, which will be responsible for the boot process, checks the system configuration and makes it bootable.
>
> It's not downloading and installing the `grub` package. The package itself was installed with the command above.

```console
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch --recheck
```

> **Command explanation**
>
> - `grub-install`: the grub install command
> - `target=x86_64-efi`: sets the target architecture to `x86_64-efi`
> - `efi-directory=/boot`: sets the location of the `efi` directory
> - `bootloader-id=arch`: sets the bootloader name to `arch`
> - `recheck`

Now you can change the bootloader's default settings with `# nano -w /etc/default/grub`. For example, you can shorten the time to wait for choosing an OS to boot to 0 or 1 second. We won't touch it here. Once the settings are done, generate/install the 'file' for booting (`/boot/grub/grub.cfg`) based on the settings in `/etc/default/grub`.

```console
# grub-mkconfig -o /boot/grub/grub.cfg
```

> **Command explanation**
>
> - `grub-mkconfig`: grub generates the configuration file actually used for booting (`grub.cfg`) according to the settings written in `/etc/default/grub`.
> - `o`: in the UNIX family, `o` is short for output and is often used to specify the path/name of the generated file.
> - `/boot/grub/grub.cfg`: the most standard path and file name checked at boot.

This is also the command you use later whenever you change grub's settings for whatever reason. In particular, when experimenting with kernel parameters and the like, you edit the `/etc/dafault/grub` file and then update with the command above.

To summarize:

- after downloading and installing the grub package,
- installing the grub bootloader onto the system leaves an extra configurable file called `/etc/default/grub`; if needed, edit it to your taste, then
- you must create `/boot/grub/grub.cfg` with `# grub-mkconfig -o /boot/grub/grub.cfg`,
- so that the boot process can read the `/boot/grub/grub.cfg` settings and boot.

### **systemdboot**

**This only applies if you decided not to use grub.**

```console
# bootctl --path=/boot install
```

You need to edit the `/loader.conf` file in `/boot/loader`.

```console
# nano -w /boot/loader/loader.conf
```

Write the following in the file

```
default arch
editor 1
timeout 3
```

> **Configuration explanation**
>
> - `editor`: during boot, you sometimes adjust kernel parameters or boot settings using the ‘e’ key, etc.; setting this to 0 prevents that. My personal recommendation is to set it to ‘1’ for now and change it after confirming everything works properly.
> - `timeout 3` is how long the boot screen waits for a selection. Likewise, set it for now; you can change it to ‘0’ or the like later.

For `arch` after default, put a name you'll recognize. Usually `arch` is used. But be careful: remember the name you put here, because in the step right after this you must use the same name for the `abcd` part of the `entries/abcd.conf` file name. **If you write `arch` now, you just create an `entries/arch.conf` file later.**

Now let's create and edit a file in the `entries` directory under the `loader` directory.

```console
# nano -w /boot/loader/entries/arch.conf
```

Before adding content to the `arch.conf` file, read the **Note** section below once; I'll show an example file at the end of the note.

> **Note**
>
> If you run `# ls /boot`, you'll see `loader` and various image files (`vmlinuz`, `initramfs-linux`, etc.) under the `/boot` directory. Those are what you include in the file's content.
>
> For `title`, use a name that's easy to recognize. The name written here isn't needed by any other setting; it's used as the menu name during boot. People usually use something like `ArchLinux`.
>
> **Watch out for the ”z” in `vmlinu”z”` in the `linux` entry.**
>
> ~~Additionally, I use `quiet` and so on after the `rw` option.~~ I used to, but now I just don't use the `quiet` option again. No big reason; I wanted to see whether anything says Failed.

```
title ArchLinux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=/dev/sda3 rw
```

> **Someone told me to use PARTUUID, what's that??**
>
> For the `option` entry, the recommendation is of course to use `PARTUUID`. But for now, we'll substitute specifying the root partition's path. According to the wiki, "`PARTUUID` has the advantage when a partition is reformatted or its path mapping changes", but assuming that's rare for ordinary and beginner users, we specify the path. Alternatively, changing it after confirming that the system boots properly is also an option.
>
> If you want to use `PARTUUID`,
>
> (1) copy or write down the `PARTUUID` of the root partition (here, `sda3`) using `# blkid`, then change the content of the `options` entry above to `root=PARTUUID=123efg45-789 rw`. Of course, your own `PARTUUID` goes in place of `123efg45-789`. (A `PARTUUID` is long by nature. Don't be alarmed! / Check carefully that nothing like `“` (double quotes) got added!!)
>
> (2) If writing it down is a hassle, save the file with the `options` line deleted, then use `# echo "options root=PARTUUID=$(blkid -s PARTUUID -o value /dev/sda3) rw">> /boot/loader/entries/arch.conf` (a command with nothing you need to replace for your environment). Whichever method you use, be sure to run `# cat /boot/loader/entries/arch.conf` again to check that what you added is written properly.
>
> If adding the `PARTUUID` path this way leaves you with two `options` lines, it goes without saying that you delete the original `options` line written with `/dev/sda3`;;

### exit

If you leave the `chroot` environment now with `# exit`, introduced below, and reboot, you won't be able to connect to the network. So let's install the network manager and enable it.

First, if you haven't installed it yet,

```console
# pacman -S networkmanager
```

Then set the network manager to run automatically on reboot.

```console
# systemctl enable NetworkManager.service
```

> **Command explanation**
>
> `systemctl` is the command that manages services (daemons) in the `systemd` init system. Use the `start` option to turn a service on and start using it right away in the currently booted environment, and the `enable` option to have it start automatically at every boot. At the end, write the name of the target service.

If you missed this part and have no network, you can re-enter the system using the `chroot` method and install it.

> If it won't boot after installation, you can also fix errors using `# arch-chroot /mnt`.
>
> ```console
> // Installation finished, rebooted, and there's a problem.
> // Boot from the Live USB again
> # mount -v -t ext4 /dev/sda3 /mnt
>
> // Check that the directory structure exists properly on /dev/sda3 mounted at /mnt
> # ls /mnt
>
> // If it looks fine, mount the remaining drives too (if needed)
> # mount -v -t vfat /dev/sda1 /mnt/boot
>
> // Enter the system again
> # arch-chroot /mnt
>
> // Do the necessary repairs
>
> // Exit the same way as when finishing the installation
> # exit
> # cd
> # umount -lR /mnt
> # reboot
> ```

```console
# exit
```

### umount

Unmount the devices that were mounted on each directory.

```console
# umount -lR /mnt
```

### reboot

Reboot!!!

```console
# reboot
```

Pull out the USB drive when the machine shuts down and starts up again.

After rebooting, you'll see a screen asking you to log in, which means **the installation succeeded**! **Congratulations!**

Some of you are probably happy, and some are probably thinking 'what is this?'; I think the latter feel that way because they're used to graphical environments. Except for those who'll work headless, like on a server, once you install the desktop environment that follows, you can use Arch in a graphical environment.

## Installing a Graphical Environment

~~(Times have changed..)~~ Let's install `wayland`.

> The desktop environment part, that is, the `wayland` and `gnome` parts, covers software I don't use all the time, so it may be less up to date.

### Logging In

Log in with the ID/password you set.

Since you're logged in as a regular user, the prompt changes to `$`. Run commands with `sudo`.

### Rechecking the Internet Connection

```bash
$ ping -c 3 www.google.com
```

### Configuring Pacman

We have a few things to install now, so let's update with `pacman`.

```console
$ sudo pacman -Syu
```

It's also worth taking a look at the `/etc/pacman.conf` and `/etc/makepkg.conf` files.

### Basics

This guide is based on `Gnome`.

### Required Installs

If you'd also like to use the additional Gnome software, install `gnome-extra`.

```console
$ sudo pacman -S xorg-xwayland gnome (gnome-extra)
$ sudo systemctl enable gdm
```

Once installation is complete, run `systemctl enable` so the display manager can be used. The `gnome` package includes the essential software used in the `Gnome` desktop environment, including `gdm`.

`gdm` is the 'Gnome Display Manager', a program that manages **the desktop environment's login screen** and the overall graphical environment. Remember running `# systemctl enable NetworkManager` earlier for the internet connection? This time we used `enable` so the gdm service runs at every boot.

### reboot again

Reboot.

```console
$ sudo reboot
```

So, how is it? Did the login screen boot into a graphical environment?

After entering your user password, the Arch Linux Gnome environment is all set.

Ah! But there are a few more recommendations!

### Installing a Web Browser

#### **Firefox**

What should you do if the web browser you want to use is **Firefox or Google Chrome**? For Firefox, just install it!

```console
$ sudo pacman -S firefox
```

Firefox is installed!

> Software you can install with pacman is what's available in Arch's official repositories. The official repositories only contain software compatible with Arch Linux's licensing, that is, open source/free software.
>
> For Chrome, the official repositories have an open source version of Chrome called **chromium**. Honestly, the difference between the two isn't big! (Installing chromium: `$ sudo pacman -S chromium`)
>
> But if you absolutely must use Google Chrome, let's try **the AUR, Arch's pride and joy**!

> **vscode is acting weird!**
>
> For the reason above, the visual studio code installed from the official repositories is actually an open source version called [Code-OSS](https://archlinux.org/packages/community/x86_64/code/). If you want to use the proprietary software version that's exactly the same as the vscode you used from MS, learn how to use the AUR as explained below, then install '[visual-studio-code-bin](https://aur.archlinux.org/packages/visual-studio-code-bin/)' or a similar package.

#### **Google Chrome**

The AUR, the Arch User Repository, is a collection of packages maintained and uploaded by Arch users. **You definitely need to know how to build packages using the AUR.** Even if you use an AUR helper (yay, etc.), which you can find later in the 'Appendix', errors can happen at any time. When they do, you'll have no choice but to install manually.

> **Even according to the [Arch Linux wiki page](https://wiki.archlinux.org/title/AUR_helpers), an AUR helper is only ever third-party software that helps you, not an officially supported program.**

The basic idea is:

- Go to "aur.archlinux.org" and download the `snapshot` of the package you want to install.
- When you extract the downloaded snapshot, there's a `PKGBUILD` file,
- and with the `MAKEPKG` command, the computer installs it according to the contents of `PKGBUILD`.

I'll explain the details below.

> If you'd rather use `Git` to skip the tedious snapshot download→extract process, see the end of this chapter!!

For now, you only have the 'Web' software, so using that (or Firefox, if you installed it),

```file
Go to "https://aur.archlinux.org",
type ‘google-chrome’ in the search box on the right,
and the AUR page for Google Chrome appears.
In the small box on the right (Package Actions),
use Download Snapshot to save it to the Downloads directory.
Open a terminal,
```

```console
$ cd Down[tab]
```

All modern shells provide auto-completion with the `Tab` key.

```console
$ ls
```

You'll see `google-chrome.tar.gz`. `name.tar.gz` means a file compressed with `gzip` and packaged with `tar`. Extract it with the `tar` command.

```console
$ tar -xvzf goog[tab]
```

You can think of `tar` as a file extraction program.

> **Command explanation**
>
> - `x`: extract
> - `v`: verbose (print progress)
> - `z`: a file compressed with gzip
> - `f`: extract the archive with the name that follows
> - I auto-completed the rest of the name with the `Tab` key.

```console
$ ls
```

When it's done, run `ls` again and you'll see a `google-chrome` directory!

```console
$ cd goog[tab]
$ makepkg -risc
```

> **Command explanation**
>
> - `r`: after the build is complete, removes the packages that were installed as dependencies needed during the build. If you remove dependency packages with this option, and the AUR package still depends on them when it's upgraded, they'll be installed again when building the upgraded version. But if you know upgrades aren't frequent, or you want a cleaner system, turning this option on to remove them is a good idea.
> - `i`: automatically installs the package once the build is complete. Without this option, you'd have to install it with a follow-up command, `$pacman -U pkgname-pkgver.pkg.tar.zst`. This saves you that hassle.
> - `s`: automatically installs the dependencies that are official repository packages installable with pacman. If an AUR package depends on another AUR package, you must install that one manually first.
> - `c`: after installation, deletes the temporary files created during the build.

**google-chrome is installed!** You can find it via `Activities`>`All` in the top panel!

> Using `Git`
>
> ```console
> // If Git isn't installed
> $ sudo pacman -S git
>
> // I recommend creating something like an AUR directory (makes managing AUR packages easier)
> $ cd ~
> $ mkdir ~/AUR
> $ cd ~/AUR
>
> // See the Git Clone URL in the AUR page's search result
> // You can also click to copy it -> paste in the terminal with ctrl+shift+v
> // Clone the repository into a directory called Chrome
> $ git clone https://aur.archlinux.org/google-chrome.git Chrome
>
> $ cd Chrome
> $ makepkg -risc
>
> // Later, when there's an update
> $ cd ~/AUR/Chrome
> $ git pull
> $ makepkg -risc
> ```

### Korean Fonts

Since no Korean fonts are installed on the system, visiting a Korean site like Naver shows the Korean text as square boxes. You can fix this by installing a package such as `noto-fonts-cjk`. (`$sudo pacman -S noto-fonts-cjk`)

But here, to get more familiar with the AUR, let's install a font that isn't in the official repositories through the AUR.

```console
(go to aur.archlinux.org → search ttf-nanum → download snapshot)
$ cd Down(tab)
$ tar -xvzf ttf(tab)
$ cd ttf(tab)
$ makepkg -risc
```

With the steps above, the Korean font is installed. At this point, even if you use YouTube/Naver in Google Chrome, Korean text is displayed without breaking.

But you want to search in Korean, and you can't type Korean. What should you do?

### Typing Korean

Many people asked about setting up Korean input, so I created a [separate page]({{< ref "korean" >}}).

## Conclusion

Extract just the commands from this long post and you get the 30 or so lines below. The post is so long that you may have thought 'Arch Linux, simple? Yeah, right', but for users who understand and have memorized all these commands, when they're handed a laptop/desktop they need to use right away, they can build the system exactly the way they want in 5–10 minutes, depending on the setup!

In a way, I think this is exactly why people go on about it being a more concise and elegant distribution than Debian → Ubuntu or Red Hat → Fedora.

Thanks for making it through this long post! 🤩

Now then! Arch Linux is too easy, so on to [Gentoo]({{<ref "gentoo" >}})..lolol

```console
# cfdisk /dev/sdx
# mkfs.vfat -F 32 /dev/sdxN
# mkfs.ext4 -j /dev/sdxN
# mkswap /dev/sdxN
# swapon /dev/sdxN
# mount -v -t ext4 /dev/sdxN /mnt
# mkdir -pv /mnt/boot
# mount -v -t vfat /dev/sdxN /mnt/boot
# timedatectl set-ntp true
# hwclock --systohc --utc
# reflector --sort rate -c KR >> /etc/pacman.d/mirrorlist
# pacstrap /mnt base linux linux-firmware vim networkmanager
# genfstab -U /mnt >> /mnt/etc/fstab

# arch-chroot /mnt
# passwd
# vim -w /etc/locale.gen
# locale-gen
# echo LANG=en_US.UTF-8 > /etc/locale.conf
# echo pcname > /etc/hostname
# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
# pacman -S grub efibootmgr
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch --recheck
# grub-mkconfig -o /boot/grub/grub.cfg
# systemctl enable NetworkManager.service

# exit
# umount -lR /mnt
# reboot
```

## Appendix

### Reader Contributions

Contributed by Davido (<osh3606@gmail.com>) (thank you for the contribution!)

```console
// btrfs & timeshift
// Install the btrfs management tools
# pacman -S btrfs-progs

// Format the file system
# mkfs.btrfs /dev/sda3

// Create subvolumes
# mount /dev/sda3 /mnt
# cd /mnt
# btrfs subvolume create @
# btrfs subvolume create @home
# cd /
# umount /mnt

// Mount the subvolumes
# mount -o rw,noatime,compress=lzo,space_cache=v2,subvol=@ /dev/sda3 /mnt
# mkdir /mnt/home
# mkdir /mnt/boot
# mount -o rw,noatime,compress=lzo,space_cache=v2,subvol=@home /dev/sda3 /mnt/home
# mount /dev/sda1 /mnt/boot

// Edit fstab
// To use a snapshot program, you must remove subvolid from fstab after genfstab.
// Otherwise, a later btrfs restore will look for the pre-restore id and won't restore properly…
# nano /mnt/etc/fstab

// In the mount options for /, /home
// → delete subvolid=####
// → delete one of subvol=###/,subvol=###

// Snapshots/restores with timeshift
// After installation, take a btrfs snapshot of the freshly installed state (using timeshift)
# sudo pacman -S git
# git clone https://aur.archlinux.org/timeshift.git
# cd timeshift
# makepkg -si
# sudo timeshift --create --comments "Fresh install"

// To restore
# sudo timeshift --restore

// Choose the snapshot number you want
# sudo reboot
```

### Backing Up with btrfs Snapshots

(Reference: [timeshift developer's repository](https://github.com/teejee2008/timeshift))

You must switch to `btrfs mode` with the `# timeshift --btrfs` command (the default is `rsync mode`)\
Reason: backups are much faster, and since the backup itself is a byte-by-byte copy, reportedly nothing is lost.

#### Why remove subvolid from fstab, in detail

(Reference: [timeshift developer's repository issue](https://github.com/teejee2008/timeshift/issues/241))

- `fstab` must contain `@` and `@home`. You must use `subvol=@` instead of `subvolid`. The device must be specified with `UUID=` or `/dev/xxx`.
- timeshift was written for Ubuntu-based distributions. When used on Fedora or Arch, it will mostly work fine, but since it was built for that purpose, issues may come up.

### A Word About the AUR

An AUR helper is software that helps you update or install AUR packages.

In general, there are various kinds, ranging from

- utilities that only check whether the repository has a newer version than the package installed on your system,
- utilities that go one step further and download the newer version if it exists (search and download),
- utilities that take care of building too (search and build),
- and finally, utilities that also cover packages installed with `pacman`, that is, they search not only the AUR but also the official repositories to install and update, so you never need to use `pacman` day to day (pacman wrapper).

The [Arch wiki](https://wiki.archlinux.org/title/AUR_helpers) clearly states that **AUR helpers are not officially supported**. So you won't get support for problems that arise while using an AUR helper.

In older versions of this post, I listed `yaourt` first because it was so widely used it was practically the default. But `yaourt` was discontinued. So I updated it to say, "`yaourt` has been discontinued, so if you'd like another recommendation, I recommend `cower`", but `cower` has been discontinued too.

The popular ones right now seem to be `auracle` (search and download) and `yay` (pacman wrapper).

I haven't been using Arch Linux all that long (though it's getting long now;;), and in that time countless AUR helper packages have appeared, gained popularity, and faded away. It's easier on the mind to just create a `Git` or AUR directory under your home and manage packages by updating them with `$git clone`.

Above all, **I recommend not using AUR packages at all, or keeping them to a minimum, wherever possible.**\
↳ +1

~~The author currently (*2022/11/14*) has [aurutils](https://aur.archlinux.org/packages/aurutils) installed and uses it only for `$ pacman -Qm | aur vercmp`.~~ No longer used.
