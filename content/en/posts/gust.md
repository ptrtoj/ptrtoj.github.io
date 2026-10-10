---
title:  "The Grand Unified Settings Theory"
date:   2022-11-02
url: 'gust'
---

## the **G**rand **U**nified **S**ettings **T**heory

This isn't an explanation of any particular distribution's features but my **opinions**; all my opinions on various programs are collected here.

![gust](/gust-setup.png)
**Gentoo + Dwm** 😊 (I run dwm/st/dmenu with nothing but source edits, no patchez)

## Criteria for Preference

### 0. Documentation

Basically, and personally, my broad preferences come down to how well something is documented.

For example,

- For distributions, ~~the old Gentoo wiki~~, the current Arch wiki
- For window managers, [`Xmonad`](https://xmonad.org/)
- For terminals, [`kitty`](https://sw.kovidgoyal.net/kitty/), which even has [separate documentation](https://sw.kovidgoyal.net/kitty/conf) just for its `config`
- For color themes, [Dracula](https://draculatheme.com/) or [Nord](https://www.nordtheme.com/)

I prefer things that are well documented like these.

### 1. Minimalism

Complexity breeds bugs. Unless it's a feature I particularly like, I prefer a simple and minimal approach wherever possible ([a fun, philosophical conference talk about simplicity](https://youtu.be/LKtk3HCgTa8)). That's simply because it fits my [philosophy](https://www.notion.so/b8c523c5c3414e8ca6a9d245e5c5cbce), not because it's unconditionally the right direction that everyone must follow.

### -. Licenses

I don't strictly require `GPL`, but I don't go out of my way to use proprietary software either.

Trying hard to stick to `GPL` + `MIT` + `BSD` + ~~`Apache`~~ licenses

### -. Consistency

Unless you really want to build everything around a different language, setups like

- LISP: `GuixSD` + `EXWM` + `Emacs`
- Haskell: (`NixOS`) + `Xmoand`
- Lua: (ANY) + `Awesome` + `NeoVIM`
- C: (ANY) + `DWM` + `ST` + `Surf`

are also possible, but it's not a criterion I pursue that far right now.

## Distributions.old

### 0. Before We Begin

*`systemd` doesn't affect my choice much. Honestly, I'm not very interested in the `init system` debate. I find the Editor War (`vim` vs `emacs`) or the indentation war (`Tab` vs `Spaces`) way more fun.*

Choosing a distribution has been so hard for me personally that I've applied criteria so peculiar and picky that I'm embarrassed to tell other people about them.

### 1. It must be an independent distribution

**A criterion I can't give up**

`Ubuntu` is out because it's based on `Debian`

`Mint` is out because it's based on `Ubuntu`

`Suse` is out because it was based on `RedHat` in the past

The `RedHat` family and `Fedora` are out because each was based on the other at some point

`Artix`, `Manjaro`, `EndeavourOS`, etc. are out because they're based on Arch Linux

→ Survivors: `Gentoo`, `Solus`, `Arch`, `Debian`, `Void`, `Venom`, `KISS`, `CRUX`, `LFS`, `NixOS`

### 2. The original creator must still be around

`Gentoo`: Daniel Robbins ran away and made `Funtoo`

`Solus`: the original creator bailed too

`Arch`: Judd Vinet got busy and bailed too

`Debian`: Ian Murdock's suicide

`Void`: the creator went silent, then tried to come back but was kicked out

→ Survivors: `Venom`, `KISS`, `CRUX`, `GUIX`, `LFS`, `NixOS`

### 3. Proprietary software support

For example, whether `Google Chrome` or `Visual Studio Code` is supported

Important, because I don't use Linux for ‘philosophy (Only Free and Open Source)’ reasons

→ Survivors: `NixOS` (`LFS`???, `CRUX`???)

→ Didn't meet the earlier criteria but worth noting

- `Arch` (has the `AUR`)
- `Gentoo` (the official repository has nearly everything)
- `FreeBSD` (same)

### 4. The package manager must have a clean name

Even I think this is going too far; anyway

→ Survivors

- `NixOS` : `nix` ← doesn't follow the FHS, so installing extensions for `vscode` etc. gets needlessly tricky ugh
- `LFS` has no package manager at all. Realistically unusable T_T

→ Didn't meet the earlier criteria but worth noting

- `FreeBSD` → `pkg` : but with `FreeBSD`, watching Netflix (no DRM support) is a problem
- `Alpine` → `apk` : but it's mainly meant for the cloud, which is a problem

## Distributions.new

![gust-best](/gust-best.png)

### First, let's talk about this one → `ArchLinux`

A family member had to be hospitalized, so I was packing my laptop, and after agonizing until that very morning, the one I reached for was Arch. Yep. You can install it in 10 minutes lol

- Pros
    1. It's a rolling release, so I don't have to agonize over whether to just do a fresh install at every point release
    2. `AUR`: so convenient when I want to sync `vscode` and `google-chrome`
    3. I know it inside out, so learning anything else is too much of a hassle;
- Cons ← the reasons I kept agonizing until the very end

    As pointed out under the old criteria

    1. The creator no longer maintains it ← You might wonder what the problem is, but personally I dislike it because the philosophy the distribution was originally designed with can gradually fall apart. Looking at how Volkerding of `Slackware` is still developing it as BDFL, you can guess that the reasons and philosophy behind `Slackware` are being preserved very well. The same goes for de Raadt of `OpenBSD`.
    2. ‘That’ criterion people might curse me for: the package manager name.. `pacman`.. hmm.. **`FreeBSD` has `pkg`!!**

### Then why not just use `FreeBSD`?

1. Have you seen the [802.11ac support status page](https://wiki.freebsd.org/WiFi/80211ac)?

Most WiFi chipsets aren't supported. In other words, if you want to use `FreeBSD` on a typical laptop, a WiFi dongle is a must. The problem isn't that they ‘won't’ develop it because of FOSS philosophy, but that development is ‘slow’. They're short on developers and actively need help.

2. WiFi isn't the only problem; even getting graphics working on a 2019 laptop was hard. I eventually failed.

Meanwhile, on `OpenBSD`, with no extra commands or package installs, it detected the APU, found and installed the firmware on its own, and `Xorg` came right up. Wow.

### Then why not just use `OpenBSD`??

The package manager names.. `pkg_add` to install, `pkg_info -Q` to search, `pkg_add -U` to update???? Not using it!

### With cons this obvious, why agonize at all?

Because it's **the ‘root (根本)’ UNIX.**

### What's wrong with `GuixSD`??

`GuixSD` is really good. On top of that, a package manager and operating system made with `guile`, a dialect of `scheme`, which is in the `lisp` family, wow, insane. All system configuration can be done in `lisp` lololol

Add `EXWM` and you can build a full Emacs environment lol

But!

1. As a GNU distribution, it has very strict FSF standards. Things you'd wonder about don't get ported either. For example, there's no `firefox`.
    1. Of course, you can install it with `non-guix` (`firefox`)
    2. Of course, you can install even more with `flatpak` (`chrome`, `vscode`)
2. And the kernel: it uses `linux-libre` with `GNU Hurd` as the end goal, and the `init system` isn't `systemd` (I wouldn't even ask for that), nor `openrc` or `SysV`, but `GNU Sheperd`
    1. Of course, the regular `linux` kernel can be installed with `non-guix`
    2. Of course, non-free firmware blobs can also be installed with `non-guix`

One way or another, it's a hassle to have to tinker separately to get things working that just work on other distributions (`Arch`, `Nix`)..

And I have a very minor extra complaint

1. `emacs` support is so good that the `config` files I put up here become hard to sync with the outside world. Sure, it doesn't matter if I just use `use-package`, but then why use `Guix`??…
2. As expected, user count is king. `ibus-hangul` v1.5.3, wow… As of August 30, 2022, when I wrote this, v1.5.4 was released on August 23, 2020, a whole two years earlier, wow.. ("Then why don't you bump the version yourself??" is a fair point. Agreed.)

### What's wrong with Nix??

Managing `vscode` extensions is rough, because as a functional package manager it doesn't follow the FHS.

Still, I have to note that `nix-shell` is unbeatable for quickly setting up development environments

Another touchy point is that it keeps warning you not to change `stateVersion`, whether it's `20.03` or `21.05`, for now. I understand the reason, but in a declarative distribution, I really dislike seeing a version variable fixed to the year and month of installation.

Option variable names changing (`exfat-utils` → `exfat`) is another thing; overall it seems like it'll be good to use once it matures more.

### Functional package managers and distributions

In conclusion, I think they'll take back the top spot over time.

## Distributions - So, What's the Verdict?

### Arch Linux

The `Python` of distributions

So good for quickly spinning something up and testing

Takes 10 minutes to install, tops

### Gentoo Linux

The `C` of distributions

### Void Linux

The `Haskell` of distributions

Very attractive, but nobody uses it, so it's a hassle to use (especially package version bumps)

### Crux Linux

???

### LFS, BLFS

The `ASM` of ~~distributions~~ books

### Ubuntu

Isn't this totally `Objective-C`? lololol

### Debian

The `C++` of distributions

### Slackware

The `Fortran` of distributions

### Fedora

The `Java` of distributions, part 1

### CentOS

The `Java` of distributions, part 2

### RHEL

The `Java` of distributions, part 3

### OpenSUSE

The `C#` of distributions

Still, it gets 0.01 extra points for having a rolling release

Leap for fixed release, Tumbleweed for rolling release

But they've fought among themselves over whether the Tumbleweed update command is `$zypper up` or `$zypper dup` lolol

### NixOS

This is the future, so I guess I could call it the `Rust` of distributions, wow

It's actually `Nix` itself lolol

### GuixSD

`Lisp` itself lolol

But disguised as `Guile` and lying in ambush

## Desktop Environments (DE)

### Gnome

There no longer seems to be any reason to prefer `Plasma` or `XFCE`

That said, `Gnome Terminal` ligature support, please.. I'm begging T_T `Konsole` has it T_T I don't want to install a separate terminal just for looks.. 🤔

The comfortable thing about preferring `Gnome` is that most distributions ship `Gnome` as the default, more or less lol

### Plasma

However, if you particularly like `Plasma`, your choices shrink. Or become a pro and run it on ArchLinux or Gentoo. Though, compiling `Plasma-meta` on Gentoo, just thinking about it, wow

### XFCE

It used to be an ancient, musty desktop environment, but after an unexpected large-scale update it became sleek lol

### Cinnamon/Mate

For grandmas

I don't mean it's bad. It's very comfortable. Nothing to tweak

Especially if you use one of the two via the Linux Mint editions, it's almost Windows-level lol

### Budgie

It came out of Evolve OS, the distribution was renamed Solus, and then Solus faded away, leaving behind only the masterpiece called `Budgie` lolol

## Key Bindings

Before getting into window managers, and especially editors, I want to talk about key remapping.

VIM's use of `h`, `j`, `k`, `l` and switching modes with `esc`, and even `~` coming to mean home on Unix, all [come from the ADM-3A terminal](https://catonmat.net/why-vim-uses-hjkl-as-arrow-keys).

In particular, if you use something like a programmable 40% keyboard, moving the current `TAB` to `ESC` and `CAPS LOCK` to `CTRL` can prevent [a repetitive strain injury (RSI)](https://en.wikipedia.org/wiki/Repetitive_strain_injury) like the so-called emacs pinky.

## Window Managers (WM)

The compositor is `picom`, of course. `compton` is now deprecated.

### DWM

- Minimal

### EXWM

`Emacs`!! `Lisp`!!

- No need to choose a terminal
- No need to agonize over `bash` vs `zsh` vs `fish`
- Of course, you'll most likely end up using `picom`
- Depending on your choice, you might use a status bar like `polybar`
- **But alas.. single-threaded..**

### BSPWM

- Really only does the window manager's job
- So it needs `sxhkd` for key bindings

### Xmonad

- `Haskell`!!

### i3

No gaps support

But plenty of people ask why you'd even use gaps. Then just use a DE. Lots of people use a WM because they don't want to waste screen space, so they don't understand gaps.

Anyway, for gaps you end up installing a fork called `i3-gaps`

### Sway

Hmm, so with `wayland` you don't need `picom` 🤔

Hmm, but the video driver side is a bit lacking 🤔

### Awesome

`Lua`!!

`NeoVIM` is `Lua` too!! United under Lua!!

## App Launcher

### Dmenu

Maybe the most successful child of Suckless;; (in the order `dmenu` > `dwm` > `st` > `surf`?)

### Rofi

Seems to be the most popular replacement for people who find `Dmenu` too plain

## Status Bar

### Polybar

Does everything

### Lemonbar

Haven't tried it yet

## Shell

### sh

Wow…

### Zsh

`oh-my-zsh`

prompt → `pure`

### Fish

There was the POSIX compatibility controversy lolol

### Csh

Smells like BSD, part 1

### Ksh

Smells like BSD, part 2

### Dash

Ubuntu's shell

### Bash

Smells like the default

## Editor

I no longer edit my config directly; I just use **the best-documented bundle**.

NeoVIM → `AstroNvim`, `LuanrVim`, etc.

Emacs → take a look at `Doom Emacs`, etc.

### Helix

`Rust` is the best!

### NeoVIM

**United under `Lua`!!**

Or so I said, but while happily using `neovim`, it occurred to me that I must really dislike fork projects. Even when I `ssh` into a server for the first time, the fact that I can fly around in the default `vim` doesn't change, so I gave up on `neovim` and just use `vim` if it's installed on the system.

- Reason 1: aliasing `vim` → `nvim` is just too much of a hassle. Especially for someone like me who keeps hopping between distributions and reinstalling things, it's even more of a hassle
- Reason 2: I have to additionally `alias` `vim` in the `root` environment, or install `vim` separately at the system level (for example, `vim` in `NixOS` `configuration.nix`, and `neovim` in `home-manager`, like this), which is a hassle
- Reason 3: in the end, I don't like the direction of `vim` development either, but I'm also uneasy about `neovim`, the community-developed one with `neo-` in front

### Vim

is everywhere.

That said, `${XDG_CONFIG_HOME}` support, please… grr

### Emacs

is everything, but not an editor

If you can delete a line with `dd` in vim, in emacs it's `C-a C-k` lolol

If you can insert a blank line below the cursor with `o` in vim, in emacs it's `C-e C-j` lolol

If you can insert a blank line above the cursor with `O` in vim, in emacs it's `C-a C-j C-p` lolol

([reference](https://stackoverflow.com/a/72583658))

My style is to keep plugins to a minimum, but for actual editing, maybe I need `evil`?

### Visual Studio Code

Money is king.

## Terminal

### st

Having to patch in even basic features is such a hassle..

### urxvtc

`rxvt-unicode` a.k.a `urxvt`, with daemon feature

### alacritty

Users love it, citing that it's written in `rust`

Ligature support, requested since January 5, 2017, still isn't there, yet the [issue thread](https://github.com/alacritty/alacritty/issues/50) was treated as spam and closed (?)

### kitty

Critics hate it, citing that it's written in `python`

There are issues with the remote server connection feature. The creator's reaction ([1](https://github.com/kovidgoyal/kitty/issues/2481), [2](https://github.com/kovidgoyal/kitty/pull/3544)) is, wow, not much different from `alacritty`, wow

### Termite

Haven't tried it

## Fonts

Whatever terminal you use, when some characters break into boxes, it's usually a font problem rather than a problem with the terminal itself (of course, in hardcore cases like `st`, it'd be a patch problem; but anyone reading this already knows that;)

So here are commands for a basic emoji test and Unicode test.

```shell
## Emoji
$ curl https://unicode.org/Public/emoji/5.0/emoji-test.txt

## Unicode
$ curl https://www.cl.cam.ac.uk/~mgk25/ucs/examples/UTF-8-demo.txt
```

- If some icons don't show up properly → use a patched font ([link](https://www.nerdfonts.com/))
- If some Unicode (foreign languages) doesn't show up properly → use DejaVu Fonts ([link](https://dejavu-fonts.github.io/))
- Keep the font you want as your main font and set the above as fallback fonts. ([link](https://wiki.archlinux.org/title/Font_configuration#Set_default_or_fallback_fonts))

### Nerd Fonts

[Nerd Fonts](https://www.nerdfonts.com/)

### Berkeley Mono

(Paid, [link](https://berkeleygraphics.com/typefaces/berkeley-mono/))

The base pack alone is good, but for some icon support I patched it with `nerd fonts` → `font patcher`: see `NFM` below

### BerkeleyMono NFM

(Personal, [Patched](https://www.nerdfonts.com/))

Can't upload it anywhere because of the license

Icons were added, but the base font ~~doesn't support some ligatures~~ has since been updated. So installing `Fira Code` below or `FiraCode NFM` from `nerd fonts` is even better

### Fira Sans, Fira Code

### Fantasque Sans Mono

### Font Awesome

### IBM Plex

Personally, I like its Korean font too (IBM Plex CJK KR)

### JetBrains Mono

### Powerline

## Color Theme

### Nord

Well.. but Nord is the pretty one lol ([link](https://www.nordtheme.com/))

If I use the Nord theme on ArchLinux, I install the following from the AUR lol

- Nord-KDE
- Nord-GTK
- Nord-Icons
- Nord-Konsole

### Solarized

by. Ethan Schoonover

the root of it all ([link](https://ethanschoonover.com/solarized/)) *~~but nobody seems to use it much these days lol~~*

[More about it](https://en.wikipedia.org/wiki/Solarized)

### Modus

light: `operandi`, dark: `viviendi`

by. Protesilaos Stavrou ([website](https://protesilaos.com/), [YouTube](https://www.youtube.com/c/ProtesilaosStavrou))

Ahh.. the root of `Emacs`? ([link](https://protesilaos.com/emacs/modus-themes))

### Dracula

([link](https://draculatheme.com/))

### Gruvbox

## E-mail Service

### ProtonMail

Try building your own email server at least once! You'll feel grateful to Gmail! lololol

Ah! Not a framework lol I mean setting up `Postfix` + `Dovecot` + `SpamAssassin` + `OpenDKIM` lolol

## Password Manager

- Once you start using a PM, I recommend turning off saving passwords, payment methods, and addresses in your web browser in particular.
- Using a PM doesn't make you safe; if anything, it concentrates the attack target in one place. So you must defend it at all costs with a long master password + 2FA + biometrics, etc.

### Bitwarden

- The first one I tried, and I use it because it's the best

### 1password

- Haven't used it

### LastPass

- Haven't used it

## 2FA Auth App

### Aegis

`GPL`

## Domain Service

### Namecheap

### Cafe24

If you want to buy Korean domains, especially `.kr`, `.co.kr`, etc., there's not much other choice

## VPS

### Vultr

Supports a Seoul region

### Linode

### Digital Ocean

Need to check whether it supports a Seoul region. It didn't when I was deciding between it and `Vultr`.

## CDN

### Cloudflare
