---
title:  "Types of Linux Distributions"
date:   2019-03-01
url: 'distro'
---

## A Little History

In 1991, Linus Torvalds announced 'Linux Kernel, ver.0.01'.

It then incorporated the utilities of Richard Stallman's 'GNU (**G**NU is **N**ot **U**nix)' and established itself as a true operating system (OS).

In 1991, H. J. Lu's 'Boot-Root' was born as disk images that included the Linux kernel.

To make up for the difficult installation process and limited package management of the very early days (1991–92), 'SLS (Softlanding Linux System)' appeared. It reportedly included a graphical environment via the X Window System. It can be considered the first appearance of the distribution format that continues to this day.

> **Linux? LinuxOS? GNU/Linux?**
>
> Linux always refers to the kernel alone.
>
> Some argue that what is commonly called 'Linux' or 'Linux OS' should properly be called 'GNU/Linux'!

> ”Let me interject for a moment. What you guys are referring to as Linux, is in fact, GNU/Linux …(rest omitted)”
>
> → Richard M. Stallman

In December 1992, Yggdrasil Linux/GNU/X was released as a commercial distribution.

To fix the shortcomings of SLS (system-wide bugs), Slackware (by Patrick Volkerding) appeared in 1993, and then Ian Murdock released Debian, which is still famous today. Starting with Debian, a huge number of distributions such as Ubuntu began to pour out, and in the independent camp as well, various distributions such as Red Hat, Arch Linux, and Gentoo Linux were released.

## Three Ways to Classify

There are three main criteria for classifying Linux distributions.

The first is **lineage**.

By this criterion, you can divide them into the Debian/Ubuntu/Mint family, the Red Hat/Fedora/CentOS/recently Rocky family, and the independent ones (distributions built from scratch rather than based on another distribution) such as Arch Linux, Gentoo Linux, and Void Linux.

The second is the **package manager**.

Classifying by the package manager in use, such as `apt`, `rpm`/`yum`/`dnf`, `pacman`, `emerge`, `zypper`, `xbps`, `pkg`, or `eopkg`, is useful, but so is whether it's a binary or a source package manager. Today, 99% of distributions use binary package managers that allow very fast installation. However, Gentoo Linux, CRUX, LFS, and the BSDs when using ports have package managers that download source code and compile it locally.

Finally, there's the **release model**.

Recently, many technical limitations have been overcome, and 'rolling release' has been growing very quickly. This is a model with no fixed versions, where regular system upgrades alone keep your computer up to date. So as long as you update/upgrade regularly, everyone is using the same version. The Linux generally known as the epitome of rolling release is 'Arch Linux'. Rolling-release distributions tend to pick up what each package's developers release very quickly. The fixed release model, called 'fixed release', is typified by Ubuntu. With versions made of year.month, you can usually predict that a new version will come out at a set time. In addition, according to certain criteria, it provides LTS (Long Term Support) versions, which are supported much longer than regular versions, an advantage for servers and similar uses.

## By Release Model

Below, distributions are organized by release model.

### Rolling Release

Rather than periodic releases, this type of distribution can be updated whenever a new version comes out. It's characterized as 'Bleeding Edge'. Users who always want the latest version of packages (software) love it. With fixed release or LTS (Long Term Service) types, when a new version comes out you have the burden of downloading and installing it again and setting up your environment, but anyone whose PC runs a rolling-release OS can always keep the latest kernel and packages. Because rolling release by nature installs and runs the newest features whose stability hasn't been fully verified, problems occasionally occur.

#### Arch Linux

It's notorious for its difficult installation process (but what if there were a well-organized [guide]({{< ref "arch" >}})?). Once you successfully boot from bootable media, you're dropped into a CLI (command-line interface), and you install the drive partitions, formatting, base system, bootloader, and so on yourself. Since you build the whole PC yourself, it offers a lot of freedom.

It uses PACMAN (**PAC**kage **MAN**ager) as its package manager. Written in C, I think PACMAN is ahead of other distributions' package managers in speed and flexibility. Thanks to the AUR (Arch User Repository), you can also download sources and build them yourself.

The huge and powerful community built around the [Korean Arch Linux Wiki](https://wiki.archlinux.org/index.php/Main_page_(%ED%95%9C%EA%B5%AD%EC%96%B4)), the [Arch Linux homepage](https://www.archlinux.org/), the [Arch Linux forums](https://bbs.archlinux.org/), and others is another point of pride for Arch Linux. If you run into a problem while using Arch Linux, putting 'arch' in front of your Google search turns up all kinds of solutions. If you don't like the search results, or none fit your situation, ask in the communities above and users will gladly help. The lack of a GUI installer led to the birth of Manjaro, ~~Antergos~~ (discontinued -> forked as EndeavourOS), and others.

Installation is done in the CLI (command-line interface), and even after the basic installation is done, only the base system is installed; you have to install Xorg and a desktop environment or window manager yourself. So even if you successfully complete your desktop, you can't be sure how stable it is or how vulnerable it is security-wise. The community generally says "if it works, it works", but this can be quite annoying for perfectionists. As a rolling release, there are no specific versions, and the distribution keeps rolling along with continuous updates. So, considering the risk of breakage during major version upgrades that non-rolling distributions have, it's quite a comfortable distribution.

On international forums, people use the 'BTW I Use ARCH' meme to mock the many people who subtly show off being Arch users because Arch is installed manually. But the users themselves use it too.

#### Gentoo Linux

Gentoo Linux is generally known for an installation process that's even harder than Arch Linux's. Gentoo Linux users even compile and build the kernel themselves. That makes it **very flexible**, and you can create a **Linux optimized for your own hardware**. The downside is that the slower your PC's processing speed (CPU performance), the longer builds take. Since the package manager also compiles from source, it takes a bit more time. However, if you want to manage Linux down to its depths yourself, or you're curious about and drawn to an OS you build up yourself, it's well worth trying.

Gentoo is also a rolling release. As with Arch Linux, the entire installation happens in the CLI, and you have to do the configuration yourself. Going further than Arch, every package is downloaded as source code and compiled locally. The interesting thing is that if your hardware can keep up, it's not a big problem to use. Using `USE` flags, you can compile each package including or excluding the extra features you will or won't use. Personally, **if your CPU isn't workstation-class, I recommend trying it once but using a binary distribution (most ordinary distributions other than Gentoo)**.

### Fixed Release

These distributions release a new version at set points in time (Fixed Release). Even if quite some time has passed since a release, as long as it's still within its support period, you can receive support (it may be hard to use the latest package versions, but you'll at least get all the security patches). In the past, upgrading when a new version came out was very difficult, but companies like Red Hat have tried various ways to solve this, and that inconvenience has largely gone away. These distributions are used by people who don't particularly want to try the new features of the latest packages and want a **stable distribution**.

#### Debian

**I consider it the original among modern Linux distributions.** Debian, which appeared in the early '90s alongside SlackWare and others (strictly speaking, Slackware came first), still has many users thanks to its solid foundation. Having reigned as one of the oldest and most popular OSes, it has a huge number of packages. However, because it aims to be so stable, the period until a new release is long, and it's mainly used on servers. It has produced a famous child distribution, Ubuntu.

If you find Ubuntu Linux too boring or want to dig a little deeper, I recommend [Debian Linux](https://www.debian.org/). You might assume Debian Linux is outdated because it's old, but it's still going strong. 'Mature' is the better word. Debian Linux is famous for being solid. Since packages go through quite systematic review and testing before being uploaded to the repository (package collection), you can trust and use any package installable from Debian's default repositories. However, because of this, you may end up using much older package versions than on other distributions. You can get around this by editing `etc/apt/source.list` to use the `testing` version, which lets you use it like a rolling-release distribution.

#### Ubuntu

A distribution based on Debian and supported by the UK company 'Canonical', it's one of the distributions most often recommended to Windows or macOS users trying Linux for the first time. Installation is easy, usage is intuitive, and on top of that it's **an excellent distribution lacking nothing in important features**. It has branched out into Ubuntu Mate (Mate), Kubuntu (KDE), Xubuntu (XFCE), and Lubuntu (LXDE), based on various desktop environments.

If you're getting started with Linux, I recommend [Ubuntu Linux](https://www.ubuntu.com/) or its derivative, [Mint](https://linuxmint.com/). Most things can be done in the GUI (graphical UI environment), and installation is easy.

#### Mint

Built on Ubuntu (recently a Debian-based version is also being actively developed; [LMDE](https://linuxmint.com/download_lmde.php)). It's much loved for having a desktop environment similar to Windows. It's also perfectly fine for beginners to install and maintain.

#### Fedora

Named after the fedora, the hat worn by Shadowman in the Red Hat company logo. As a fixed release, you need to upgrade when a new version comes out. Thanks to much progress, problems during upgrades have become far less common. It also claims to be 'the closest to perfectly realizing the open source philosophy'. Accordingly, the official repositories carry packages that strictly follow the open source philosophy. However, if you want to install proprietary (owned by an individual or organization, not open source) packages, that's possible too. It uses the famous **RPM package manager** (Fedora: 'DNF package manager' / Red Hat: 'YUM package manager').

It's one of Red Hat's main derivative distributions. When Red Hat Linux (different from today's RHEL) was discontinued (2003), Fedora officially began, and RHEL (Red Hat Enterprise Linux), which uses Fedora as its baseline, became the only distribution officially supported by Red Hat (Fedora is run by its community; distributions run this way are also called 'community driven distributions'). **It's an excellent distribution that has often pioneered innovative efforts in the Linux world.** Since Red Hat Linux is used overwhelmingly on servers, I think Fedora Linux suits those who work, or plan to work, in server-related fields. (It's sometimes teased on internet communities as the beta testing version of RHEL.)

Many people consider Fedora the start of bleeding-edge (newest, cutting-edge technology) territory. Strictly speaking it's an out-of-the-box distribution, but it's famous for shipping the latest features and packages. Fedora is also famous for actively putting the newest packages in its official repositories. There may be some bugs, but it makes the latest package versions available to users. I think it hits the exact sweet spot between the out-of-the-box camp and the bleeding-edge camp. Linus Torvalds, the creator and developer of the Linux kernel, once mentioned in an interview that he used Fedora, saying "installing Debian is too hard, and I'm a developer, not a distribution installation expert". (The hard-to-install Debian Torvalds mentioned was probably from the 'Woody' era, when there was no GUI installer.) Personally, I also think it's an optimal distribution, development environment included, once you've built up some Linux experience.

#### RHEL (Red Hat Enterprise Linux)

Red Hat's enterprise-supported **paid** Linux. It's also famous for the Red Hat international certification track called the Red Hat Certification Program (RHCP). RHCP is a certification program that verifies whether you're qualified to use Red Hat servers or desktops.

If you register for development purposes, you can use it for free on up to 1 physical machine and 16 virtual machines.

#### openSUSE

A Linux distribution from Europe (Germany). The LEAP version is a fixed release, and the Tumbleweed version is a rolling release.

#### elementary OS

A distribution famous for its beauty. With many visual similarities to macOS, it seems to be **attracting many users who moved from macOS to Linux**.

#### Slackware

This is Slackware. If you're more fascinated by the history of Linux, I recommend Slackware. It's the oldest distribution. (Actually, there's only about a 3-month(?) difference from Debian. But apart from Debian and Slackware, most distributions released at that time have either died out or changed direction. I think Slackware and Debian are the only distributions that keep their original philosophy and form.) Strictly speaking, you could consider 'Boot Root' the first distribution, but Slackware is known to be definitely the first distribution in today's form. Its package managers are `pkgtool` and `slackpkg`. It's an attractive distribution with both a package manager where you check dependencies and compile yourself and one that enables modern dependency checking and binary installation. Like Debian, it has old packages. It's a distribution worth experiencing at least once.

### Others - LFS (Linux From Scratch)

A distribution that exists in the form of a book. It's actually ambiguous whether it should be called a distribution at all. You start with LFS from the [homepage](http://www.linuxfromscratch.org/). Once you complete LFS, you can do more advanced builds along various branches such as BLFS (Beyond LFS) and CLFS (Cross-compile LFS). It's generally considered excellent for learning about Linux systems. On a Linux-based host PC, you finish all the basic preparation for OS installation, such as GCC, and then enter the installation environment by rebooting. It seems to have many limitations for everyday use. If you need a broad and deep understanding of Linux systems, it seems like a distribution well worth referencing or studying.

I think LFS is purely for learning. No installation image (iso) file is provided; you install while reading the book. To set up the installation environment, you create a '/mnt/lfs/tools' directory and compile essential packages such as GCC there (because the configuration and versions of the host PC's GCC and other essential packages can't be trusted), then use the packages in /tools to install the base of /mnt/lfs (again, GCC and the remaining essential packages), and then compile any additional packages you need with the base system's GCC. Later, with BLFS and others, you can even compile Xorg and a desktop environment. Many distributions, Gentoo among them, were effectively born through the same or a similar process as LFS, BLFS, CLFS, and so on. If you're interested in making a distribution, studying and researching through LFS would be a fine idea. Since everything has to be compiled, like Gentoo it's heavily affected by the CPU.

## Closing

That's a brief introduction to the major distributions. I hope this post helps you choose a distribution to use. It's a personal wish, but I hope there will be more and more people in South Korea using Linux as their daily machine!
