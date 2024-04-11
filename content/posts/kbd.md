---
title:  "옳게된 키보드 레이아웃"
date:   2022-12-05
url: 'kbd'
---

## 🔧 근본

저처럼 근본 찾으시는 분들은 익히 아시겠지만 Bill Joy의 `vi`가 `h`,`j`,`k`,`l`을 방향키로 쓰게 된 이유, `esc`가 모드 변경에 쓰인 이유, 특히 유닉스 계열에서 홈 디렉터리를 `~`로 줄여 쓰기 시작한 원인은 모두 ADM-3A 키보드에 있습니다.

Bill Joy가 `vi`를 개발하던 당시 사용하던 키보드가 아래와 같기 때문이죠.

![adm-3a-full](/kbd-lsi-adm-3a-full.jpg)\
출처: [Lear Siegler's ADM-3A computer terminal's full keyboard](https://catonmat.net/why-vim-uses-hjkl-as-arrow-keys)

`H`,`J`,`K`,`L` 상단의 화살표가 보이시나요? 방향 이동을 위해서는 `Ctrl`과 함께 `H`,`J`,`K`,`L`을 입력할 수 있었습니다.

`A` 좌측에 `Ctrl`, `Q` 좌측에 `ESC`가 위치해 있습니다.

독립적인 `:`(콜론) 키가 숫자 `0` 우측에 위치합니다. 현재도 독립적인 `;`(세미콜론)처럼 `:`(콜론)도 독립 키였습니다.

최상단 행에서 가장 우측 끝에 보시면 `HOME`키가 있는데 여기에 같이 존재하는 특수키로 `~`(tilde)가 있습니다. 이로써 편의를 위해 `${HOME}`은 `~`로 표기하게 되었죠.

![adm-3a](/kbd-lsi-adm-3a.jpg)\
출처: [Lear Siegler's ADM-3A computer terminal](https://catonmat.net/why-vim-uses-hjkl-as-arrow-keys)

## 👍🏻TAB → ESC

코드 작성 시에서만 문제가 되는 것은 아닙니다. 쉘 자동 완성에서도 `Tab`은 유용하게 쓰이니까요.

그러나 아래에서 작성했든 기능이 없게된 왼쪽 `Ctrl`키를 이용해 `Tab`을 살려두는 방법도 있습니다.

Vim-insert 모드에서 `Ctrl-T`를 활용해 들여쓰기를 할 수 있고, 내어쓰기는 `Ctrl-D`로 가능합니다. ([출처](https://news.ycombinator.com/item?id=21592886))

## 👍🏻CAPS LOCK → CTRL

최근에 `CAPS LOCK`을 활용하신 기억이 나나요?(~~MySQL~~) 근본대로 좌측 `Caps Lock`을 `Ctrl`로 변경해줍니다.

`Emacs`를 사용하기에도 편해집니다 ㅋㅋ

## 👍🏻CTRL_L → TAB

~~그래도 아예 없애는 건 서운하니~~ 쉘 자동 완성 등에서 아직 유용한 경우가 많으니 살려는 드릴게..

## 🏁How?

([참조](https://www.emacswiki.org/emacs/MovingTheCtrlKey))

### Gnome

gnome-tweaks를 이용합니다

### Plasma

system settings → input devices → keyboard → advanced에 ‘Caps Lock is also a Ctrl’ 등의 옵션이 있습니다.

### 그 외 (1) - Xmodmap

`xmodmap`을 설치해 사용합니다.

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

`.xinitrc` 등에서 소환합니다. (보통 복사하신 시스템 `xinitrc`에서 적용되어 있습니다)

```bash
$ [[ -f ~/.Xmodmap ]] && xmodmap ~/.Xmodmap
```

### 그 외 (2) - Xkbd

```bash
$ setxkbmap -option ctrl:swapcaps     # Swap Left Control and Caps Lock
```

## ➕Bonus - `Hyper` key

`Hyper` key is long gone. But if we move `ctrl` to `caps` (or `tab` whatever), there’s two ctrl on the left side of keyboard. So we can bring `Hyper` key back to life.

([ref](https://www.reddit.com/r/commandline/comments/4gusjx/comment/d2l0wpe/?utm_source=share&utm_medium=web2x&context=3): You can also put `.Xmodmap` in your `.xinitrc` with `xmodmap ~/.Xmodmap &`. I have it between the `setxkbmap` and `xcape` lines, and it has been stable.)

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

## 여담

### (1) QMK

많은 분들이 커스텀 키보드를 활용하시면 키 리맵 소프트웨어([QMK](https://github.com/qmk/qmk_firmware))를 사용하실 겁니다. 특히, 40%키보드에서 키 리맵을 하신다면 특히 이 페이지에서 설정하는 방식으로 활용해보시는것은 어떨까 싶네요. ~~(게이머는 많은 고난이 예상되므로 제외)~~

![planck](/kbd-planck_kbd.png)
40% Ortholinear ([출처](https://ergodox-ez.com/pages/planck))

### (2) HHKB

많은 개발자들이 사랑하는 해피해킹 키보드도 있긴 합니다; 이쪽에선 아예 `Caps Lock` 자리가 `Control`로 변형되어 있습니다. 그.. 그런데 `Delete`까지 `Enter`(Return) 상단으로 이사;;

**[HHKB Pro Hybrid Type-S \(Snow/Blank\)](https://hhkeyboard.us/hhkb/pro-hybrid-type-s/sku/cg01000-307401)** 구입해서 사용하고 있습니다.

`emacs` 사용도 너무 편하고 좋네요. `vim`이고 뭐고 안가리는 전무후무 맞는듯 ㄷㄷ

![HHKB](/kbd-hhkb_kbd.png)
HHKB ([출처](https://happyhackingkb.com/))
