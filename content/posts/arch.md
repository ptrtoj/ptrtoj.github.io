---
title:  "아치 리눅스 설치 가이드"
date:   2017-11-07
url: 'arch'
---

## 서문

아치 리눅스를 써야 하는 이유가 무엇인지에 대한 질문이 등록되는 것을 자주 목격합니다. 저도 객관적으로 똑부러지게 '이런 점이 장점이고 이런 점은 약점이지만 이런 점에서 매력을 느낀다'라고 설명하지 못합니다 (물론, 제가 느끼는 강점과 매력은 분명하지만 이는 매우 주관적이기 때문입니다).

하지만, 제가 명확히 알고 있고 여러분들께 소개해 드릴 수 있는 것은  '**아치 리눅스의 설치 과정이 그렇게 당혹스러우리만큼 어렵지는 않다**'라는 것입니다.

일반적으로 윈도우 혹은 맥 진영에서 리눅스를 경험해 보고 싶은 신규 유저들에게 쉬운 배포판으로 우분투를 소개합니다. 충분히 이해할 수 있습니다. 설치 과정은 GUI로 표현되기 때문에 매우 편합니다. 하지만 그 과정들이 어떤 작업인지 이해하지 못하고 'Next' 만을 클릭하는 것을 '쉽다'고 표현하는게 올바른가에 대해서는 고민이 됩니다.

아치 리눅스는 설치 과정이 커맨드 라인 기반(CLI)입니다. 모든 것을 키보드를 이용해 명령하여 설치해야 합니다. 하지만 그 과정 덕분에 리눅스가 어떻게 빌드되는지에 대해서 정확히 이해할 수 있고, 내 시스템이 어떻게 구성되어 있는지를 파악하기 유리합니다 (더 깊은 이해는 [Gentoo]({{< ref "gentoo" >}}), [CRUX](https://crux.nu/) 혹은 [LFS](https://www.linuxfromscratch.org/)를 통해 성취할 수 있습니다).

제가 처음 아치 리눅스에 관심을 가지게 되었을 때에도 설치 과정이 불편했습니다. 여기서, '불편했다'라는 것은 번역되어 있는 자료의 부재가 크게 작용했습니다. **영어로된 자료에만** 의존해 설치해야 한다는 점이 힘들었습니다. 설치 당시 '한글로 된 양질의 자료가 필요하다'라고 절감했습니다.

한글 아치 리눅스 위키 페이지의 설치 가이드를 추천하실지 모르겠습니다. 매우 훌륭하게 정리되어있고, 아래에서 소개할 설치 과정도 그 페이지를 기반으로 합니다. 다만, 초급 사용자가 사용하는 데에 있어서도 그 페이지로 충분하다는 데에는 동의할 수가 없었습니다. 저는 아치 리눅스가 한국에서 더욱 확산되고, 특히 '**초보 사용자의 설치 환경에 큰 도움을 주고 싶다**'라는 데에 방점을 찍었습니다.

![아치 리눅스 로고](/arch-logo.png)

KISS, 즉, Keep it Simple, Stupid! 이것은 유닉스 계열의 정통 철학일 뿐만 아니라 아치 리눅스 진영에서도 핵심적으로 추구하는 철학입니다. 아치 리눅스는 단순함을 추구합니다. 흥미로운 점은 그 단순함을 위해서 얼마나 많은 복잡함을 응축해야 하는가 입니다. 우측의 스크롤 바를 보시면 당황스러우실 수 있습니다. '초보자를 위해 글을 작성했다고 제창하면서 얼마나 자세히 적어두고 있는거냐'라고 반문하실 수도 있습니다.

반전은 이 스크롤 바를 읽어 내려가면서 직접 설치해야 하는 패키지의 수는 오히려 5개로 단순화했다는 점입니다. 물론 이는 수동으로 입력하여 설치하는 패키지(*dependency 제외*)를 뜻합니다. 그 5가지는 `base`, `xorg-wayland`, `gnome`, `google-chrome`, `ibus-hangul`입니다.

보신 바와 같이 그 5개의 패키지 중에서도 한 개는 실제 사용에 필수가 아니지만, 여러분들이 아치 리눅스를 사용하는데 유용할만한 지식(AUR 사용법)까지 전달 드리기 위해 설치하는 것(`google-chrome`)입니다.

이 한 페이지를 통해 독자분들은 **기본 시스템 구축**과 관련된 분야(파티션, 포맷, 파일시스템 테이블, 부팅 과정과 부트 로더의 역할), **데스크탑 환경** 구축, 웹 브라우저 설치 과정을 통한 **AUR 학습** 그리고 아치 리눅스에서 **한글**을 사용하는 것이 얼마나 쉬운지 느끼실 수 있습니다.

**그 단순함을 성취하기 위해 모순적이게도 오히려 상당히 많은 내용을 담을 수 밖에 없었습니다.**

## **설치 전 주의 사항**

**공식 아치 리눅스 설치 가이드는 [단 한 가지](https://wiki.archlinux.org/index.php/Installation_guide) 버전만 존재합니다.**

그 외 모든 가이드는, 이 페이지를 포함, **특정 목적**(사용자 편의/작성자 편의/강의 목적)을 위해 편집된 내용입니다. 그러므로, 우선 공식 위키의 내용을 이해한 후, 이 가이드를 보면서 입맛에 맞게 수정/적용하시는 것을 권장하는 바입니다.

본 글에서 설명드리는 내용을 모두 이해하신 분들은 2021년 4월 1일부터 [Arch Install](https://wiki.archlinux.org/title/Archinstall)이라는 이름으로 배포되는 설치 도움 프로그램을 활용하셔서 전보다 수월히 설치하실 수도 있습니다!

## 설치 준비

설치 이미지 파일(`iso`) 다운로드 방법은 아래와 같습니다.
(참조: [카이스트 미러](http://ftp.kaist.ac.kr/ArchLinux/iso/latest/))

```
카이스트 미러 접속 > 목록 중 > archlinux-YYYY.MM.DD-x86_64.iso 를 클릭하여 다운로드
```

### Windows에서

[Rufus](https://rufus.ie/en/)를 이용해 제작합니다. 자세한 내용은 생략합니다.

### Linux에서

리눅스 시스템에서 USB 마운트 이후, 아래의 명령을 실행합니다.

```console
# lsblk
```

> **명령어 설명**\
> `lsblk`: **l**i**s**t **bl**oc**k** device → 블록 디바이스 목록을 출력합니다.

결과 출력을 통해 내 USB 디바이스 장치명을 확인합니다.

보통, 용량을 통해서 확인합니다.
`sdb`, `sdc`처럼 사용자에 따라 다른 이름이 표시됩니다.

아래 명령어를 통해 USB를 포맷하고 새로운 iso를 작성할 준비를 합니다.

주어진 예시 명령어의 `/dev/sdb`에서 `sdb`부분을 본인 환경에 맞게 입력해야 합니다.
위에서 확인한 본인 USB 장치의 이름을 넣어줍니다.

> **아래 명령어는 USB 내부의 모든 파일을 삭제합니다.**\
> **중요한 파일이나 삭제가 용인되지 않는 파일은 반드시 백업을 하시고 진행하시길 바랍니다.**

```console
# sudo wipefs --all /dev/sdb
```

아래 명령어를 통해 준비된 USB에 iso 파일을 구워줍니다.

`if` 이후에 `(iso 파일 다운로드 위치)/archlinux-(날짜)-x86_64.iso`를 본인 환경에 맞게 입력합니다.

해당 경로에 아치 리눅스 iso 파일이 하나밖에 없다고 가정할 경우, `archli`와 같이 앞 부분을 입력한 후, `Tab` 키를 이용해 자동 완성이 가능합니다.

다만, `Enter` 입력 전에 **반드시 자동 완성된 파일 이름이 원하는 결과로 입력되었는지 확인**하시기 바랍니다.

아래 명령어에서 `of`부분 이후에는 위에서 준비한 USB 경로를 **본인 환경에 맞는 것**을 입력합니다.

```console
# sudo dd bs=4M if=~/Downloads/archlinux.iso of=/dev/sdb status=progress && sync
```

> **명령어 설명**
>
> - `dd`: `dd`라는 이름의 프로그램을 이용합니다. (과거 UNIX 시절에는 ‘잘못 사용하면 디스크를 사용 불가능하게 만드는 프로그램’이라는 의미로 '**D**isk **D**estroyer'라는 별명이 있었습니다.)
> - `bs`(**b**lock **s**ize): 블락 크기는 4M (생략 가능)
> - `if`(**i**nput **f**ile): 인풋 파일 (지금은 아치 리눅스 iso 파일)
> - `of`(**o**utput **f**ile): 작성할 대상인 아웃풋 디바이스의 경로

이제 재부팅해줍니다.

재부팅 도중에 본인 메인 보드에 맞는 바이오스/UEFI진입 키를 이용합니다.
제 경우는 `F2` 입니다. 보통은 `F2`, `F10`, `F11`, `Del` 등입니다.

바이오스/UEFI 진입 키를 통해 부팅 할 미디어 중, 방금 제작한 USB를 찾아 선택합니다.

이 USB를 통해 정상적으로 부팅이 이루어지면 리눅스 부팅 중과 부팅 후의 선택 화면에서는 일반적으로 디폴트 값으로 가리키고 있는 것을 통해 부팅하시면 됩니다 (`Boot Arch Linux`혹은 `x86_64`같은 것이 포함되어 있음).

또, 키 맵을 선택하라는 알림이 등장할 수도 있는데 디폴트가 `US`로 선택되어 있는 경우가 많으므로 그냥 `Enter`를 입력하시면 됩니다.

> **설치 미디어 USB 부팅 자체가 안되는 경우!**
>
> 설치화면 진입 자체가 안되는 문제는 BIOS/UEFI 세팅 문제일 가능성이 큽니다.\
> **`secure boot`가 사용 안함(`Disabled`)으로 되어있는지 확인**하시기 바랍니다.\
> `fast boot`, 혹은 `CSM 모드`(`LEGACY/BIOS MODE`)에서 발생하는 오류일 수도 있습니다.\
> **추천드리는 것은 모두 사용 안함(`Disabled`)입니다.**

## 베이스 시스템 설치

### UEFI 여부 확인

우선, UEFI로 설치해야 하는지 그리고 UEFI로 설치가 가능한지 확인하도록 하겠습니다.

아래 파티션 계획 파트에서 다시 말씀드리겠지만,
UEFI로 부팅된 것이 맞고 UEFI로 설치할 생각인 경우의 파티션과
그렇지 않은(BIOS로 부팅된 경우) 파티션이 다릅니다.

UEFI의 경우, 권장 설치 설정은 디스크 타입을 `GPT`로 그리고 반드시 `ESP 파티션`을 가질 것을 요구합니다.

이 가이드는 대부분의 일반 사용자 부팅 환경이 UEFI이므로 UEFI 설치용 파티션만을 설명합니다.

확인은 아래 명령어를 통해 할 수 있습니다.

```console
# ls /sys/firmware/efi
```

> **명령어 설명**
>
> `ls`: **l**i**s**t. 주어지는 경로(`/sys/firmware/efi`) 내부 파일들을 보여줌

결과에 `efivars`라는 디렉터리가 보이면 UEFI로 설치하실 수 있습니다.

### 인터넷 연결 확인

인터넷 연결 상태를 확인합니다.

```console
# ping -c 3 www.google.com
```

> **명령어 설명**
>
> - `ping` 명령어를 통해 뒤에 명시된 서버에 핑을 보냅니다.
> - `c`이후의 숫자 3을 다른 숫자로 변경해 사용하셔도 무방합니다.
> - `c`옵션을 안 넣었거나 숫자를 빼먹어서 `ping`이 계속 실행될 경우 당황하지 마시고, `ctrl + c`(`SIGINT`; 시스템 인터럽트 시그널)를 이용해 작동을 중단하실 수 있습니다!

무선 환경의 경우, 아치 리눅스 설치 프로그램은 [iwctl](https://wiki.archlinux.org/title/Iwd#iwctl) 유틸리티를 동봉합니다.

```console
#  iwctl
[iwd]#device list
// 본인 디바이스명 확인

[iwd]#station YOUR_DEVICE scan
[iwd]#station YOUR_DEVICE get-networks

// 공개 네트워크(비밀번호 없음)의 경우
[iwd]#station station YOUR_DEVICE connect NETWORK_SSID

// 비밀번호 아는 비공개 네트워크의 경우 'ctrl+d'로 나와서
#  iwctl --passphrase NETWORK_PASSWD station YOUR_DEVICE connect NETWORK_SSID

```

위 과정을 통해, 네트워크에 연결된 이후, 제대로 작동하는지 `ping`으로 연결 상태를 확인합니다.

### 파티션 계획

이 과정은 **드라이브 구획을 나누고 계획을 잡아두는 것**으로 그 계획에 맞는 포맷은 다음 단계에서 진행 됩니다.

파티션은 대용량 저장 장치를 어떤 구획으로 나누어 사용할지에 대해서 정하는 순서입니다.\
일반적으로는 세 파트로 나눕니다.\
각각 `부팅을 위한 구획`, `데이터들이 저장될 구획`, 그리고 `리눅스 스왑`으로 쓸 구획입니다.

가장 먼저, `부팅을 위한 파티션` 관련해서 UEFI로 부팅했고, 설치하려는 경우 `ESP`로 불리는 **E**FI **S**ystem **P**artition을 계획해야 합니다. ESP를 계획하지 않으면 UEFI 환경에서 부팅을 설정해주는 것이 굉장히 까다로워집니다.

ESP는 ‘**타입**: EFI System Partition’으로 설정되어 있고 ‘**포맷**: FAT 파일 시스템’으로 된 파티션입니다. ESP는 `/boot`디렉터리에 마운트 됩니다.

더 깊이 관심 있으신 분들은 `/boot`에 마운트 하는 것과 `/efi`에 마운트 하는 것 간에 어떤 차이가 있는지 검색해보시면 좋습니다[[읽을거리](https://wiki.archlinux.org/title/EFI_system_partition#Typical_mount_points)].

두 번째 구획인 데이터 저장 구획은 단순하게 `Linux File System`이라는 타입으로 사용할 예정입니다. 이 두 번째 파티션은 `/`, 즉 `root` 디렉토리에 마운트 되어 모든 데이터가 저장될 구역이라고 생각하시면 됩니다.

마지막 스왑 파티션은 이름처럼 `Linux Swap`이라는 타입을 붙이면 되겠습니다.

(본인 사용 환경, 혹은 입맛에 맞게 추가적인 파티션을 계획하셔도 좋습니다. 별개로 계획된 `/home` 등)

> **`SWAP` 굳이 필요? 사이즈 굳이 그만큼?**
>
> 리눅스 스왑이란 램 환경이 모자랄 때 하드디스크의 스왑으로 설정된 부분에 자료를 로드해두고 램처럼 활용하는 것을 의미합니다.
>
> 일부 사용자는 "스왑이 사용되는 순간부터 이미 램이 모자란 것과 같은 것이므로 스왑을 쓰기보다는 차라리 하드웨어를 업그레이드 하겠다"라고 하며 스왑을 사용하지 않는 경우도 있습니다.
>
>LFS 유저들 중 이런 견해가 많이 보입니다. 공식 매뉴얼에서도 이렇게 기재해두었습니다. [LFS 공식 매뉴얼](http://www.linuxfromscratch.org/lfs/view/stable/) > 2.4.1.2. The Swap Partition > “Swapping is never good. (이하 생략)”
>
> 반대로, 4G 넘는 램을 갖고 있더라도 시스템에서는 실제 램 사이즈와 관계없이 4G 정도의 스왑을 구획하는 [경우](https://opensource.com/article/19/2/swap-space-poll)도 많습니다.
>
> **`hibernation`** (노트북 등의 장치에서 덮개를 닫아 수면 상태로 보내는 것) **등의 기능을 위해 램 사이즈 만큼의 스왑 파티션을 꼭 챙겨야 한다는 [의견](https://forums.gentoo.org/viewtopic-t-1069006.html)도 있습니다.**
>
> 이 문서에서는 가장 널리 쓰이는 설정대로 진행합니다.

아래 명령어를 통해 내 드라이브로 어떤 것들이 있는지 확인해보겠습니다.

```console
# lsblk
```

결과 출력에 `sda`, `sdb` 혹은 `hda`와 같은 대용량 저장 장치가 보이실 겁니다.\
대용량 장치가 여러 개일 경우 `sda`,`sdb`,`sdc`순으로 출력 됩니다.\
`hda`는 기존의 IDE 방식 커넥션으로 연결된 하드디스크, `sda`는 최근 SCSI/SATA방식으로 연결된 HDD 혹은 SSD입니다.

**부팅을 위해 마운트한 USB가 아닌지 확실히 확인하셔야 합니다**. 확인은 보통 용량으로 하면 큰 문제가 없습니다.

가장 일반적인 `sda`라고 가정하고 계획을 잡아보겠습니다.\
이번 설치 과정에서는 총 세 구획으로 쪼개려고 합니다.\
쪼개고 나면 `sda1`,`sda2`,`sda3`으로 나누어집니다.\
만약 `sdb`였다고 하면 `sdb1`,`sdb2`,`sdb3`이 되겠죠?

보통 `/dev/sda1`, `/dev/sda2`처럼 경로명을 표기하는데 앞의 `dev`는 'device'를 뜻합니다.

아래처럼 계획을 잡아보겠습니다.

> 계획은 사람마다 다를 수 있지만 본인 파티션의 **몇 번째가 어떤 것**으로 쓰일지 제대로 기억하고 계셔야 합니다. 특히,**부팅 파티션/스왑 파티션/루트 파티션**으로 정확히 개념을 잡고 그게 각각 **1,2,3** 중에 어느 것으로 쓰일 것인지 기억하시고 아래 예제를 따라오셔야 합니다.

| 적용 파티션 | 타입                 | 코드명   | 사이즈      | 용도             |
| ----------- | -------------------- | -------- | ----------- | ---------------- |
| /dev/sda1   | EFI System Partition | EF00     | 512M        | ESP(/boot)       |
| /dev/sda2   | Linux Swap           | 8200(82) | 4G          | 스왑파티션(swap) |
| /dev/sda3   | Linux FileSystem     | 8300(83) | 나머지 용량    | 루트파티션(/)    |

> **파티션 사이즈?**
>
> ESP, 흔히 말하는 '부트 디렉터리'에 마운트 시킬 파티션 용량 그리고 스왑 용량 관련해서 정답도 없고, 추천하는 크기도 다 다르다는 점은 충분히 알고 있습니다. 다만 **초보자의 관점에서 혼란스러움 없이 진행하기 위해서 우선은 위와 같이 가장 기본이라고 할 수 있는 용량으로 진행**해 보도록 하겠습니다. 설치를 하고 계시는 여러분이 이미 각종 고급 설정에 익숙하시면 충분히 수정해서 설치하셔도 됩니다.

### **cfdisk**

(`gdisk` 활용 파트 삭제하였음. `cfdisk`가 아닌 다른 프로그램을 활용하실 분들은 별도 문서를 참조하시기 바랍니다.)

> **아래 명령어들은 하드디스크 내부의 모든 파일을 사용할 수 없게 만듭니다.
>중요한 파일이나 삭제가 용인되지 않는 파일은 반드시 백업을 하시고 진행하시길 바랍니다.**

아래 명령어에 디스크 경로가 포함되는데 **본인 환경에 맞는 것**을 입력하시면 됩니다.

```console
# cfdisk /dev/sda
```

UEFI 설치를 위해서는 라벨이 `GPT`라고 상단에 표기 있는지 확인합니다. `MBR` 혹은 `DOS`가 아니어야 합니다!

만약 처음에 물어보면 `GPT`를 선택해주시면 됩니다.

물어보는 경우, `Primary`로 진행합니다.

표에 이미 있는 파티션이 출력되는 경우, 기존 파티션들을 한 줄씩 내려가면서 `DELETE`해줍니다. (키보드의 화살표 방향키를 이용합니다.)

그리고 나서 모든 용량이 `FREE SPACE`가 되면 계획한 파티션을 만들겠습니다.

처음 만들 파티션은 `/dev/sda1`, EFI System Partition, 512MB입니다.

따라서,

```
NEW 선택(Enter)
용량 크기를 입력: 512M
물어보는 경우, 앞으로 생성할 모든 파티션도 마찬가지로: Primary 선택
우측 방향키를 통해 TYPE으로 커서를 옮긴 후: Enter
보통 맨 위에 있는: EFI System을 선택
```

두번째 만들 파티션은 `/dev/sda2`, Linux swap, 4GB였습니다.

방향키를 아래로 내려서 `free space`를 향하게 하면 아래에 `NEW` 버튼이 다시 활성화된 것을 볼 수 있습니다.

```
NEW
용량 크기를 입력: 4G
TYPE: Linux Swap 찾아서 선택
```

이제 `/dev/sda3`도 만들어보겠습니다.

```
NEW

용량 크기는 디폴트로 출력되는 크기가 남아있어야 할 최대 용량인지 확인하고: Enter

TYPE도 새로 선택할 필요 없이
아마 자동으로 Linux FileSystem일텐데
혹시 모르니 다시 표에서 확인해주시면 됩니다.
마지막으로 WRITE를 선택하시면 됩니다.
QUIT 선택해서 나옴.
```

계획대로 작성되었으니 저장한다는 개념으로 WRITE 해주고 나오겠습니다.

```
방향키를 이용해 WRITE로 이동, ENTER로 선택
QUIT 선택해서 나옴
```

이제 포맷 단계로 진행하시면 됩니다!!

### 포맷

포맷은 파티션 각각의 계획에 맞는 파일 시스템으로 진행합니다.

UEFI환경에서 ESP, 즉 **부팅 파티션은 FAT32 파일 시스템**을 원합니다. 따라서 아까 EFI시스템으로 파티션을 계획했던 `sda1`은 `fat32`로 포맷을 할 것이고, 기본 파일 시스템으로 쓰기로 했던 `sda3`는 가장 범용으로 쓰이는 `ext4`파일 시스템으로 포맷 하겠습니다.

> **스왑 파티션은 다음 단계에서 포맷 합니다.**\
> `/dev/sda2`를 포맷하지 않도록 주의해주세요!

`vfat`은 512MB를 잡았으니 금방 되고 `ext4`로 포맷 예정인 `sda3`는 본인이 부여한 용량에 따라 시간이 잠깐 소요되는 것이 정상입니다.

아래 명령어에 디스크 경로가 포함되는데 **본인 환경에 맞는 것**을 입력하시면 됩니다.

```console
# mkfs.vfat -F32 /dev/sda1
# mkfs.ext4 -j /dev/sda3
```

> **명령어 설명**\
> `mkfs`: **M**a**K**e **F**ile **S**ystem

### 스왑

스왑 드라이브를 만들겠습니다.

스왑 드라이브는 위에서와 같은 포맷 과정을 수행하고나서 켜주는 과정을 추가로 거칩니다.

아래 명령어에 디스크 경로가 포함되는데 **본인 환경에 맞는 것**을 입력하시면 됩니다.

```console
# mkswap /dev/sda2
# swapon /dev/sda2
```

> **명령어 설명**\
> `mkswap`: **M**a**K**e **Swap**

### 마운트

마운트는 [리눅스 파일 체계](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/index.html) (`/`(루트 디렉토리)로부터 뻗어나오는 파일 트리) 각각의 디렉터리에 어떤 드라이브의 어떤 파티션을 이용하겠다고 연결하는 거구나 생각하시면 편합니다.

이 가이드에서 소개한 계획을 따라하고 계시다면 루트 디렉터리(`/`)와 루트 아래의 부트 디렉터리(`/boot`)만 마운트하면 됩니다.

당연한 얘기지만 `/boot`는 `/` 하위 디렉터리로 존재하는만큼, 파티션 계획에서는 3번째였던 `/` 파티션부터 마운트해야 정상적으로 `/boot` 디렉터리를 만들어 드라이브를 마운트할 수 있습니다. 아래 가이드는 이 시나리오를 기반으로 작성되어 있으니 걱정하지 않고 무작정 따라하셔도 정상 작동하긴 합니다.

별도로 각자 파티선 계획 단계에서 구상한 계획에 따라 홈 디렉터리(`/home`)등을 따로 하나의 드라이브(예를 들어, `/dev/sda4`)로 마운트 하는 것도 당연히 가능합니다.

**설치 중의 `/mnt`이하 디렉터리 구조들이, 설치를 완료하고 재부팅하면 그대로 `/`이하 디렉터리 구조가 됩니다.**

> **설치용 USB의 역할**
>
> `/mnt` 아래에 새로운 시스템 디렉토리들을 모두 마운트하거나 생성하고, 반드시 필요한 소프트웨어를 설치한 다음(`pacstrap`), 해당 시스템 내부로 진입합니다(`chroot`).
>
> 다시 말해서, 설치 프로그램이 하는 역할은 라이브 리눅스 시스템을 제공하여 `/mnt` 아래에 또 다른 새로운 리눅스 환경을 만들 수 있는 도구를 제공하는 것 이상도 이하도 아닙니다.
>
> 바로 이 점을 이용해, A 리눅스 iso 혹은 라이브 USB를 통해, B 리눅스 배포판을 설치하는게 가능한 것이죠!

우선은 `/mnt`에 루트 디렉터리로서 쓸 Linux FileSystem 파티션(`/dev/sda3`)을 연결해보겠습니다.

아래 명령어에 디스크 경로가 포함되는데 **본인 환경에 맞는 것**을 입력하시면 됩니다.

```console
# mount /dev/sda3 /mnt
```

이렇게 하고 나면 `/mnt`는 설치 완료 후 재부팅하면 `/`가 됩니다.

그리고 나서 `/mnt` 아래에 부트 디렉터리를 만들어 줍니다.

```console
# mkdir /mnt/boot
```

> **명령어 설명**\
> `mkdir`: **M**a**K**e **Dir**ectory

마운트 하기 위해 미리 만들었던 `/dev/sda1`를 `/mnt/boot`에 마운트 해줍니다.

```console
# mount /dev/sda1 /mnt/boot
```

### 시간 설정

`#timedatectl set-ntp true`명령어를 통해 서버 시간과 동기화할 수 있습니다. 특히, 윈도우즈를 쓰던 컴퓨터에서 처음 리눅스를 설치할 때는 사용하는 시간 기준이 각각 localtime과 UTC 기반으로 다르기 때문에 꼭 수행하는 것을 추천합니다.

더 고전적인 방법은 `#date` 명령어를 통해 직접 현재 적절한 시간을 입력하는 것입니다. 휴대폰 등의 별도 디바이스를 통해 구글에 'current utc time'을 검색해 현재 UTC 시간을 확인합니다. `#date` 명령어로 확인한 시간을 입력합니다. 순서는 '월일시분년' 입니다. 예를 들어, 2021년 12월 31일 14시 56분은 `#date 123114562021`로 입력합니다. 이 방법을 알아두는 것이 쓸모 없어 보일지도 모르지만 `#timedatectl`과 같은 명령어는 init 시스템으로 `systemd`를 지원해야만 가지고 있는 명령어입니다(물론 배포판 열에 아홉은 `systemd`이긴 함..). 따라서, 다른 방법도 알아두시면 혹시나 모를 상황에 도움이 될 수 있습니다.

위 과정이 너무 귀찮거나 네트워크를 통해 자동으로 시간을 동기화 하는 것에 거리낌이 없으시다면 `#ntpd -q -g`를 통해서 생략 가능합니다. 서버 시간 자동 동기화는 ntp 서버에 내 기기의 IP가 드러난다는 점 등 개인 정보에 예민한 분들은 선호하지 않습니다. 그리고 아치 리눅스 설치 iso에는 `ntp`가 동봉 되지 않습니다. 이 방법으로 진행하실 분들은 `#pacman -Syu`이후, `#pacman -S ntp`를 통해 `ntp`패키지를 설치하시고 사용하시면 됩니다.

```console
// 편한 방법 하나를 선택
#  timedatectl set-ntp true # 혹은
#  date 123114562021        # 혹은 ntp를 설치해
#  ntpd -q -g
```

시간을 맞춘다음, 아래 명령어를 통해 하드웨어 시간도 시스템 시간과 동기화해줍니다.

```console
# hwclock --systohc --utc
```

> **명령어 설명**\
> 하드웨어 클락을 utc 기준인 현재 시스템 클락과 동기화함
>
> - `systohc`: 시스템 시간(**sys**tem time)을(to) 하드웨어 시간(**h**ardware **c**lock)으로
> - `utc`: 로컬타임(`localtime`)을 기준으로 하지 않고 `utc`를 기준으로 함을 명시
> - `hctosys`: 하드웨어시간을 시스템 시간으로 밀어내기도 합니다.

### mirrorlist

> 기존 가이드의 내용대로 진행하셔도 됩니다만 **최근 iso들은 네트워크가 연결된 상태에서 이미 적당한 `mirrorlist`를 작성해줍니다.** 따라서 별도 수정 필요 없이 **그냥 사용하셔도 무방한 것으로 확인됩니다.**
>
> 여기도 도움이 될 추가 정보가 있으니 한 번쯤 읽어보셔도 좋고, **그냥 넘어가실 분들은 한 챕터 아래인 `pacstrap` 항목부터 이어가시면 됩니다.**

미러 리스트는 아치에서 패키지 설치를 할 때 기본 데이터 베이스로 활용할 서버를 나열해 놓은 리스트입니다. 미러 리스트에 적힌, 사용할 서버가 가깝거나, 속도가 가장 빠르게 접속 되는 것으로 나열되어야 설치/사용 환경에서 유리합니다.

따라서, 수정해 보도록 하겠습니다. nano 에디터를 사용해서 파일을 열겠습니다.

```console
// 기존 파일 백업은 필수
#  cd /etc/pacman.d
#  cp mirrorlist mirrorlist.bak

// 수정하기 위해 파일 열기
#  nano -w /etc/pacman.d/mirrorlist
```

다 지우고 적으실 분들은 지우기 시작할 라인으로 커서를 옮기신 이후, `Alt+Shift+T`를 하시면 맨 끝줄까지 삭제됩니다.

[아치 리눅스 미러리스트 페이지](https://archlinux.org/mirrorlist/?country=KR&protocol=http&protocol=https&ip_version=4&ip_version=6)에서 생성된 결과입니다.

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

> **nano 편집기를 사용하여 편집 후 저장하고 나오는 방법**\
> `Ctrl+X`→`y`→`Enter`
>
> - `ctrl+x` : 종료
> - 그리고 묻는 것: 수정된 사항이 있는데 수정하십니까? `y`
> - 파일 명은 뭐로 합니까? (디폴트 값은 원래 파일명으로 되어 있으므로) `Enter`

> **vim 편집기를 사용하여 편집 후 저장하고 나오는 방법**\
> `Esc`→`:wq`→`Enter`
>
> - `esc`를 이용해 `insert`모드(입력 모드)에서 `normal`모드(일반 모드)로 나옵니다.
> - `:`를 이용하여 명령어를 입력할 준비를 합니다. 화면 하단에 콜론이 입력된 것을 확인하실 수 있습니다.
> - `w`rite, `q`uit

[reflector](https://wiki.archlinux.org/title/reflector)가 뭔지 알고 어떻게 사용하는지 익숙하신 분들은 지금 단계에서 설치해서 속도(`--rate`)와 국가 코드(`KR`)에 따라, 혹은 본인 환경에 맞게 새로 작성하셔도 무방합니다. 뿐만 아니라, 설치하신 경우 `systemd` 서비스를 통해 부팅시마다 미러 리스트를 업데이트하도록 설정할 수도 있습니다. 원하시는 옵션을`/etc/xdg/reflector/reflector.conf`에 미리 적어두신 후,`# systemctl enable reflector.service`를 통해 켜줍니다.

### pacstrap

이제 `pacstrap` 명령어를 통해 새로운 시스템이 될 `/mnt`에 필수 프로그램들을 설치합니다.

반드시 설치해야 하는 것들을 포함하는 명령어는 아래와 같습니다**만 명령어 아래 단락을 꼭 확인하시기 바랍니다!**

```console
# pacstrap /mnt base linux linux-firmware networkmanager
```

추가적으로 아래 항목들은 위 `linux-firmware` 뒤에 추가할만한 것들입니다.

예상하기로 여러분께 가장 필요할 것으로 생각되는 패키지부터 아마도 필요하지 않을 것 같은 패키지 순서로 나열합니다.

```
# 에디터 프로그램 하나 이상
nano
vim
emacs

# 대부분의 사용자에게 필요
base-devel
man-db
man-pages

# 여기서부터는 무슨 패키지인지 모르시겠다면 불필요한 것입니다.
# 하지만 본인 부팅 환경에 필요하다면 설치하시면 됩니다.
# 물론, arch-chroot 이후 pacman으로도 설치 가능
mdadm
lvm2
```

[base 패키지 목록](https://archlinux.org/packages/core/any/base/)과 [base-devel 패키지 목록](https://archlinux.org/groups/x86_64/base-devel/)은 링크에서 확인하시기 바랍니다.

### genfstab

`/etc/fstab`은 부팅할 때 어느 디렉토리에 어느 드라이브가 마운트 되야 하는지를 시스템이 알게 해주는 파일 시스템 테이블(**F**ile **S**ystem **Tab**le) 파일입니다.

아래 작업은 `genfstab`이라는 스크립트를 활용해 그 파일 시스템 테이블 파일(`/etc/fstab`)을 자동으로 생성하는 작업입니다. (Gentoo, LFS  등 더 귀찮은 수작업 시스템은 `genfstab`과 같은 유틸리티를 제공하지 않아 손으로 작성합니다.)

```console
# genfstab -U /mnt >> /mnt/etc/fstab
```

> **명령어 설명**
>
> - `genfstab` : **gen**erate **F**ile **S**ystem **tab**le
> - `U` : U는 대문자(`UUID`로 작성하겠다는 의미, 검색: `UUID vs PARTUUID`)
> - 현재 `/mnt` 아래 마운트 된 드라이브들을 그대로
> - `>>`: 뒤에 나오는 파일명에 추가로 작성(Append). 부등호를 한개만 사용하면(`>`) Overwrite 명령이 되어 파일이 이미 존재하더라도 내용을 지우고 새로 작성됩니다.
> - `/mnt/etc/fstab`: 내용이 담길 파일명

`# cat /mnt/etc/fstab` 명령어를 통해서 어떤 내용이 적힌 건지 확인해 보실 수도 있습니다.

> **명령어 설명**\
> `cat`: con**cat**enate의 줄임

인자로 주어지는 파일의 내용을 보이거나, 파이프(`|`→`Shift+\`)를 활용해 다른 파일에 입력할 때 자주 활용합니다.)

## 아치 리눅스 진입

### 루트 진입

```console
# arch-chroot /mnt
```

> **명령어 설명** \
> `arch-chroot`: **arch-ch**ange **root**, `/mnt`를 `/`로 가정하는 환경으로 진입합니다.

설치될 시스템 내부로 들어왔습니다! 추가적인 시스템 세팅을 진행하도록 하겠습니다.

여기에서 추후 사용할 소프트웨어를 설치하시면 나중에 재부팅 후에도 설치된 채로 남아있습니다. 예를 들자면, vim 에디터를 사용하실 분들은 `# pacman -Syu` 이후, `# pacman -S vim` 명령어를 통해 설치하셔도 재부팅 이후, 아치 환경으로 부팅했을 때 해당 패키지가 설치되어 있습니다.

> 설치 완료 후, 부팅이 안되는 경우 `# arch-chroot /mnt`를 활용해 오류 수정할 수 있습니다.
>
> ```console
> 설치 완료 후, 재부팅 해봤는데 문제가 있다.
> 다시 Live USB로 부팅
> # mount -v -t ext4 /dev/sda3 /mnt
>
> /mnt에 마운트된 /dev/sda3에 디렉터리 구조가 정상적으로 존재하는지 확인
> # ls /mnt
>
> 정상이면 (필요한 경우) 나머지 드라이브도 마저 마운트
> # mount -v -t vfat /dev/sda1 /mnt/boot
>
> 다시 시스템 내부로 진입
> # arch-chroot /mnt
>
> 필요한 보수 작업 실행
>
> 설치 완료 과정과 동일하게 빠져나오기
> # cd
> # umount -lR /mnt
> # reboot
> ```

### 인터넷 확인

```console
# ping -c 3 www.google.com
```

### 비밀번호 설정

```console
# passwd
```

타이핑하시는 '설정할 비밀번호'는 **원래 화면에 안보입니다**. 놀라지 마세요!!

확인차 **총 두 번** 입력하는게 맞습니다. 놀라지 마세요!!

### 언어 생성

```console
# nano -w /etc/locale.gen
```

파일이 뜨면 화살표 키를 이용해 아래로 내려가 `en_US.UTF-8`을 찾습니다. (vim 사용자 → `/en_US.UTF`, `Enter`, 찾는 부분 나올 때 까지 `n`, 해당 줄의 주석에서 `x`)

**앞의 주석(`#`)만 제거해주세요.**

`/etc/locale.gen`은 어떤 언어 시스템을 생성할지 결정합니다. 즉, 여기서 `en_US.UTF-8`과 `ko_KR.UTF-8`을 주석 해제 해 두어야 아래에서 `locale.conf`에 둘 중 어느 하나를 골라 적더라도 실제로 사용할 수 있습니다.

`ko_KR.UTF-8`을 사용하지 않으실 분들은 `en_US.UTF-8`만 주석 해제 하시고 생성하시면 됩니다.

설정 파일대로 생성을 합니다.

```console
# locale-gen
```

### 언어 설정

`locale.conf`파일은 시스템 전반의 언어를 관리하는 파일입니다. 여기에 지금 `en_US.UTF-8`을 적어넣을텐데, 한글 디렉토리명, 터미널에서의 한글 오류 등을 선호하시는 분들은 `ko_KR.UTF-8`을 입력해주시면 됩니다.

```console
# echo LANG=en_US.UTF-8 > /etc/locale.conf
```

> **명령어 설명**
>
> `echo` 와 `>` 는 `LANG=en_US.UTF-8` 이라는 문장을 그대로 `/etc/locale.conf` 파일에 **덮어 씌우겠다** 라는 뜻입니다. 파일이 없을 경우 파일이 생성됩니다. `>>`를 이용해 기존 내용은 그대로 두고 추가할 수도 있습니다.

이전처럼 이부분도 `# nano -w /etc/locale.conf` 혹은 `# cat /etc/locale.conf` 통해 확인해 보실수 있습니다.

> 위 명령어와 다르게 `# localectl set-locale LANG=en_US.UTF-8` 과 같이 적용하는 방법도 있습니다. 이는 `systemd` init system에서만 사용 가능한 방법이므로 다양한 시스템을 활용하시는 분들은 원 글에 설명 드린 명령어를 활용하시면 더 유익할 것으로 생각합니다.

(생략 무방) 아래의 명령어로 **현재 진행하고 있는 쉘 환경**에서의 언어도 `en_US.UTF-8`이라고 선언합니다.

```console
# export LANG=en_US.UTF-8
```

### 호스트네임 설정

아래 명령어로 **원하는 호스트명, 즉, PC 이름을 설정**

```console
# echo YOURHOSTNAME> /etc/hostname
```

### 로컬타임 설정

위에서 시간을 UTC 기준으로 맞춰뒀으니 이제 한국 시간으로 맞춥니다.

```console
# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

> **명령어 설명**
>
> - `ln`: **l**i**n**k
> - 옵션
>   - `-s`: **s**ymbolic
>   - `-f`: **f**orce
> - `/etc/localtime`을 `/usr/share/zoneinfo/Asia/Seoul`에 링크

앞으로`/etc/localtime`을 참조할 때 실제론 `/usr/share/zoneinfo/Asia/Seoul`를 향하도록 해주는 작업입니다.

> 리눅스에서의 심볼릭 링크 혹은 소프트 링크는 윈도우즈 환경의 바로가기 생성과 유사하다고 생각하시면 편합니다. 즉, `ln -s A B`하면 B를 참조할 때 A로 가라는 이정표를 꽂아둡니다. ~~(마치 `int* B = &A`)~~
>
> 인자 순서가 참 헷갈리는데, 유닉스 환경에서는 ‘있는 것 → 없는 것’ 순으로 쓴다고 암기하시면 됩니다. 이미 있는 A파일로 향하는, 아직 없는 이정표 B를 세웁니다. `mv`도 있는 것 → 없는 것, `cp`도 있는 것 → 없는 것.

### 사용자 추가

아래 명령어의 `SOMEUSERNAME` 부분에 **본인이 사용하실 이름**을 입력합니다.

```console
# useradd -m -g users -G wheel -s /bin/bash SOMEUSERNAME
```

> **명령어 설명**
>
> - `m` : user라는 이름을 홈 디렉토리에 생성. 유저의 홈이 됨.
> - `g`: 소문자 g는 기본 그룹을 의미, 따라서, `users` 그룹에 포함됩니다.
> - `G`: 추가적으로 속할 그룹 설정(참조: [아치리눅스 그룹리스트](https://wiki.archlinux.org/index.php/Users_and_groups#Group_list))
> - `wheel`: 관리자 그룹(`su` 명령 사용 가능 그룹)
> - `s`: default 아닌 쉘을 사용하기 위한 옵션(`zsh`, `fish`를 사용하고자 할 때)
> - `/bin/bash`: `s`로 다른 쉘을 사용하겠다고 했으니 그 다른 쉘로써 bash를 지정

새로 생성한 사용자의 비밀번호를 설정합니다.

아래 명령어의 `SOMEUSERNAME` 부분을 **본인이 지정한 유저 이름으로 변경**해야 합니다.

```console
# passwd SOMEUSERNAME
```

### sudo

> **sudo? doas? su -l?**\
> 최근 해외 포럼에서 `sudo`에 대한 논란이 작게나마 발생합니다.
>
> BSD 계열에서 자주 보이는 'OpenBSD의 `doas`를 통해 더 미니멀하게 접근할 수 있다'는 의견과
> 원래 `sudo`가 개발된 목적이 다중 사용자 컴퓨터에서 활용하기 위함이었던 것을 감안하면 '1인 사용 환경에서는 `sudo`든 `doas`든 필요 없이 `su -l`등을 통해 직접 루트 환경이 되어 작업하는 것이 보안상 적절하다' 등이 주요 논지입니다.
>
> 판단은 여러분 몫입니다.
>
> 초보 사용자용 가이드인 만큼 대부분의 경우에 접하기 쉬운 `sudo`를 설치하겠습니다.

sudo가 설치되지 않은 경우(`# which sudo`를 통해 확인→`/bin/sudo` 등 경로가 나오면 설치되어 있음), `# pacman -S sudo`를 통해 설치합니다.

vim 에디터가 익숙하신 분들은 그냥 `# visudo` (`EDITOR` 환경 변수가 `vi`으로 잡혀 있는 경우) 하시거나 `# EDITOR=vim visudo` 하셔도 됩니다.

```console
# EDITOR=nano visudo
```

> `sudoers` 파일은 `# nano /etc/sudoers`커맨드로도 수정이 가능하지만 **원칙은 `# visudo`를 통해 수정하는게 맞습니다**. `visudo`가 `sudoers`파일의 문법적 오류 등을 점검하고 잠그는 기능을 하기 때문에 임의로 `# nano /etc/sudoers`를 직접 수정하는 것은 권장하지 않습니다.

`sudo` 명령어 사용 가능 그룹에 새로 만든 사용자의 추가 그룹이었던 `wheel`을 추가하겠습니다.

```file
##
## User privilege specification
##
root ALL=(ALL) ALL

## Uncomment to allow members of group wheel to execute any command
#  %wheel ALL=(ALL) ALL   // 이렇게 주석 처리 되있던 것을
%wheel ALL=(ALL) ALL      // '#'을 지워서 이렇게 되도록 주석 제거 처리 해주면 됩니다.

## Same thing without a password
#  %wheel ALL=(ALL) NOPASSWD: ALL
// 위 설정의 주석을 제거하면,
// wheel 그룹 소속 사용자가 '비밀번호마저도 필요없이'
// sudo만 붙이면 명령 실행이 가능하도록 합니다.
// 보안상의 이유로 강력히 반대합니다.
// 어떤 변경을 하는 것인지 정확히 알고 계시는 분만 주석 제거하시기 바랍니다.
```

### 부트로더

부트로더 설치를 하겠습니다. 두 가지 중 하나를 선택해 이용할 수 있습니다.

1. `grub`은 대부분 배포판에서 사용하기 때문에 익혀두시면 다른 배포판을 다룰 때에도 익숙할 수 있습니다. 다만 `grub`과 더불어 `efibootmgr`과 같은 프로그램을 추가로 설치해서 진행합니다.
2. `systemd-boot`는 `systemd`에 내장된 기능을 이용해 부트를 시도합니다. 첫 설치는 조금 더 까다롭다고 느끼실 수 있지만 추가 설치를 최소화하려는 아치 배포판의 철학과 매우 잘 부합한다고 생각합니다. (`grub` 파트 뒤에서 설명합니다.)

> 제 추천을 원하신다면 `systemd-boot`를 추천 합니다만 둘 다 어떤 것인지 처음 들어보셨거나, 들어봤다고 하더라도 다른 부트로더, 예를 들어, `{e,}lilo`가 뭔지 모르신다면 `grub`을 사용하시기 바랍니다.
>
> 부트로더는 시스템을 부팅시키는게 핵심 역할이기 때문에 다양한 부트로더를 활용해봤거나 수정을 직접 해본 경험이 충분하지 않은 이상 많은 배포판에서 활용하고, 문서나 지원을 검색하기 용이한 패키지를 사용하는 것이 맞다고 봅니다.

> **grub'2'? gummiboot?**
>
> `grub`은 현재 `grub2`입니다. `grub-legacy`라고 하여 예전 BIOS환경일 때부터 쓰여온 훌륭한 소프트웨어입니다. 다만, 너무나 많은 기능을 제공하다보니 단지 UEFI 환경의 부트만을 원하는 사용자에게는 의미없는 기능들도 추가되어 있습니다. 이는 곧, 소프트웨어에 예기치 못한 충돌이나 문제가 발생할 소지가 많을지도 모른다는 의미로 받아들이기도 합니다.
>
> `systemd-boot`는 `bootctl`혹은 `gummiboot`라고도 불리는데 그 이유는 `systemd`가 흡수를 하기 전 외부에 존재하는 소프트웨어일 때의 이름이기 때문입니다. 이제는 `systemd`와 완전히 합쳐져서 UEFI 부트만을 보조하는 기능을 수행합니다(물론, BIOS 부트를 설치할 수 있습니다만 추천하지 않습니다). 아치리눅스 설치 과정에서도 이미 default로 설치되는 프로그램이기도 합니다.

### **grub**

설치를 합니다. 설치 전에 레파지토리(아치 리눅스 측 패키지 저장소)를 기준으로 로컬 레파지토리(설치 중인 컴퓨터가 가진 소프트웨어들의 정보)를 업데이트, 동기화 합니다.

```console
# pacman -Syu
```

> **명령어 설명**
>
> - `S`: 원격 레파티토리와 동기화
> - `y`(`-refresh`): `S`와 쓰일 수 있는 옵션으로 원격 레파지토리의 패키지 DB를 새로 다운로드
> - `u`(`-sysupgrade`): `S`와 쓰일 수 있는 옵션으로 다운로드한 DB에 맞게 시스템 패키지를 업그레이드

이제 사용할 패키지들을 가져와서 설치하겠습니다.

```console
# pacman -S grub efibootmgr
```

이제 grub을 이용해 부트가 가능하도록 설치하겠습니다.

> 이 과정을 많은 분들이 혼란스러워 하시는데 이 과정은 '시스템에 부트로더 설치' 입니다. 즉, 부팅 과정을 책임질 `grub`이 시스템 설정을 확인하고 부팅이 가능하도록 만드는 과정입니다.
>
> `grub` 패키지를 다운로드, 설치하는 것이 아님. 패키지 자체는 위 명령어로 설치하였습니다.

```console
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch --recheck
```

> **명령어 설명**
>
> - `grub-install`: 그럽 설치 명령
> - `target=x86_64-efi`: 타겟 아키텍쳐 `x86_64-efi`로 지정
> - `efi-directory=/boot`: `efi`디렉터리 위치 지정
> - `bootloader-id=arch`: 부트로더-이름은 `arch`로
> - `recheck`

이제, `# nano -w /etc/default/grub`을 통해, 부트로드의 기본 설정을 변경할 수 있습니다. 예를 들어, 부팅할 OS선택을 기다릴 시간을 0 또는 1초 처럼 짧게 변경하거나 할 수 있습니다. 여기서는 건드리지 않겠습니다. 설정이 완료된 경우, `/etc/default/grub`의 설정들을 기반으로 하여 부팅을 위한 '파일(`/boot/grub/grub.cfg`)'을 생성/설치합니다.

```console
# grub-mkconfig -o /boot/grub/grub.cfg
```

> **명령어 설명**
>
> - `grub-mkconfig`: grub이 `/etc/default/grub`에 쓰인 설정대로 실제 부팅에 사용할 설정 파일(`grub.cfg`)를 생성합니다.
> - `o`: UNIX 계열에서 `o`는 output의 줄임으로 생성 파일 경로/이름을 지정할 때 자주 쓰입니다.
> - `/boot/grub/grub.cfg`: 가장 기본적으로 부팅시 확인하는 경로이자 파일명입니다.

추후에 여러가지 이유로 grub 설정을 변경한 때에도 사용하는 명령어입니다. 특히 커널 변수등을 실험해보거나 할 때 `/etc/dafault/grub` 파일을 수정한 후에 위 명령어를 통해 업데이트를 합니다.

정리하자면

- grub 패키지를 다운로드 받아 설치한 이후,
- grub 부트 로더를 시스템에 설치하면 `/etc/default/grub`이라는 추가 설정 가능 파일을 남기는데, 필요하다면 이 파일을 입맛에 맞게 수정하고 나서
- `# grub-mkconfig -o /boot/grub/grub.cfg`를 통해 `/boot/grub/grub.cfg`를 만들어야
- 부팅 과정에서 `/boot/grub/grub.cfg` 설정을 읽어들여 부팅이 가능해집니다.

### **systemdboot**

**grub을 사용하지 않기로 하신 분들만 해당합니다.**

```console
# bootctl --path=/boot install
```

`/boot/loader`의 `/loader.conf` 파일을 수정해야 합니다.

```console
# nano -w /boot/loader/loader.conf
```

파일에 아래와 같이 작성

```
default arch
editor 1
timeout 3
```

> **파일 설정 설명**
>
> - `editor`: 부팅 중에 ‘e’키 등을 이용해 커널 변수 혹은 부팅 환경설정을 조정하는 경우가 있는데 0으로 설정 시에 이것을 방지합니다. 개인적인 권장으로는, 지금은 ‘1’로 설정해 두고 정상 작동한다는 것을 확인한 이후에 수정하는 것을 추천합니다.
> - `timeout 3` 은 부팅 화면 중에 선택을 기다리는 시간입니다. 이것도 마찬가지로 지금은 설정해두되 나중에 ‘0’등으로 수정하셔도 됩니다.

디폴트 이후 `arch`에는 본인이 알아볼 이름을 넣으시면 됩니다. 보통은 `arch`를 이용합니다. 다만, 주의하실 점은 이 부분에 넣은 이름을 꼭 기억하셨다가 바로 뒤에 따라오는 과정에서 `entries/abcd.conf` 파일의 `abcd`부분인 파일명에 동일하게 작성해 주셔야 한다는 점입니다. **지금 `arch`로 작성하시면 나중에 그냥 `entries/arch.conf` 파일을 만드시면 됩니다**.

이제 `loader` 디렉터리 아래의 `entries` 디렉터리에 파일을 생성하고 수정하겠습니다.

```console
# nano -w /boot/loader/entries/arch.conf
```

`arch.conf`파일에 내용을 추가하기 전에, 아래의 **참고**부분을 한번 읽어보시고, 참고란 끝에 예시 파일을 보여드리겠습니다.

> **참고**
>
> `# ls /boot` 해보시면 `/boot` 디렉터리 아래에 `loader`와 각종 이미지 파일(`vmlinuz`, `initramfs-linux` 등)이 있는 것을 확인하실 수 있습니다. 그것들을 파일 내용에 포함하는 것입니다.
>
> `title`에는 알아보기 쉬운 이름을 사용하시면 됩니다. 이 곳에 쓰인 이름은 다른 설정에서 추가로 필요하지는 않고 부팅 과정에서 메뉴 이름으로 활용됩니다. 보통 `ArchLinux` 등을 사용하는 편입니다.
>
> **`linux` 항목에서 `vmlinu”z”`에 주의하시기 바랍니다.**
>
> ~~추가적으로 저는 `rw` 옵션에 이어 `quiet` 등을 사용합니다.~~ 그랬었는데, 다시 `quiet` 옵션은 그냥 안씁니다. 큰 이유는 없고 Failed가 뜨는지 보고 싶어졌습니다.

```
title ArchLinux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=/dev/sda3 rw
```

> **PARTUUID 쓰라는 사람 있던데 그게 뭐임??**
>
> `option` 항목에서 권장 사항은 물론 `PARTUUID`를 이용하는 것입니다. 하지만 우선은 루트 파티션 경로를 지정해 주는것으로 대체하도록 하겠습니다. 위키에 따르면 “리포맷 되거나 경로 매핑이 변경되는 경우에 비해 `PARTUUID`가 장점을 가진다”라고 되어있는데, 일반 사용자, 초보 사용자 기준에 그럴 일이 드물 것으로 가정하고 경로를 지정합니다. 또는, 정상 부팅이 되는 것을 확인한 이후에 수정하는 것도 한 가지 방법이 될 것입니다.
>
> `PARTUUID`를 사용하고 싶으신 분들은
>
> (1) `# blkid`를 통해 루트파티션(지금은 `sda3`)의 `PARTUUID`를 복사하시거나 적어두신 후 위의 `options` 항목의 내용을 `root=PARTUUID=123efg45–789 rw` 로 해주시면 됩니다. 물론 `123efg45–789`이 부분에 본인의 `PARTUUID`가 들어갑니다. (원래 `PARTUUID`가 깁니다. 놀라지 마세요! / `“`(쌍따옴표) 같은게 추가되지 않았는지 잘 확인하시기 바랍니다!!)
>
> (2) 적어두는 것이 귀찮은 분들은 `options` 줄을 지운채로 저장하신 후 `# echo "options root=PARTUUID=$(blkid -s PARTUUID -o value /dev/sda3) rw">> /boot/loader/entries/arch.conf`(본인 환경에 맞게 대체해야 되는 부분 없는 명령어임)를 활용하시면 됩니다. 무슨 방법을 사용하시든 반드시 다시 `# cat /boot/loader/entries/arch.conf`하셔서 추가한 내용들이 적절히 적혀 있는지 확인하시기 바랍니다.
>
> 이렇게 `PARTUUID`를 이용한 경로를 추가하셔서 `options`가 두 줄이 된 경우, 기존에 `/dev/sda3`로 작성되어 있는 `options`줄은 삭제하는건 당연한겁니다;;

### exit

현재 상태로 아래에서 소개하는 `# exit`을 통해 `chroot`환경에서 나가고 재부팅을하면 네트워크 연결이 가능하지 않습니다. 따라서, 네트워크 매니저를 설치하고 켜주도록 하겠습니다.

우선, 아직 설치하지 않았다면,

```console
# pacman -S networkmanager
```

그리고 네트워크 매니저를 재부팅시에 자동적으로 실행되도록 설정합니다.

```console
# systemctl enable NetworkManager.service
```

> **명령어 설명**
>
> `systemd` init 시스템에서 서비스(데몬)들을 관리하는 명령어가 `systemctl`입니다. 현재 부팅된 환경에서 바로 서비스를 켜고 사용을 시작하려면 `start`, 부팅 때마다 자동 실행되게 하려면 `enable`옵션을 사용합니다. 끝에는 대상 서비스의 이름을 적습니다.

혹시, 이 부분을 놓치고 지나가셔서 네트워크가 안되는 분들은, `chroot` 방법을 통해 다시 시스템에 진입하셔서 설치가 가능합니다.

> 설치 완료 후에 부팅이 안되는 경우에도 `# arch-chroot /mnt`를 활용해 오류 수정할 수 있습니다.
>
> ```console
> // 설치 완료 후, 재부팅 해봤는데 문제가 있다.
> // 다시 Live USB로 부팅
> # mount -v -t ext4 /dev/sda3 /mnt
>
> // /mnt에 마운트된 /dev/sda3에 디렉터리 구조가 정상적으로 존재하는지 확인
> # ls /mnt
>
> // 정상이면 (필요한 경우) 나머지 드라이브도 마저 마운트
> # mount -v -t vfat /dev/sda1 /mnt/boot
>
> // 다시 시스템 내부로 진입
> # arch-chroot /mnt
>
> // 필요한 보수 작업 실행
>
> // 설치 완료 과정과 동일하게 빠져나오기
> # exit
> # cd
> # umount -lR /mnt
> # reboot
> ```

```console
# exit
```

### umount

각 디렉터리에 마운트 되어있던 장치들을 제거합니다.

```console
# umount -lR /mnt
```

### reboot

재부팅을 합니다!!!

```console
# reboot
```

장치가 종료되고 다시 시작되는 시점에 USB를 빼주시면 됩니다.

재부팅이 되면 로그인 하라는 화면이 나올텐데 **설치에 성공**하신 겁니다! **축하드립니다!**

아마 기쁘신 분도 있고, 이게 뭐야 하시는 분도 있을텐데, 이게 뭐야 싶으신 분들은 그래픽 환경에 익숙하셔서 그런거라고 생각합니다. 서버와 같이 헤드리스(headless)로 작업하실 분들을 제외하고 앞으로 이어질 데스크탑 환경을 설치하시면 그래픽 환경에서 아치를 사용하실 수 있습니다.

## 그래픽 환경 설치

~~(시대가 바뀌어서..)~~ `wayland`를 설치하도록 하겠습니다.

> 데스크탑 환경 부분, 즉 `wayland`와 `gnome` 부분만큼은 제가 항시 사용하는 소프트웨어가 아니다보니 최신화가 덜 될 수 있습니다.

### 로그인

본인이 설정한 ID/Password로 로그인을 합니다.

일반 사용자로 로그인하였으므로 프롬프트는 `$`로 변경됩니다. `sudo`를 활용해 실행합니다.

### 인터넷 연결 재확인

```bash
$ ping -c 3 www.google.com
```

### 팩맨 설정

이제 설치할 것들이 몇 가지 있으니 `pacman`을 업데이트하겠습니다.

```console
$ sudo pacman -Syu
```

`/etc/pacman.conf` 파일과 `/etc/makepkg.conf`를 살펴보시는 것도 좋습니다.

### 기본 정보

가이드는 `Gnome`을 기준으로 소개합니다.

### 필수 설치

추가로 제공되는 그놈 소프트웨어들도 사용하시고 싶으시면 `gnome-extra`를 설치하시면 됩니다.

```console
$ sudo pacman -S xorg-xwayland gnome (gnome-extra)
$ sudo systemctl enable gdm
```

설치가 완료되고 나면 디스플레이 매니저를 사용할 수 있게 `systemctl enable`을 진행합니다. `gnome` 패키지에는 `gdm`을 포함한 `Gnome` 데스크탑 환경에서 사용하는 필수 소프트웨어가 포함되어 있습니다.

`gdm` 은 'Gnome Display Manager'로써, **데스크탑 환경의 로그인 화면**과 전반적인 그래픽 환경을 관리하는 프로그램입니다. 이전에 인터넷 연결을 위해 `# systemctl enable NetworkManager` 했던 것을 기억하시나요? 이번엔 gdm 서비스를 부팅시마다 사용하기 위해 `enable`을 사용하였습니다.

### reboot again

재부팅을 진행합니다.

```console
$ sudo reboot
```

자 어떤가요? 로그인 화면이 그래픽 환경으로 부팅 되셨나요?

이제 사용자 비밀번호를 입력한 후에 아치 리눅스 Gnome 환경 준비가 모두 완료되었습니다.

아! 하지만 권장 사항들이 몇 가지 더 있네요!

### 웹브라우저 설치

#### **파이어폭스**

사용하고 싶은 웹 브라우저가 **파이어폭스, 혹은 구글-크롬**이라면 어떻게 해야 할까요? 파이어폭스라면 설치해주면 됩니다!

```console
$ sudo pacman -S firefox
```

파이어폭스 설치가 완료되었습니다!

> 팩맨을 활용해서 설치할 수 있는 소프트웨어의 경우는 아치의 공식 레파지토리에 올라와 있는 것들입니다. 공식 레파지토리에는 아치 리눅스의 라이선스와 호환이 가능한, 즉, 오픈 소스/자유 진영 소프트웨어만 있습니다.
>
> 크롬의 경우에는 공식 레파지토리에 **chromium**이라는 오픈소스 버전 크롬이 있습니다. 사실 둘의 차이점은 크지 않아요!(chromium 설치:`$ sudo pacman -S chromium`)
>
> 하지만 나는 꼭 구글-크롬 을 사용하고 싶다하시면 **아치의 자랑인 AUR을 사용**해 보겠습니다!

> **vscode가 이상해요!**
>
> 위에서 적은 이유로 공식 레파지토리를 통해 설치되는 visual studio code는 사실 [Code-OSS](https://archlinux.org/packages/community/x86_64/code/)라는 이름의 오픈 소스 버전입니다. MS에서 사용하시던 vscode와 완전히 동일한 사유 소프트웨어(proprietary software) 버전을 사용하고자 하신다면 아래에서 설명하는 AUR 사용 방법을 숙지하신 다음, '[visual-studio-code-bin](https://aur.archlinux.org/packages/visual-studio-code-bin/)'이나 유사 패키지를 설치하시기 바랍니다.

#### **구글 크롬**

AUR 이란 Arch User Repository로 아치 사용자들이 관리하고 업로드하는 패키지들의 모음입니다. **AUR을 이용해 패키지를 빌드하는 방법은 확실히 알아두셔야 합니다**. 나중에 ’부록’에서 찾으실 수 있는 AUR-helper(yay 등)를 사용하시더라도 오류는 언제든지 발생할 수 있습니다. 그 때는 부득이하게 수동으로 설치하셔야 합니다.

> **[아치리눅스 위키페이지](https://wiki.archlinux.org/title/AUR_helpers)에 따르더라도, AUR helper는 언제나 도움을 주는 제 3 자 소프트웨어일 뿐, 공식적으로 지원을 받는 프로그램이 아닙니다**.

기본 개념은

- "aur.archlinux.org"에 접속해서 내가 설치하려고 하는 패키지의 `snapshot`을 다운로드 받습니다.
- 다운로드 받은 스냅샷의 압축을 해제하면 `PKGBUILD`파일이 있는데,
- `MAKEPKG`라는 명령어를 통해 컴퓨터가 `PKGBUILD`의 내용대로 설치를 하는 것입니다.

자세한 내용은 아래를 통해 설명드리겠습니다.

> `Git`을 활용해 귀찮은 스냅샷 다운로드→압축 해제과정을 피하실 분들은 이 챕터 끝을 참조하세요!!

지금은 '웹'소프트웨어 밖에 없으니 그것을 이용해서(혹은 파이어폭스를 설치하셨다면 파이어폭스를 이용해서)

```file
"https://aur.archlinux.org"를 접속한 후에
우측의 검색 창에 ‘google-chrome’을 입력하시면
구글-크롬에 해당하는 AUR페이지가 뜹니다.
우측의 작은 대화상자(Package Actions)에서
Download Snapshot 을 이용해 Downloads 디렉터리로 가져옵니다.
터미널 실행 후,
```

```console
$ cd Down[tab]
```

최근 모든 쉘은 키보드의 `Tab` 키로 자동 완성 기능을 제공합니다.

```console
$ ls
```

`google-chrome.tar.gz` 를 확인할 수 있습니다. `name.tar.gz`은 `gzip`으로 압축되고 `tar`로 패키지된 파일 이라는 뜻입니다. 해제는 `tar`명령어를 이용하면 됩니다.

```console
$ tar -xvzf goog[tab]
```

`tar`는 파일 압축 해제 프로그램이라고 생각하셔도 됩니다.

> **명령어 설명**
>
> - `x`: extract(압축해제)
> - `v`: verbose(과정 출력)
> - `z`: gzip 으로 압축된 파일
> - `f`: 이어 입력할 이름의 압축 파일을 해제
> - `Tab`키를 이용해 나머지 이름은 자동완성해보았습니다.

```console
$ ls
```

완료되고 나서 다시 `ls`를 해보면 `google-chrome` 디렉터리가 생겼습니다!

```console
$ cd goog[tab]
$ makepkg -risc
```

> **명령어 설명**
>
> - `r`: 빌드가 완성된 이후, 빌드 과정에서 필요로 했던 의존성으로 설치된 패키지들을 다시 제거합니다. 이 옵션으로 의존성으로 설치된 패키지를 제거한 경우, AUR 패키지 업그레이드 당시에 아직도 의존하는 패키지라면 해당 업그레이드 버전을 빌드할 때 다시 설치됩니다. 그러나 업그레이드가 자주 있지 않다는 점을 알거나 더 깔끔한 시스템을 원하는 경우, 옵션을 켜서 제거하는 것도 좋은 방법입니다.
> - `i`: 빌드가 완성되면 자동으로 설치를 진행합니다. 이 옵션을 제거하는 경우, 이어지는 명령어로 `$pacman -U pkgname-pkgver.pkg.tar.zst`를 입력하여 설치해야 합니다. 이런 번거로움을 없애줍니다.
> - `s`: 의존성 중, pacman으로 설치 가능한 공식 레파티토리 패키지들은 자동으로 설치합니다. AUR 패키지 중에서, 또 다른 AUR 패키지에 의존하는 경우는 수동으로 먼저 설치해주어야 합니다.
> - `c`: 설치 이후, 빌드 과정에서 생성된 임시 파일을 삭제합니다.

**google-chrome 설치가 완료되었습니다!** 상단 패널의 `Activities`>`All`을 통해서 확인하실 수 있습니다!

> `Git` 활용시
>
> ```console
> // GIT 미설치 경우
> $ sudo pacman -S git
>
> // AUR 디렉터리 등 생성하는 것을 추천 (AUR 패키지 관리 편의성)
> $ cd ~
> $ mkdir ~/AUR
> $ cd ~/AUR
>
> // AUR 페이지 검색 결과 중, Git Clone URL 참조
> // 클릭해 복사도 가능 -> 터미널에서 붙여넣기는 ctrl+shift+v
> // 레파지토리를 Chrome이라는 디렉터리로 클론
> $ git clone https://aur.archlinux.org/google-chrome.git Chrome
>
> $ cd Chrome
> $ makepkg -risc
>
> // 추후 업데이트 있을 때
> $ cd ~/AUR/Chrome
> $ git clone
> $ makepkg -risc
> ```

### 한글 폰트

한글 폰트가 시스템에 설치되어 있지 않아, 네이버 등을 접속해보면 네모난 박스로 표시됩니다. 이는, `noto-fonts-cjk` 등의 패키지를 설치하여 해결할 수 있습니다. (`$sudo pacman -S noto-fonts-cjk`)

그러나 여기서는 AUR에 더 익숙해지고자 공식 레파지토리에는 없는 폰트를 AUR을 통해 설치해보겠습니다.

```console
(aur.archlinux.org에 접속 → ttf-nanum검색 → snapshot 다운로드)
$ cd Down(tab)
$ tar -xvzf ttf(tab)
$ cd ttf(tab)
$ makepkg -risc
```

위의 과정을 통해 한글 폰트 설치가 완료되었습니다. 여기까지 진행하면, 구글 크롬을 통해 유투브/네이버를 이용하더라도 한글 폰트가 깨지지 않고 출력됩니다.

하지만, 한글로 검색을 하고 싶은데 한글 입력이 안됩니다. 어떻게 해야 할까요?

### 한글 입력

한글 입력 과정에 대한 문의가 많아 [별도 페이지]({{< ref "korean" >}})를 신설했습니다.

## 결론

이 긴 글에서 명령어만 추려내면 아래의 30줄 남짓이 전부입니다. 글이 길어서 정작 ‘아치 리눅스가 무슨 심플(?)’이라고 생각하셨을지 모르지만 이 명령어들을 모두 이해하고 외워져 있는 사용자들 입장에선 당장 사용해야 하는 랩탑/데스크탑을 제공받았을 때 설정에 따라 5~10분만에 자신이 원하는대로 시스템을 구축할 수 있습니다!

어찌 보면 데비안 → 우분투, 혹은 레드햇 → 페도라보다 더 간결하고 우아한 배포판이라고 사람들이 떠드는 이유가 바로 여기에 있다고 생각합니다.

긴 글 읽느라 수고하셨습니다! 🤩

자! 그럼 아치 리눅스는 너무 쉬우니 [젠투]({{<ref "gentoo" >}})로..ㅋㅋㅋ

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

## 부록

### 독자 추가 사항

Davido(<osh3606@gmail.com>)님 추가 사항 (내용 기여 감사합니다!)

```console
// btrfs & timeshift
// btrfs 관리 프로그램 설치
# pacman -S btrfs-progs

// 파일시스템 포맷하기
# mkfs.btrfs /dev/sda3

// 서브 볼륨 생성
# mount /dev/sda3 /mnt
# cd /mnt
# btrfs subvolume create @
# btrfs subvolume create @home
# cd /
# umount /mnt

// 서브볼륨 마운트
# mount -o rw,noatime,compress=lzo,space_cache=v2,subvol=@ /dev/sda3 /mnt
# mkdir /mnt/home
# mkdir /mnt/boot
# mount -o rw,noatime,compress=lzo,space_cache=v2,subvol=@home /dev/sda3 /mnt/home
# mount /dev/sda1 /mnt/boot

// fstab 수정
// 스냅샷 프로그램 사용하기 위해서는 genfstab 이후 fstab에서 subvolid 없애줘야 함.
// 안없애면 추후에 btrfs 복구시 복구 전 id를 찾으려 하기 때문에 정상적으로 복구 안됨…
# nano /mnt/etc/fstab

// /, /home 마운트 옵션에서
// → subvolid=#### 삭제
// → subvol=###/,subvol=### 중 하나 삭제

// timeshift를 이용한 스냅샷/복구
// 설치 종료 후 fresh installed 상태에서 btrfs 스냅샷 만들기(timeshift사용)
# sudo pacman -S git
# git clone https://aur.archlinux.org/timeshift.git
# cd timeshift
# makepkg -si
# sudo timeshift -–create -–comments “Fresh install”

// 복구시
# sudo timeshift –restore

// 원하는 스냅샷 번호 선택
# sudo reboot
```

### btrfs의 스냅샷 기능을 이용해서 백업시

(참조: [timeshift 개발자 레파지토리](https://github.com/teejee2008/timeshift))

`# timeshift --btrfs`커맨드를 통해 `btrfs 모드`로 설정해 주어야함 (기본 설정은 `rsync 모드`)\
이유: 백업 속도에서 차이가 많이 나고, 백업 자체도 byte-by-byte copy라 손실이 없다고 하네요.

#### fstab에서 subvolid 지우는 이유 상세 설명

(참조: [timeshift 개발자 레파지토리 이슈](https://github.com/teejee2008/timeshift/issues/241))

- `fstab`에는 반드시 `@`와 `@home`이 있어야 함. `subvolid` 대신 `subvol=@`를 반드시 사용해야 함. 디바이스는 반드시 `UUID=` 이나 `/dev/xxx`를 통해 명시되야 함.
- timeshift는 우분투 기반 배포판 위해 작성됨. 페도라 혹은 아치에서 사용하는 경우, 대부분 작동에는 문제가 없겠으나 그 목적으로 만들어진 프로그램이다보니 이슈가 발생할 수 있음.

### AUR 관련 당부 말씀

AUR Helper란 AUR 패키지들의 업데이트나 설치를 도와주는 소프트웨어입니다.

일반적으로,

- 현재 시스템에 설치된 패키지 버전보다 더 상위 버전이 레파지토리에 존재하는지 확인만 해주는 유틸리티
- 그보다 한 걸음 더 나아가 상위 버전이 존재한다면 다운로드 받아 주는 유틸리티(search and download)
- 빌드까지 책임져주는 유틸리티(search and build)
- 마지막으로 아예 `pacman`으로 설치한 패키지를 포함, 즉, AUR만이 아니라 공식 레파지토리까지 검색하여 설치와 업데이트를 진행해주기 때문에 평소에 `pacman`을 사용할 일이 없도록 해주는 유틸리티(pacman wrapper)

까지 다양한 종류가 있습니다.

[아치 위키](https://wiki.archlinux.org/title/AUR_helpers)에서는 분명히 **AUR 헬퍼들은 공식 지원 대상이 아님**을 밝히고 있습니다. 따라서, AUR 헬퍼를 사용하다가 발생하는 문제들에 있어서는 지원을 받지 못합니다.

옛날 버전 글에서 `yaourt`가 디폴트 수준으로 많이 사용되어서 가장 앞에 적었습니다. 그런데 `yaourt`는 개발이 중단되었습니다. 그래서 "`yaourt`가 중단되어 다른 소프트웨어 추천을 원하신다면 `cower`를 추천합니다."라고 수정해 놨었는데 `cower`도 개발이 중단되었습니다.

현재 많은 인기를 끌고 있는 것은 `auracle`(search and download), `yay`(pacman wrapper)로 보입니다.

제가 아치 리눅스를 사용한 기간이 그리 긴 시간도 아닌데(슬슬 길어지고 있네요;;) 그간 수많은 AUR 헬퍼 패키지가 등장했다가 인기를 끌고 사라져갔습니다. 그냥 `Git` 혹은 AUR 디렉터리를 홈 아래에 만드신 후, `$git clone`를 통해 업데이트 하면서 관리하시는 게 속편합니다.

무엇보다 **가급적 AUR 패키지를 아예 안 쓰거나, 최소화하는 것을 권장**합니다.\
ㄴ 2222

~~작성자는 현재(*2022/11/14*) [aurutils](https://aur.archlinux.org/packages/aurutils)를 설치해서, 오직 `$ pacman -Qm | aur vercmp` 용도로만 활용하고 있습니다.~~ 사용하지 않음.
