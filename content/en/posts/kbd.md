---
title:  "The Righteous Keyboard Layout"
date:   2022-12-05
url: 'kbd'
---

## 🔧 Back to the Roots

Those of you who, like me, care about where things come from probably already know this: the reason Bill Joy's `vi` uses `h`, `j`, `k`, `l` as arrow keys, the reason `esc` is used to switch modes, and especially the reason Unix-like systems started abbreviating the home directory as `~` all trace back to the ADM-3A keyboard.

That's because this is the keyboard Bill Joy was using while he developed `vi`.

![adm-3a-full](/kbd-lsi-adm-3a-full.jpg)\
Source: [Lear Siegler's ADM-3A computer terminal's full keyboard](https://catonmat.net/why-vim-uses-hjkl-as-arrow-keys)

See the arrows above `H`, `J`, `K`, `L`? To move the cursor, you could press `H`, `J`, `K`, `L` together with `Ctrl`.

`Ctrl` sits to the left of `A`, and `ESC` sits to the left of `Q`.

A dedicated `:` (colon) key is located to the right of `0`. Just like the dedicated `;` (semicolon) key today, `:` (colon) had its own key.

If you look at the far right end of the top row, there's a `HOME` key, and the other character printed on that same key is `~` (tilde). That's how `${HOME}` came to be written as `~` for convenience.

![adm-3a](/kbd-lsi-adm-3a.jpg)\
Source: [Lear Siegler's ADM-3A computer terminal](https://catonmat.net/why-vim-uses-hjkl-as-arrow-keys)

## 👍🏻TAB → ESC

Losing `Tab` isn't just a problem when writing code. `Tab` is also useful for shell auto-completion.

However, as described below, there's a way to keep `Tab` alive by using the left `Ctrl` key, which ends up with nothing to do.

In Vim's insert mode, you can indent with `Ctrl-T` and outdent with `Ctrl-D`. ([source](https://news.ycombinator.com/item?id=21592886))

## 👍🏻CAPS LOCK → CTRL

Can you remember the last time you used `CAPS LOCK`? (~~MySQL~~) Following the roots, change the left `Caps Lock` to `Ctrl`.

It also makes `Emacs` much easier to use, lol.

## 👍🏻CTRL_L → TAB

~~It would be sad to get rid of it entirely~~ It's still useful for things like shell auto-completion, so I'll let it live...

## 🏁How?

([reference](https://www.emacswiki.org/emacs/MovingTheCtrlKey))

### Gnome

Use gnome-tweaks.

### Plasma

Under system settings → input devices → keyboard → advanced, there are options like ‘Caps Lock is also a Ctrl’.

### Others (1) - Xmodmap

Install and use `xmodmap`.

```file
!
! Swap Caps_Lock and Control_L
!

remove Lock = Caps_Lock
remove Control = Control_L
keysym Control_L = Caps_Lock
keysym Caps_Lock = Control_L
add Lock = Caps_Lock
add Control = Control_L
```

Load it from `.xinitrc` or similar. (It's usually already applied in the system `xinitrc` you copied.)

```bash
$ [[ -f ~/.Xmodmap ]] && xmodmap ~/.Xmodmap
```

### Others (2) - Xkbd

```bash
$ setxkbmap -option ctrl:swapcaps     # Swap Left Control and Caps Lock
```

## ➕Bonus - `Hyper` key

`Hyper` key is long gone. But if we move `ctrl` to `caps` (or `tab` whatever), there’s two ctrl on the left side of keyboard. So we can bring `Hyper` key back to life.

([ref](https://www.reddit.com/r/commandline/comments/4gusjx/comment/d2l0wpe/?utm_source=share&utm_medium=web2x&context=3): You can also put `.Xmodmap` in your `.xinitrc` with `xmodmap ~/.Xmodmap &`. I have it between the `setxkbmap` and `xcape` lines, and it has been stable.)

```file
clear      lock
clear   control
clear      mod1
clear      mod2
clear      mod3
clear      mod4
clear      mod5
keycode      37 = Hyper_L
add     control = Control_L Control_R
add        mod1 = Alt_L Alt_R Meta_L
add        mod2 = Num_Lock
add        mod3 = Hyper_L
add        mod4 = Super_L Super_R
add        mod5 = Mode_switch ISO_Level3_Shift
```

## Side Notes

### (1) QMK

Many of you who use custom keyboards probably use key remapping software ([QMK](https://github.com/qmk/qmk_firmware)). If you remap keys on a 40% keyboard in particular, why not try the setup described on this page? ~~(Gamers are excluded, since they can expect a lot of suffering.)~~

![planck](/kbd-planck_kbd.png)
40% Ortholinear ([source](https://ergodox-ez.com/pages/planck))

### (2) HHKB

There's also the Happy Hacking Keyboard, loved by many developers; on it, the `Caps Lock` position has been turned into `Control` outright. B.. but even `Delete` has moved to above `Enter` (Return);;

I bought and use the **[HHKB Pro Hybrid Type-S (Snow/Blank)](https://hhkeyboard.us/hhkb/pro-hybrid-type-s/sku/cg01000-307401)**.

It makes using `emacs` so comfortable. Whether it's `vim` or anything else, it really seems to be one of a kind.

![HHKB](/kbd-hhkb_kbd.png)
HHKB ([source](https://happyhackingkb.com/))
