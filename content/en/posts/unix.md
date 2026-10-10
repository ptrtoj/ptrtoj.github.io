---
title:  "A History of Unix"
date:   2019-01-01
url: 'unix'
---

## Preface

If you aren't particularly curious about the history of UNIX, you can jump straight to ‘[Types of Linux Distributions]({{< ref "distro" >}})’.

## UNIX

To explain the history of UNIX (hereafter 'Unix'), we have to go back to the mid-1960s. At the time, MIT, AT&T's Bell Labs, and GE (General Electric) were working on an experimental time-sharing operating system called Multics (hereafter 'Multics') for the GE-645 mainframe (the ancestor of today's operating systems, where multiple simultaneous users and multitasking are taken for granted). Disappointed by the size and complexity of Multics, Bell Labs gradually pulled out of the research, but Ken Thompson, Dennis Ritchie, Doug McIlroy, and Joe Ossanna, who remained interested until the end, did not. Judging that the project had simply been ahead of its time, the four wanted to keep experimenting on a somewhat smaller scale.

> UNIX® ?
>
> → UNIX is an official trademark of [The Open Group](https://www.opengroup.org/).
>
> It's used to refer to computer operating systems and tools that conform to the [Open Group Base Specifications Issue 7](https://pubs.opengroup.org/onlinepubs/9699919799/) (POSIX.1-2008 or IEEE Std 1003.1 - 2008).

In 1969, Ken Thompson found a little-used PDP-7 from DEC (Digital Equipment Corporation) at the lab. On this PDP-7, Thompson, Ritchie, and others implemented a hierarchical file system. This became the `/`, `/bin`, `/usr`, `/etc`, and so on that are still handed down in Unix-like operating systems. They didn't stop there and went on to develop computer processes, device files (under `/dev`), a command-line interpreter, an assembler, an editor, a shell, and more. Finally, Douglas McIlroy ported the TMG compiler-compiler to PDP-7 assembly, giving birth to the first high-level language on Unix. Thompson used this tool to develop the first version of the B programming language.

The first operating system born this way wasn't developed with Bell Labs' support, couldn't multitask, and didn't even have a name. It was simply the result of engineers working day and night to reach the goal they had set for themselves. Its first name was Unics (**Uni**plexed **I**nformation and **C**omputing **S**ervice; pronounced "eunuchs"), but Brian Kernighan later claimed to be the source of the name UNIX, adding the joke that "no one can remember anyway". Dennis Ritchie and Doug McIlroy also later credited Kernighan with the name.

The computer researchers, who now wanted to build an operating system that could multitask on hardware bigger than the PDP-7, learned that the legal department needed a document editor at the time (Bell Labs reportedly had lawyers and patent attorneys producing an enormous volume of patent-related paperwork for its many patent applications). They secured funding by promising that if they were bought a PDP-11, they would build fantastic software for editing documents.

In 1970, having received their first PDP-11/20, the researchers quickly created document formatting software known as roff and added a document editor as well. All of these programs were written in PDP-11/20 assembly. Bell Labs began using them to write patent-related documents conveniently. (roff soon evolved into troff.)

When other departments at Bell Labs bought DEC PDP-11s, they had already decided to use Unix rather than DEC's own operating system. As Unix kept growing in complexity and more and more departments and research teams wanted to use it, the need for a manual arose. Thus, on November 3, 1971, the Unix Programmer's Manual was created. This was the beginning of the `man` command still used today.

In 1973, Unix Version 4 was rewritten in C, a high-level language. Although assembly itself survived until Version 8, this introduced software portability (the ability to easily use software on computers with different instruction sets through compilation). The same year, with the release of AT&T's Unix Version 5, licenses were offered to educational institutions (companies were offered licenses starting with Unix Version 6 in 1976 - a commercial license reportedly cost US$20,000, or $96,190 in 2020 dollars). Versions 5 and 6, which began to be distributed externally, made possible the legendary book [Lions' Commentary on UNIX 6th Edition, with Source Code](https://www.amazon.com/dp/1573980137/).

## BSD

Originally, BSD was neither a copy of Unix nor even a markedly different version. It simply added a few extra helpful utilities on top of source code owned by AT&T.

In 1975, Ken Thompson took a break from Bell Labs and came to the University of California, Berkeley as a visiting professor. He helped install Unix Version 6 there and began porting Pascal to the system. Graduate students Chuck Haley and **Bill Joy** improved Thompson's Pascal and also developed an improved text editor, **ex**. When nearby universities became interested in these improvements, Joy put together the first Berkeley Software Distribution (1BSD) and first distributed it on March 9, 1978. (1BSD was more of an add-on to Unix Version 6, not an independent, complete operating system.)

2BSD was released in May 1979. It updated 1BSD and included two new programs. One was the **vi text editor** written by Joy, and the other was the C shell.

## The Unix Wars

### Prelude

AT&T created Unix, but by the 1980s, the Computer Systems Research Group at the University of California, Berkeley had come to lead non-commercial Unix developers.

In 1984, while AT&T was working to make System V the standard, some vendors got together and formed the X/Open Standard, which turned things into a full-blown conflict. The X/Open Standard was started to further unify Unix, with Sun Microsystems and others building a unified system under the lead of BSD Unix. The problem was that TCP/IP and other technologies that remain the foundation of networking today weren't implemented in System V but had been ported to BSD 4.2.

In 1985, AT&T established the System V Interface Definition (SVID) standard, and demanded that other Unix-based operating systems brand themselves based on System V.

In 1988, the IEEE established the POSIX specification, defining standards for compatibility between BSD and System V platforms. POSIX was mandated by the US government as the standard for various systems. Sun Microsystems joined the X/Open Standard, and some vendors who feared Sun's licensing got together separately and founded the Open Software Foundation (OSF). Not to be outdone, in 1988 AT&T created UNIX International (UI). AT&T then pursued an aggressive strategy, even partnering with Sun, to get ahead of the other distributions. (But then Sun went its own way with System V.4..)

### The Lawsuit

These quarrels continued right into the 1990s. BSD, however, didn't pay much attention and focused solely on developing its own operating system. It began adapting the operating system to the POSIX specification and cleaned up its networking code.

Then BSD developers including Donn Seeley, Mike Karels, Bill Jolitz, and Trent Hein left the University of California and founded Berkeley Software Design (BSDi). BSDi began selling a commercial version of BSD Unix built for the Intel platform. At this point, they promoted it as if it were free of AT&T code.

In 1992, AT&T finally sued BSDi and other BSD parties.

The University of California countersued.

Bill Jolitz left BSDi to develop 386BSD, and this 386BSD is the ancestor of today's FreeBSD, OpenBSD, and NetBSD.

In January 1994, the lawsuit ended in what amounted to a win for Berkeley. The terms of the settlement were that "AT&T would no longer sue Berkeley, its users, or its distributors in connection with the upcoming 4.4BSD release".

> **If 386BSD had been available when I started on Linux, Linux would probably never have happened.**
> **→ Linus Torvalds, 1993**

### The Aftermath

It goes without saying that the lawsuit slowed BSD's development. Beyond that, many potential customers, afraid of being directly dragged into further lawsuits or suffering pointless damage, stayed away, and BSD failed to gain much market share.

> Linux is on track to eventually own the x86 UNIX market.
>
> … (omitted) …
>
> In the foreseeable future, Linux rather than NT
> will be the biggest threat to SCO
>
> → [Confidential Microsoft memo](https://en.wikipedia.org/wiki/Halloween_documents), 1998

Meanwhile, in 1991, a Finnish graduate student named Linus Torvalds developed a kernel called Linux. Users began leaving BSD, noisy with lawsuits, to look at the Linux kernel. Linux began taking a large share, by what you could call a happy accident.

## Kinds of BSD

### Overview

Unlike Linux distributions, a BSD builds an entire operating system. In other words, it differs from the concept of a Linux distribution, which provides a package manager so you can download packages from various software makers and install them on top of the distribution. A BSD is distributed with everything in its official installable repository, including the packages judged to be stable after an active review process. (Think of Slackware.)

> **BSD 'distributions'??**
>
> Actually, the BSD camp dislikes the word 'distribution'. As noted below, a BSD develops and distributes a single, complete operating system. The BSD camp keeps asking that they be called 'operating systems'.

Among the BSDs still alive today, the big three brothers are the most famous: FreeBSD, NetBSD, and OpenBSD.

They split into three because their goals for the operating system differed. FreeBSD wanted to be a BSD suitable for ordinary users, NetBSD a BSD that runs on any hardware, and OpenBSD the most secure BSD, even at some cost in compatibility.

Chronologically, NetBSD was forked from 386BSD first. Almost at the same time, FreeBSD was developed by users dissatisfied with 386BSD's slow pace of development. A little later, Theo de Raadt, unable to resolve differences in goals and opinions with the NetBSD developers, forked OpenBSD.

Of course, these three aren't the only ones; there are various smaller BSDs. For example, DragonFlyBSD, GhostBSD, NomadBSD, and FuryBSD. They were made, respectively, to improve ease of use for ordinary users, to make installing a graphical environment easier, as a secure operating system to run from USB, and to improve ease of use for ordinary users.

And when developing operating systems specialized for servers, the BSDs, with their emphasis on stability, are actively used. For example, pfSense, TrueNas, OPNsense, and others are built from BSDs.

However, as many people find inconvenient, the BSDs have a few shortcomings as **everyday operating systems**. Currently the most notable are that you can't watch Netflix because DRM isn't supported (even though Netflix's own server infrastructure is well known to use FreeBSD...), and that support for newly released hardware is limited.

### Personal Opinion

### FreeBSD

#### Package Manager Command

Setting aside the oft-heard reasons such as 'BSD is the true UNIX lineage' and 'unlike Linux distributions, FreeBSD is distributed as a whole OS', what attracted me most was that FreeBSD's package management command is literally `pkg`. At some point I became disillusioned with all the flowery names, such as `apt`, `apt-get`, `dnf`, `yum`, `emerge`, `pacman`, `xbps-install`, `eopkg`, `zypper`, `nixpkg`, and `cards`, even though they all do similar or identical things, so I decided to stop distro-hopping by using the simplest package manager command as my criterion. (Alpine Linux's `apk` is also worth considering. However, it still seems to have many shortcomings as a desktop operating system. I often use it for container images.) OpenBSD's `pkg_add` command is nice too, but I think FreeBSD's `pkg` command, which goes one step further, is by far the most beautiful and most in line with the UNIX philosophy.

### NetBSD

Ordinary people rarely get to work with enough different hardware to need NetBSD, so I haven't used it for long.

### OpenBSD

First of all, 'security' isn't my number one goal in running my machines. It's true that OpenBSD has a strong appeal in terms of security. An additional appeal is that, unlike every other BSD or Linux distribution, it isn't interested in promoting itself to attract new users, and it doesn't force anyone to be friendly to users on its forums and elsewhere. It even tends to openly ignore questions that are too simple or easy.

In other words, OpenBSD is a BSD developed by a group of 'geeks' who share exactly the same interests. (Which may be why De Raadt, one of NetBSD's founders, was kicked out with the final sign-off of his fellow founders; December '94.) Because of this tendency, innovative file systems such as ZFS still haven't been ported.

> There are differing opinions internally about porting ZFS.
> As some Linux distributions also experience, a lack of manpower is cited as one reason.
> ZFS boasts an enormous amount of source code, and it's true that the current number of contributors is far from enough to meet OpenBSD's code review standards.
