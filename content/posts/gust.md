---
title:  "통합 세팅 이론"
date:   2022-11-02
url: 'gust'
---

## the **G**rand **U**nified **S**ettings **T**heory

특정 배포판의 기능에 대한 설명이 아닌 **견해**, 각종 프로그램에 대한 견해는 여기에 모두 합쳤습니다.

![gust](/gust-setup.png)
**Gentoo + Dwm** 😊 (dwm/st/dmenu는 패치 없이 소스 수정만 해서 씀미댜)

## 선호도 기준

### 0. 문서화

기본적으로, 그리고 개인적으로, 큰 틀에서의 선호도는 문서화 정도에 따라 갈립니다.

예를 들어,

- 배포판의 경우, ~~옛 젠투 위키~~, 현 아치 위키
- 윈도우 매니저의 경우, [`Xmonad`](https://xmonad.org/)
- 터미널의 경우, [`kitty`](https://sw.kovidgoyal.net/kitty/), 심지어 `config` [문서는 따로 존재함](https://sw.kovidgoyal.net/kitty/conf)
- 컬러 테마의 경우, [드라큘라](https://draculatheme.com/)나 [노드(Nord)](https://www.nordtheme.com/)

처럼 문서화가 잘 되어 있는 것을 선호합니다.

### 1. 미니멀리즘

복잡성은 버그를 낳습니다. 제가 특별히 선호하는 기능이 아닌 경우, 가급적 심플하고 미니멀한 접근을 하는 것을 선호합니다([simple에 대한 재밌고 철학적인 컨퍼런스 영상](https://youtu.be/LKtk3HCgTa8)). 그게 그냥 제 [철학](https://www.notion.so/b8c523c5c3414e8ca6a9d245e5c5cbce)에 부합하기 때문이지 무조건 옳은 방향이므로 모두가 따라야 한다는 뜻이 아닙니다.

### -. 라이선스

반드시 `GPL`을 요구하지는 않지만, 그렇다고 Proprietary를 굳이 찾아서 쓰지도 않습니다.

`GPL` + `MIT` + `BSD` + ~~`Apache`~~ 라이선스 선에서 사용하려고 애 쓰는 중

### -. 통일성

굳이 다른 언어로 구성하고 싶은 마음이 있지 않고서야

- LISP: `GuixSD` + `EXWM` + `Emacs`
- Haskell: (`NixOS`) + `Xmoand`
- Lua: (ANY) + `Awesome` + `NeoVIM`
- C: (ANY) + `DWM` + `ST` + `Surf`

처럼 구성도 가능합니다만 현재는 그렇게까지 추구하지 않는 기준입니다.

## 배포판.old

### 0. 들어가기 앞서

`*systemd`는  크게 영향을 끼치지 않습니다. 사실, `init system` 논쟁에는 크게 관심 없습니다. 차라리 에디터 전쟁(Editor War : `vim` vs `emacs`)이나 들여쓰기 전쟁(`Tab` vs `Spaces`)이 더 꿀잼이라고 봅니다.*

배포판 선정하기가 개인적으로 너무 힘들어서 다른 사람들에게 얘기하기에 민망한 수준으로 특이하고 까다로운 기준을 적용해왔습니다.

### 1. 독립 배포판일 것

**포기 못하는 기준**

`Ubuntu`는 `Debian`에 기반하므로 탈락

`Mint`는 `Ubuntu`에 기반하므로 탈락

`Suse`는 과거에 `RedHat`에 기반했으므로 탈락

`RedHat` 계열과 `Fedora`는 서로 기반했던 시절이 있어서 탈락

`Artix`, `Manjaro`, `EndeavourOS`등도 아치 리눅스에 기반하므로 탈락

→ 살아남은 것 : `Gentoo`, `Solus`, `Arch`, `Debian`, `Void`, `Venom`, `KISS`, `CRUX`, `LFS`, `NixOS`

### 2. 처음 만든 사람이 남아 있을 것

`Gentoo`는 다니엘 로빈스가 가출해서 `Funtoo`를 제작

`Solus`도 첫 제작자 탈출

`Arch`도 쥬드 비넷 바빠서 탈출

`Debian`은 이안 머독의 자살

`Void`는 제작자 연락 두절 이후 컴백하려 했으나 쫓겨남

→ 살아남은 것 : `Venom`, `KISS`, `CRUX`, `GUIX`, `LFS`, `NixOS`

### 3. Proprietary 지원

예를 들어, `Google Chrome` 혹은 `Visual Studio Code` 등 지원 여부

‘철학(Only Free and Open Source)’을 이유로 리눅스를 쓰는 건 아니기 때문에 중요함

→ 살아남은 것 : `NixOS` (`LFS`???, `CRUX`???)

→ 앞선 기준에 부합하지 못했으나 주목할만한 것

- `Arch`(`AUR`이 있음)
- `Gentoo`(공식 레파지토리에 웬만한 게 다 있음)
- `FreeBSD`(마찬가지)

### 4. 패키지 매니저 이름이 깔끔할 것

진짜 내가 생각해도 오반데; 어쨌든

→ 살아남은 것

- `NixOS` : `nix` ←FHS 부합 안해서 `vscode`등 extension 설치가 쓸데없이 까다로워짐 ㅡㅡ
- `LFS` 패키지 매니저 자체가 없음. 현실적으로는 사용할 수가 없음 ㅠㅠ

→ 앞선 기준에 부합하지 못했으나 주목할만한 것

- `FreeBSD` → `pkg` : 다만, `FreeBSD`는 넷플릭스 시청(DRM 지원 안 함)이 문제
- `Alpine` → `apk` : 다만, 클라우드 용도가 주목적이라 문제

## 배포판.new

![[gust-best.png]]

### 일단 얘 얘기부터 → `ArchLinux`

가족 입원할 일 있어서 랩탑 챙기는데 당일 오전까지 고민하다 결국 손이 가는건 아치임 ㅇㅇ. 10분이면 설치 가능ㅋㅋ

- 강점

1. Rolling Release라 Point Release 때마다 차라리 Fresh Install 할지 고민 안 해도 됨
2. `AUR`: `vscode`, `google-chrome` 싱크하고 싶은데 너무 편함
3. 너무 속속들이 잘 알고 있어서 다른 거 배우는게 너무 귀찮;
    - 단점 ← 끝까지 고민하게 만든 이유

예전 기준에서 지적받았듯

1. 만든 사람이 이제 관리 안 함 ← 이게 뭐가 문제지? 하실 수도 있는데, 기존에 배포판을 설계했던 철학이 붕괴되어 갈 수 있어서 개인적으로 싫어합니다. `Slackware`의 Volkerding이 아직도 BDFL로써 개발하고 있는 것을 봐도 기존에 `Slackware`를 만들었던 이유와 철학이 매우 잘 유지되고 있을거라 추정할 수 있죠. 혹은 `OpenBSD`의 de Raadt도 그렇구요.
2. 남들이 들으면 욕할지도 모르는 ‘그’ 기준: 패키지 매니저 이름.. `pacman`.. 음.. `**FreeBSD`는 `pkg`임!!**

### 그럼 `FreeBSD` 쓰지 왜 안 씀?

1. [802.11ac 지원 상황 페이지](https://wiki.freebsd.org/WiFi/80211ac) 보셨나요?

어지간한 Wifi 칩셋은 지원이 안 됨. 즉, 어지간한 랩탑에서 `FreeBSD`를 사용하고자 한다면 Wifi 동글이 필수로 요구됨. 문제는 FOSS 철학 때문에 개발을 ‘안’ 하는 것이 아니라, 개발이 ‘더디다’는 것. 개발자가 모자라 도움이 적극적으로 필요한 상황임

1. 와이파이만 문제가 아니라서, 19년 출시 랩탑의 그래픽도 올리기 어려움. 결국 실패함

정작 `OpenBSD`에서는 별도의 명령어 입력이나 패키지 설치 없이 APU 확인하고 알아서 펌웨어 찾아다 설치해줘서 `Xorg` 바로 올라감. ㄷㄷ

### 그럼 차라리 `OpenBSD`를 쓰지 왜 안 씀??

패키지 매니저 이름.. 설치에 `pkg_add`, 검색에 `pkg_info -Q`, 업데이트에 `pkg_add -U`???? 안 씀!

### 그럼 단점 이렇게 명확한데 대체 왜 고민함?

그것은 **the ‘근본(根本)’ UNIX.** 이기 때문

### `GuixSD`는 뭐가 문제임??

`GuixSD` 정말 좋음. 게다가 `lisp` 계열인 `scheme` 의 일종(dialect)인 `guile`로 만든 패키지 매니저와 운영 체제라니 ㄷㄷ 미침 모든 시스템 설정(configuration)을 `lisp`으로 가능ㅋㅋㅋㅋㅋㅋ

여기에 `EXWM`으로 아예 풀 이맥스 환경 구축 가능ㅋㅋㅋ

근데!

1. GNU 진영 배포판 답게 아주 빡센 FSF 기준을 가지고 있음. 이게 왜?? 싶은 것도 포팅 안 됨. 예를 들어, `firefox`도 없음.
    1. 물론, `non-guix`로 설치 가능(`firefox`)
    2. 물론, `flatpak`으로 더한 것도 설치 가능(`chrome`, `vscode`)
2. 근데 커널도 `GNU Hurd`를 최종 목표로 일단 `linux-libre`를 사용, `init system`도 `systemd`는 바라지도 않지만 `openrc`, `SysV`도 아니고 `GNU Sheperd` 임
    1. 물론, 일반 `linux` 커널도 `non-guix`로 설치 가능
    2. 물론, non-free firmware blob도 `non-guix`로 설치 가능

이래저래해도 어쨌든 다른 배포판(`Arch`, `Nix`)은 그냥 되는 걸 실현하기 위해 별도로 만져야 되는 게 귀찮..

그리고 추가로 아주 사소한 불만이 있는데

1. `emacs` 지원이 너무 훌륭한 바람에 여기서 올린 `config` 파일은 외부와 sync하며 쓰기 어려워짐. 물론, 그냥 `use-package` 쓰면서 사용하면 상관 없는데 그러면 `Guix` 쓰는 이유가??…
2. 역시나 사용자 수가 깡패다. `ibus-hangul` v1.5.3 ㄷㄷ… 작성 시점 22년 8월 30일 기준, v1.5.4 출시가 20년 8월 23일로 무려 2년 전임 ㄷㄷ.. (”그럼 니가 버전 범프 해주지??” 는 좋은 지적임요. ㅇㅈ)

### Nix는 뭐가 문제임??

`vscode` 익스텐션 관리 ㄷㄷ함. 함수형 패키지 매니저를 쓰는 만큼 FHS에 부합하지 않기 때문.

그래도 `nix-shell`이 개발 환경 후딱 구축하기에 깡패라는 점도 꼭 적어야 함

또 하나의 예민 포인트이지만 `stateVersion`이 `20.03`이든 `21.05`든 현재로썬 수정하지 말라고 연신 경고하는 것도 애매합니다. 물론 이유는 납득을 했는데 선언식 배포판에서 버전 변수가 설치 당시 시점 연도와 월로 고정되있는게 참 보기 싫어요.

옵션 변수명 바뀌는 것(`exfat-utils` → `exfat`)도 그렇고 여러모로 더 무르익으면 쓰기 좋을 것 같습니다.

### 함수형 패키지 매니저 및 배포판들

결론적으로 시간이 지나면 최광자 자리를 탈환할 것 같아요.

## 배포판-하여튼 그래서 결론은?

### Arch Linux

`Python` 같은 배포판

후딱 올려서 테스트 하기 너무 좋음

넉넉잡고 10분이면 설치함

### Gentoo Linux

`C` 같은 배포판

### Void Linux

`Haskell` 같은 배포판

아주 매력적이지만 아무도 안 써서 쓰기 귀찮음(특히, 패키지 버전 범프)

### Crux Linux

???

### LFS, BLFS

`ASM` 같은 ~~배포판~~ 책

### Ubuntu

이거 완전 `Objective-C` 꼴 아님?ㅋㅋㅋㅋㅋㅋ

### Debian

`C++` 같은 배포판

### Slackware

`Fortran` 같은 배포판

### Fedora

`Java` 같은 배포판 1

### CentOS

`Java` 같은 배포판 2

### RHEL

`Java` 같은 배포판 3

### OpenSUSE

`C##` 같은 배포판

그래도 롤링 릴리즈 있으니 점수 0.01점 더 줌

픽스드로 Leap, 롤링으로 Tumbleweed가 있음

근데 Tumbleweed 업데이트 명령어로 `$zypper up`가 맞냐 `$zypper dup`가 맞냐로 자기들 내부에서도 싸웠던 전적이 ㅋㅋㅋㅋㅋ

### NixOS

이게 이제 미래형이니까 `Rust` 같은 배포판이라고 하면 되려나 ㄷㄷ

사실은 `Nix` 그 자체ㅋㅋㅋㅋㅋ

### GuixSD

`Lisp` 그 자체ㅋㅋㅋㅋㅋ

그러나 `Guile`로 변장하고 매복 중

## 데스크탑 환경(DE)

### Gnome

이젠 더 이상 `Plasma`나 `XFCE`를 선호할 이유가 없어보임

다만, `Gnome Terminal` Ligature 지원 좀.. 제발 ㅠㅠ `Konsole`은 되잖아 ㅠㅠ 굳이 외모 때문에 별도 터미널을 설치하기는 또 귀찮은데.. 🤔우

`Gnome`을 선호하면 속 편한게 어지간한 배포판은 default 느낌으로 `Gnome`을 가져다 씁니다 ㅋㅋㅋ

### Plasma

그러나, `Plasma`가 유독 마음에 든다면 선택지가 줄어들죠. 아니면 고수가 되어서 ArchLinux나 Gentoo에 올려도 좋구요. 다만, Gentoo에 `Plasma-meta`를 컴파일한다고 생각하면 ㄷㄷ

### XFCE

무지 오래된 냄새나는 그런 데스크탑 환경이었는데 뜬금없는 대규모 업데이트 이후 세련되게 변했습니다 ㅋㅋ

### Cinnamon/Mate

할머니용

안 좋다는 뜻 아님. 아주 편함. 건드릴 게 없음

특히, Linux Mint로 둘 중 하나 에디션을 사용하면 거의 윈도우즈 급 ㅋㅋㅋ

### Budgie

Evolve OS에서 꺼내놓더니 Solus로 배포판 이름이 바뀌었다가 Solus는 어디가고 `Budgie`라는 명작만 남김ㅋㅋㅋㅋ

## 키 바인딩

윈도우 매니저, 특히 에디터 얘기에 들어가기 전에 키 리맵 얘기를 하려고 합니다.

VIM의 `h`,`j`,`k`,`l`과 `esc`로 모드를 변경하는 것, 심지어 유닉스에서 `~`가 홈을 의미한게 된 것들은 [ADM-3A 터미널에서 유래](https://catonmat.net/why-vim-uses-hjkl-as-arrow-keys)합니다.

특히, 프로그래밍이 가능한 40% 키보드 등을 사용한다면 현재의 `TAB`을 `ESC`, `CAPS LOCK`을 `CTRL`로 옮기면 소위 얘기하는 emacs pinky 같은 [a repetitive strain injury (RSI)](https://en.wikipedia.org/wiki/Repetitive_strain_injury)를 방지할 수 있습니다.

## 윈도우 매니저(WM)

당연히 compositor는 `picom`이죠. `compton` is now deprecated.

### DWM

- 미니멀

### EXWM

`Emacs`!! `Lisp`!!

- 터미널 고를 필요 없음
- `bash` vs `zsh` vs `fish` 고민할 필요 없음
- 물론, `picom`은 쓰게될 확률이 높음
- 선택에 따라 `polybar`같은 스테이터스 바를 쓸 수도 있음
- **그러나 아아.. 단일 쓰레드..**

### BSPWM

- 진짜 윈도우 매니저 역할만 함
- 따라서 키 바인딩을 위해 `sxhkd` 필요

### Xmonad

- `Haskell`!!

### i3

는 gaps 지원 안 됨

근데 gaps 왜 쓰냐는 사람도 많음. 그럴 거면 그냥 DE 쓰지. 화면 자원 아깝다고 WM 쓰면서 gaps를 이해하지 못하는 사람들도 많더라구요.

어쨌든 gaps를 위해서는 `i3-gaps`라는 포크 프로젝트를 설치하게 됨

### Sway

흠 `wayland`에서는 `picom`이 필요 없다라 🤔

흠 다만 video driver 쪽 아쉬움이 있다라 🤔

### Awesome

`Lua`!!

`NeoVIM`도 `Lua`!! 루아로 대동단결!!

## App Launcher

### Dmenu

Suckless에서 제일 잘 난 아들이 아닐까;; (`dmenu` > `dwm` > `st` > `surf` 순?)

### Rofi

`Dmenu`가 심심하다고 느끼는 분들이 제일 많이 쓰는 대체품으로 보임

## Status Bar

### Polybar

다 됨

### Lemonbar

아직 안 써봄

## Shell

### sh

와…

### Zsh

`oh-my-zsh`

prompt → `pure`

### Fish

POSIX compatible 논란이 있었죠 ㅋㅋㅋㅋ

### Csh

BSD 냄새 1

### Ksh

BSD 냄새 2

### Dash

우분투네 쉘

### Bash

디폴트 냄새

## Editor

이제 config 직접 수정 안하고 그냥 **문서화 가장 잘 되어 있는 번들** 사용 합니다.

NeoVIM → `AstroNvim`, `LuanrVim` 등

Emacs → `Doom Emacs` 등 살펴보세요

#### Helix

`Rust` 최고!

#### NeoVIM

**`Lua`로 통일!!**

이긴한데, `neovim` 잘 쓰다가 생각난 게 제가 fork 프로젝트를 정말 비선호하나봅니다. 어차피 처음 만나는 서버에 `ssh` 하더라도 디폴트 `vim`에서도 날아다닐 수 있는건 바뀌지 않기 때문에 그냥 `neovim`을 포기하고 `vim`이 시스템에 그냥 설치되어 있으면 그거 씁니다.

- 이유 1: `vim` → `nvim` `alias` 도저히 귀찮. 특히, 저처럼 배포판 이것저것 돌아다니고 이것저것 계속 바꿔 설치하는 사람은 더 귀찮
- 이유 2: `root`환경에 추가로 `vim` 을 `alias`하거나 시스템 단위에서는 또 별도로 `vim` 설치 (예를 들어, `NixOS` `configuration.nix`에서는 `vim`을, `home-manager`에서는 `neovim`을 이 따위로)해야 해서 귀찮
- 이유 3: 결국 `vim` 개발 방향도 맘에 안들고 그렇다고 앞에 `neo-` 붙은 커뮤니티 개발 `neovim`도 꺼림칙함

### Vim

is everywhere.

다만, `${XDG_CONFIG_HOME}` 지원 좀… ㅂㄷㅂㄷ

### Emacs

is everything, but not an editor

vim에서 `dd`로 줄을 지울 수 있다면 emacs에선 `C-a C-k`입니닼ㅋㅋ

vim에서 `o`로 커서 아래에 공백열을 넣을 수 있다면 emacs에선 `C-e C-j`입니닼ㅋㅋ

vim에서 `O`로 커서 위에 공백열을 넣을 수 있다면 emacs에선 `C-a C-j C-p`입니닼ㅋㅋ

([참조](https://stackoverflow.com/a/72583658))

가능한 플러그인을 줄이고자 하는 스타일이지만, 정작 에디터 기능 관련해서는 `evil`이 필요할지도?

### Visual Studio Code

돈이 최고다.

## Terminal

### st

사실상 기본적인 기능도 패치해서 써야되는게 너무 귀찮..

### urxvtc

`rxvt-unicode` a.k.a `urxvt`, with daemon feature

### alacritty

사용자들은 `rust`로 쓰인 것을 이유로 들며 아주 선호함

2017년 1월 5일부터 요구되어 온 ligature 지원은 아직도 안되지만 [이슈 쓰레드](https://github.com/alacritty/alacritty/issues/50)는 스팸으로 처리, 닫아버린다(?)

### kitty

비판자들은 `python`으로 쓰인 것을 이유로 들며 아주 싫어함

원격 서버 접속 기능 이슈 있음. 이에 대한 제작자 반응([1](https://github.com/kovidgoyal/kitty/issues/2481), [2](https://github.com/kovidgoyal/kitty/pull/3544))이 ㄷㄷ `alacritty`와 크게 다를 건 없네요 ㄷㄷ

### Termite

안 써 봄

## Fonts

본인이 어떤 터미널을 사용하든지 일부 글자가 박스로 깨져서 나오는 현상은 터미널 자체의 문제라기보단 일반적으로 폰트의 문제인 경우가 많습니다(물론 `st`처럼 하드코어한 경우, 패치 문제겠지만; 이걸 읽는 분들이 그걸 모를리는 없고;)

따라서 기본적인 이모지 테스트와 유니코드 테스트를 해볼 수 있는 명령을 제공합니다.

```shell:
## Emoji
$ curl https://unicode.org/Public/emoji/5.0/emoji-test.txt

## Unicode
$ curl https://www.cl.cam.ac.uk/~mgk25/ucs/examples/UTF-8-demo.txt
```

- 일부 아이콘이 제대로 안 뜨는 경우 → 패치된 폰트를 사용([링크](https://www.nerdfonts.com/))
- 일부 유니코드(외국어)가 제대로 안 뜨는 경우 → DejaVu Fonts를 사용([링크](https://dejavu-fonts.github.io/))
- 메인으로 쓰고 싶으신 폰트를 둔 채로 fallback 폰트를 위의 것들로 지정하시면 됩니다.([링크](https://wiki.archlinux.org/title/font_configuration##Set_default_or_fallback_fonts))

### Nerd Fonts

[너드 폰트](https://www.nerdfonts.com/)

### Berkeley Mono

(유료, [link](https://berkeleygraphics.com/typefaces/berkeley-mono/))

기본 팩 만으로 좋지만, 일부 아이콘 지원을 위해 `nerd fonts`, → `font patcher`로 패치함: 아래 `NFM`

### BerkeleyMono NFM

(Personal, [Patched](https://www.nerdfonts.com/))

라이선스 때문에 어디 업로드해두지 못함

아이콘도 추가되었지만 기본 폰트에서 일부 ~~ligature를 지원하지 않음~~업데이트 됨. 따라서 아래 `Fira Code` 혹은 `nerd fonts`의 `FiraCode NFM`을 설치하면 더 좋음

### Fira Sans, Fira Code

### Fantasque Sans Mono

### Font Awesome

### IBM Plex

개인적으로 한글 폰트도 맘에 듬 (IBM Plex CJK KR)

### JetBrains Mono

### Powerline

## Color Theme

### Nord

뭐.. 근데 예쁜건 노드ㅋㅋㅋ([링크](https://www.nordtheme.com/))

ArchLinux에 Nord 테마를 쓰게 되면 AUR에서 아래를 설치합니다ㅋㅋ

- Nord-KDE
- Nord-GTK
- Nord-Icons
- Nord-Konsole

### Solarized

by. Ethan Schoonover

the 근본([링크](https://ethanschoonover.com/solarized/)) *~~이지만 요즘은 다들 잘 안 쓰는 것으로 보임ㅋㅋ~~*

[자세한 이야기](https://en.wikipedia.org/wiki/Solarized)

### Modus

light: `operandi`, dark: `viviendi`

by. Protesilaos Stavrou ([웹사이트](https://protesilaos.com/), [유튜브](https://www.youtube.com/c/ProtesilaosStavrou))

아아.. `Emacs` 근본?([링크](https://protesilaos.com/emacs/modus-themes))

### Dracula

([링크](https://draculatheme.com/))

### Gruvbox

## E-mail Service

### ProtonMail

개인 이메일 서버 구축은 한 번쯤 도전해보세요! Gmail에게 황송해질 겁니다! ㅋㅋㅋㅋㅋㅋㅋ

아! 프레임워크 말구요ㅋㅋ `Postfix` + `Dovecot` + `SpamAssassin` + `OpenDKIM` 세팅을 말하는 겁니다 ㅋㅋㅋㅋㅋ

## Password Manager

- PM을 사용하기 시작하면 특히 웹 브라우저의 비밀번호 저장, 결제 수단 저장, 주소 저장을 끄는 것을 권장합니다.
- PM을 쓴다고 안전해지는 것이 아니라 오히려 공격 타겟을 한 곳으로 집중시키는 노릇을 합니다. 따라서, 긴 기본 비밀번호 + 2FA + 생체 인증 등을 활용해 이 곳은 무조건 사수해야 합니다.

### Bitwarden

- 처음 써본건데 제일 좋아서 씀

### 1password

- 안써봄

### LastPass

- 안써봄

## 2FA Auth App

### Aegis

`GPL`

## Domain Service

### Namecheap

### Cafe24

한국 도메인 특히 `.kr`, `.co.kr`, 등 구매하려면 별다른 방법 없음

## VPS

### Vultr

서울 리젼 지원함

### Linode

### Digital Ocean

서울 리젼 지원되는지 확인해야함. `Vultr`랑 고민할 때는 지원을 안 했음.

## CDN

### Cloudflare
