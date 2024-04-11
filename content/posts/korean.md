---
title:  "한글 입력 방법"
date:   2022-11-05
url: 'korean'
---

> `fcitx`, `scim` 등도 잘 작동하는 것으로 알고 있습니다만,\
> 저는 리눅스를 처음 접했을 때부터 지금까지 `ibus`를 사용중이므로 가이드는 `ibus` 기준으로 작성합니다.

## 들어가기 전에

### (1) Gnome의 경우

이제 Gnome은 `ibus`와 통합되어 개발됩니다. 대부분의 배포판에서, Gnome 데스크탑 환경을 사용하시는 경우라면, 아래 과정은 Settings 앱을 통해 진행하실 수 있습니다. Region & Language 옵션을 확인하세요!

### (2) 글자가 네모로 표시된다?

입력과 무관하게 한글이 포함된 페이지에서 한글에 해당하는 부분이 네모로 표시되는 문제는 폰트가 원인입니다. 한글 표기를 할 수 있는 폰트, 예를 들어, `noto-fonts-cjk` 등을 설치해서 해결합니다.

> **`CJK`란?**
>
> `cjk`는 **C**hinese, **J**apanese, **K**orean의 앞글자를 따 동북아시아 3국 언어 관련 패키지 혹은 개발 환경을 일컬을 때 쓰입니다.

`noto-fonts`, `noto-fonts-cjk`, `noto-fonts-emoji`, `noto-fonts-extra`와 같이 폰트 패밀리 전체를 선택하는 것도 좋은 옵션이 됩니다. 각종 웹 페이지에서 다양한 글자와 기호들을 사용하기 때문입니다.

일부 배포판에 따라 `noto-fonts-extra` 등이 없을 수 있습니다. 사용하시는 배포판의 패키지 매니저를 활용해 `noto`같은 키워드로 검색해 결과를 확인하시기 바랍니다.

중요한 것은 cjk가 포함된 폰트(`noto-fonts-cjk`처럼) 혹은 `baekmuk`(백묵), `nanum`(네이버의 나눔)처럼 한글을 표기할 수 있는 폰트를 설치하는 것입니다.

## ibus 설치

우리에게 직접 필요한 `ibus-hangul`이 `ibus`에 의존하므로 `ibus-hangul`만 인자로 제공해 패키지 매니저를 실행해도 보통은 `ibus`와 함께 설치합니다.

```shell
// Debian 계열 - Ubuntu, Mint
$ apt install ibus-hangul

// Red Hat 계열 - Fedora, CentOS, Rocky, Alma
$ dnf install ibus-hangul

// Arch
$ pacman -S ibus-hangul

// Gentoo
$ emerge -av ibus-hangul
```

## /etc/environment

`ibus`를 사용하기 위해서는 `/etc/envrionment` 파일 수정이 필요합니다.

```shell
// 선호하는 텍스트 에디터로 파일을 엽니다
$ sudo vim /etc/environment
```

아래의 내용을 추가해줍니다.

```file:/etc/environment
GTK_IM_MODULE=ibus
QT_IM_MODULE=ibus
XMODIFIERS=@im=ibus
```

## ibus 세팅

아래 명령을 수행하면, 창이 뜹니다.

```shell
$ ibus-setup
```

> 데몬을 실행하냐는 창이 뜰텐데 `Yes` 하시면 됩니다.
>
> `/etc/environment`에 입력한 변수들을 설정하라는 알림이 뜰텐데 `Yes` 하시면 됩니다.
> 그리고 나면, 아래와 같은 화면이 표시됩니다.

Input Method을 클릭합니다.

![한글 입력 방법 1](/korean-1.png)

여러분들의 화면에는 아직 Korean - Hangul이 없고, English - English(US)만 있거나 English조차 없을 수도 있습니다.

> 참고로 Korean - Hangul만 제대로 추가되어 있으면 입력기 내부 언어 변경키를 활용해 영어와 한글을 모두 입력할 수 있습니다. 굳이, English를 유지할 필요가 없습니다.
>
> 즉, 아래의 작업을 모두 성공적으로 마치신 분들은 다시 아래 화면으로 진입하셔서 English를 클릭하고 우측 버튼 중, Remove를 통해 제거하셔도 무방합니다.

우측의 Add를 누릅니다.

![한글 입력 방법 2](/korean-2.png)

언어 목록이 뜹니다. 하단의 세로로 된 **⁞** 를 클릭합니다.

![한글 입력 방법 3](/korean-3.png)

입력창에 보시는 것처럼 korea를 입력합니다.

![한글 입력 방법 4](/korean-4.png)

ibus-hangul이 제대로 설치되었다면 **태극 문양의 Hangul**이 표시되어야 합니다.

![한글 입력 방법 5](/korean-5.png)

해당 Hangul을 클릭하고 Add를 눌러 나옵니다.

![한글 입력 방법 6](/korean-6.png)

이제 입력기는 추가되었습니다.

우측 Alt 혹은 한/영키를 이용해 입력기를 변경하기 위해서 Preferences 단추를 누릅니다.

![한글 입력 방법 7](/korean-7.png)

한글 입력기 내부에서 언어 변경을 어떻게 할 지 선택할 수 있습니다.

> 참고로 시스템에서 한글 입력이 훨씬 잦으신 분들은 아래 Etc 항목에서 Start in Hangul mode를 체크해 두시는 것도 좋은 옵션입니다.

Hangul Toggle Key 메뉴에서 Add 버튼을 누릅니다.

![한글 입력 방법 8](/korean-8.png)

현재 제 이미지 상에는 화면 캡쳐를 하느라 Super_L이 찍혔는데 해당칸이 공란인 상태에서 한/영키로 활용하고자 하는 키를 입력하시면 Alt_R, 혹은 Hangul로 입력됩니다.

OK를 클릭하셔서 설정을 완료하시면 됩니다.

![한글 입력 방법 9](/korean-9.png)

이제 웹 브라우저 등을 활용해 한글 입력이 잘 되는지 확인하시면 됩니다.

> `st` 터미널의 경우, `dwm`을 껐다가 켜는 수준이 아니라 재부팅을 하고서 제대로 동작한 적이 있습니다.
>
> 표기된대로 따라왔음에도 불구하고 도저히 입력이 안되시면 로그아웃 이후 재 로그인, 혹은 아예 재부팅을 해보시는 것도 좋습니다.

## 추후 부팅 시 자동 실행 설정

그래픽 데스크탑 환경, 예를 들어, Gnome, Plasma 등을 사용하시면 autostart 등의 옵션을 통해 `$ ibus-daemon -drxR`, 혹은 ibus 프로그램을 추가합니다.

> **명령어 설명**
>
> - `-d`: 데몬으로 실행해 백그라운드에서 동작합니다.
> - `-r`: 이미 실행 중인 데몬이 있다면 이번 실행으로 교체합니다.
> - `-x`: ibus XIM 서버를 실행합니다.
> - `-R`: 실행에 실패해도 패널과 설정 과정을 재시작합니다.

윈도우 매니저를 활용하시는 분들은 `.xinitrc`나 본인 윈도우 매니저의 autostart 설정을 통해 `$ ibus-daemon -drxR`을 추가합니다.

> 윈도우 매니저의 경우
>
> autostart를 통한 `$ ibus-daemon -drxR`을 활용할 때 크고 작은 오류가 발생합니다. 한글 입력이 필요할 때, 직접 `$ ibus-daemon -drxR`을 실행하거나 키보드 단축키 세팅을 통해 해당 명령을 실행하시는 것이 수월합니다.]
