---
title:  "How to Type Korean on Linux"
date:   2022-11-05
url: 'korean'
---

> I know `fcitx`, `scim`, and others work well too,\
> but I've been using `ibus` ever since I first started using Linux, so this guide is based on `ibus`.

## Before We Begin

### (1) If You Use Gnome

Gnome is now developed with `ibus` integrated. On most distributions, if you use the Gnome desktop environment, you can do the steps below through the Settings app. Check the Region & Language options!

### (2) Characters Show Up as Boxes?

When Korean characters on a web page show up as boxes, regardless of input, the cause is the font. Fix it by installing a font that can display Hangul, for example `noto-fonts-cjk`.

> **What is `CJK`?**
>
> `cjk` stands for **C**hinese, **J**apanese, **K**orean, and refers to packages or development environments related to these three Northeast Asian languages.

Selecting the entire font family, such as `noto-fonts`, `noto-fonts-cjk`, `noto-fonts-emoji`, and `noto-fonts-extra`, is also a good option, because web pages use all kinds of characters and symbols.

Depending on the distribution, packages such as `noto-fonts-extra` may not exist. Use your distribution's package manager to search for a keyword like `noto` and check the results.

What matters is installing a font that can display Hangul: a font that includes cjk (like `noto-fonts-cjk`), or fonts like `baekmuk` (Baekmuk) or `nanum` (Naver's Nanum).

## Installing ibus

`ibus-hangul`, which is what we actually need, depends on `ibus`, so running your package manager with just `ibus-hangul` as the argument usually installs `ibus` along with it.

```shell
// Debian family - Ubuntu, Mint
$ apt install ibus-hangul

// Red Hat family - Fedora, CentOS, Rocky, Alma
$ dnf install ibus-hangul

// Arch
$ pacman -S ibus-hangul

// Gentoo
$ emerge -av ibus-hangul
```

## /etc/environment

To use `ibus`, you need to edit the `/etc/environment` file.

```shell
// Open the file with your preferred text editor
$ sudo vim /etc/environment
```

Add the following lines.

```file:/etc/environment
GTK_IM_MODULE=ibus
QT_IM_MODULE=ibus
XMODIFIERS=@im=ibus
```

## Setting Up ibus

Running the command below opens a window.

```shell
$ ibus-setup
```

> A window will ask whether to start the daemon; choose `Yes`.
>
> A notification will ask you to set the variables you entered in `/etc/environment`; choose `Yes`.
> After that, you'll see a screen like the one below.

Click Input Method.

![How to type Korean 1](/korean-1.png)

Your screen may not have Korean - Hangul yet, and may only have English - English(US), or not even English.

> Note that as long as Korean - Hangul is added properly, you can type both English and Korean using the input method's internal language toggle key. There's no need to keep English.
>
> In other words, once you've successfully finished all the steps below, you can come back to this screen, click English, and remove it with the Remove button on the right.

Click Add on the right.

![How to type Korean 2](/korean-2.png)

A list of languages appears. Click the vertical **⁞** at the bottom.

![How to type Korean 3](/korean-3.png)

Type korea in the input box, as shown.

![How to type Korean 4](/korean-4.png)

If ibus-hangul was installed correctly, **Hangul with the Taegeuk symbol** should appear.

![How to type Korean 5](/korean-5.png)

Click Hangul, then press Add to exit.

![How to type Korean 6](/korean-6.png)

The input method has now been added.

To switch input methods with the right Alt or the Han/Eng key, click the Preferences button.

![How to type Korean 7](/korean-7.png)

You can choose how to switch languages within the Hangul input method.

> If you type Korean far more often than English on your system, checking Start in Hangul mode under the Etc section below is also a good option.

In the Hangul Toggle Key menu, click the Add button.

![How to type Korean 8](/korean-8.png)

My screenshot shows Super_L because I was taking a screen capture, but if you press the key you want to use as the Han/Eng key while the field is empty, it will be entered as Alt_R or Hangul.

Click OK to finish the setup.

![How to type Korean 9](/korean-9.png)

Now check that Korean input works, for example in a web browser.

> With the `st` terminal, I once had it work properly only after a reboot, not just after restarting `dwm`.
>
> If input still doesn't work even though you followed the steps as written, try logging out and back in, or rebooting entirely.

## Starting Automatically at Boot

If you use a graphical desktop environment, such as Gnome or Plasma, add `$ ibus-daemon -drxR` or the ibus program through an option like autostart.

> **Command explanation**
>
> - `-d`: Runs as a daemon in the background.
> - `-r`: If a daemon is already running, replaces it with this one.
> - `-x`: Runs the ibus XIM server.
> - `-R`: Restarts the panel and config processes even if they fail.

If you use a window manager, add `$ ibus-daemon -drxR` through `.xinitrc` or your window manager's autostart setting.

> For window managers
>
> Using `$ ibus-daemon -drxR` via autostart causes errors big and small. It's easier to run `$ ibus-daemon -drxR` yourself when you need Korean input, or to run the command through a keyboard shortcut.
