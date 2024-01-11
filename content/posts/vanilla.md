---
title:  "젠투 리눅스 바닐라 커널"
date:   2017-11-05
url: 'vanilla'
---

---

This is a translated work.
The original post was written by gg7 on Github, and you can read it via [this link](https://github.com/gg7/gentoo-kernel-guide).
Translate permission is granted by gg7.

---

이 문서는 번역본입니다.
원본은 gg7에 의해 작성되었으며, 다음 [깃허브 링크](https://github.com/gg7/gentoo-kernel-guide)에서 읽을 수 있습니다.
원작자 gg7의 번역 허가를 받았습니다.

---

## 세 줄 요약

kernel.org로부터 Git을 통해 관리되는 커널을 `make oldconfig`보다 나은 완전 자동화된 설정하는 방법

```bash
./scripts/config --enable IKCONFIG
./scripts/config --module SENSORS_NCT6775
./scripts/config --disable AUDIT
./scripts/config --set-val SND_HDA_PREALLOC_SIZE 2048
./scripts/config --set-str UEVENT_HELPER_PATH ""
```

뿐만 아니라 다양한 팁들을 제공합니다. 더 이상 4,000+ 줄의 `.config`를 무작정 복사하거나, 불필요한 코드, 헛소리 등이 불필요합니다.

## 개요

이 글은 제가 어떻게 젠투 리눅스 머신에 커널을 다운로드 받고, 설정한 후, 설치하는 지를 설명합니다.저는 이 방법이 온라인에 설명된 90% 방법 그리고 젠투 공식 문서보다도 낫다고 생각합니다. 비판은 환영합니다 🙂제 접근방법의 장점입니다. (요약; 아래의 총 설명란을 보세요):

- 커널 얻기
  - 깃 > 타르볼(tarball): 더 빠른 업데이트와 패치; 내장된 체인지로그 뷰어; 'git-bisect'가 가능
  - 더 많은 대안 접근 가능: 오래된 버전부터 가장 최신 배포 후보까지
  - 업스트림으로부터 지원을 받기 쉬움
  - 간결함과 투명성
- 수정
  - 완전 자동화, 주석과 깃에 더 적합한 수정. 커널 시드나 `.config` 파일들을 복사하는 것보다 나음
  - 커널을 업그레이드 할 때 적절한 디폴트값 관리
  - 별도의 코드/복잡성이 존재하지 않음 --- `scripts/config` 는 커널의 일부라는 점
- 커널 빌드와 설치
  - 컴파일은 루트 계정으로 이루어지지 않음
  - `march=native` 가 사용됨

제 접근방법의 단점입니다:

- ~1.5GB의 여분공간이 `.git` 을 위해 필요 ( 버전 관계 없이, `shallow clone`을 사용하지 않는다면 고정 비용에 해당 )
- 젠투 커널팀으로부터는 그 어떤 패치도 제공받을 수 없음 ( e.g. aufs, grsecurity, 작은 수정/기능 증진등 )

## 필독 사항

저는 커널 수정과 관련된 기본적인 내용을 반복하지는 않으려고 합니다 --- 그 내용들은 아래 문서들에 이미 설명되어 있습니다.

- [https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Kernel/ko](https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Kernel/ko)
- [https://wiki.gentoo.org/wiki/Kernel/ko](https://wiki.gentoo.org/wiki/Kernel/ko)
- [https://wiki.gentoo.org/wiki/Kernel/Configuration/ko](https://wiki.gentoo.org/wiki/Kernel/Configuration/ko)
- [https://wiki.gentoo.org/wiki/Kernel/Upgrade/ko](https://wiki.gentoo.org/wiki/Kernel/Upgrade/ko)

문서를 그대로 따라할 필요는 없습니다. 따라서, 페이지를 훑어보면서 혹시 아직 읽어보지 못한 내용이 있는지 확인하시기 바랍니다.

## 가이드

### 커널 확보

커널 개발 방법: [https://git.kernel.org/](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/process/2.Process.rst)

세줄 요약:

- `linux-next`: 통합 리포; 컴파일 조차 안될수도 있음
- `mainline`: "공식" 커널, 라이너스 스스로 관리함
- `stable`: 백포트 수정을 포함하는 리포

우분투나 RHEL과 같은 배포판은 각자 버전의 커널을 가지고 있습니다 (e.g. [우분트의 제니얼 리포](https://kernel.ubuntu.com/git/ubuntu/ubuntu-xenial.git/)). 뿐만 아니라, 각자 백포트 방법으로 "스테이블" 버전이나 지원되는 버전에 수정을 가합니다. 그런 커널을 젠투에 사용하는 것도 가능은 하지만 그 이유는 그다지 매력적이지 않아 보입니다.

젠투에서는 상당한 양의 여분의 패치와 지원을 받을 수 있는 다양한 "공식" 옵션을 제공합니다.

- `sys-kernel/gentoo-sources` --- 기본, 약간의 패치가 적용됨
- `sys-kernel/vanilla-sources` --- 전혀 수정 없는 업스트림의 커널
- `sys-kernel/hardened-sources` --- [고인](https://www.gentoo.org/support/news-items/2017-08-19-hardened-sources-removal.html)
- 기타 --- sys-kernel 카테고리 참조

만약 당신이 gentoo-sources를 사용하게 되면 당신은 아래의 것들을 제공받습니다:

- `CONFIG_GENTOO_LINUX`를 사용함으로 설정했다면 반드시 필요로 하는 다양한 세팅들이 미리 [자동으로 설정](https://dev.gentoo.org/~mpagano/genpatches/trunk/4.12/4567_distro-Gentoo-Kconfig.patch)됩니다.
- 다양한 패치. 예를 들어, 4.12버전을 위한 모든 패치를 여기에서 확인하실 수 있습니다: [https://dev.gentoo.org/~mpagano/genpatches/trunk/4.12/](https://dev.gentoo.org/~mpagano/genpatches/trunk/4.12/). 구체적인 예시: 당신의 `/var/tmp/portage` 가 `tmpfs`에 마운트된다면, `1500_XATTR_USER_PREFIX.patch` 없이는 portage가 어마어마한 양의 `Failed to set XATTR_PAX markings` 에러를 뿜습니다.

젠투 커널 팀은 훌륭합니다.([Greh Kroah-Hartman](https://en.wikipedia.org/wiki/Greg_Kroah-Hartman)도 멤버예요!). 확실히 그들은 가치를 더하고 저보다도 깊은 지식을 가지고 있습니다. 하지만, 그래도 저는 `git`을 통해 [kernel.org](https://www.kernel.org/)로부터 제가 사용할 커널을 구하겠습니다. (맞아요, gregkh가 `linux-stable`도 관리하지만 말이죠) 왜일까요?

- 젠투 커널 팀이 ebuild를 출시하기까지 기다릴 필요 없이 최신 커널을 `git pull` 명령을 통해 구할 수 있습니다.
- 지원을 받기가 수월합니다. kernel.org에서 [인용](https://www.kernel.org/category/releases.html)하자면: "[배포판 커널을 사용중이라면,] 부디 커널 지원을 받기 위해 본인 배포판의 지원 채널을 이용해주시기 바랍니다."
- 어느 방법을 선택하든지 커널 패치를 적용하는 것은 손쉽지만, 원하는 것만을 골라 적용하는데는 git 쪽이 더 쉽다고 느꼈습니다.
- 제 워크 플로우를 변경하지 않고서 필요하다면 mainline/linux-next 관계 없이, 아니면 다른 트리로라도 커널을 변경하는게 수월합니다.
- 더 다양한 버전에 접근이 가능합니다. `linux-stable` + `mainline` 만 해도 2,500+개의 태그를 가졌습니다. 반면에, `vanilla-sources`는 현재 7개의 ebuild만을 가지고 있습니다.
- `git-pull`은 `emerge`보다 빠릅니다. portage는 ebuild를 `/var/db/pkg` 등에 parse해야 합니다. 그 뿐만 아니라 90+MB의 tarball을 주요 버전 업데이트 때마다 다운로드 해야 합니다. 심지어 저는 `package.env`를 통해 `FEATURES=buildpkg`를 사용 안 함으로 설정할 필요도 없습니다. --- `git-pull`은 확실히 덜 짜증납니다.

어떤 커널 버전을 사용해야 하나요? [https://www.kernel.org/](https://www.kernel.org/) 에 접속하세요. 특정한 주요 버전(e.g. `4.9`)를 오랫동안 사용하고 싶다면 longterm release를 고르세요. 그렇지 않다면 최신 stable 버전을 사용하세요. 마지막으로, 실제 가이드입니다. 저는 원격 트리마다 1개의 "마스터" bare repo를 갖고 있기를 선호합니다:

```bash
/usr/src$ sudo mkdir -p linux-stable-git-bare && sudo chown "$(id -un):$(id -gn)" linux-stable-git-bare

/usr/src$ git clone --mirror --bare 'https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git' linux-stable-git-bare
```

이후, 1+ 버전을 선택합니다.

```bash
/usr/src$ git -C linux-stable-git-bare fetch --all --tags
/usr/src$ git -C linux-stable-git-bare tag --sort=-creatordate   # you can also consult https://www.kernel.org/

$ Example with v4.12.8:
/usr/src$ v="4.12.8"
```

버전 별로 디렉터리를 생성합니다.

```bash
/usr/src$ sudo mkdir "linux-stable-git-$v" && sudo chown "$(id -un):$(id -gn)" "linux-stable-git-$v"
/usr/src$ git clone --single-branch --branch "v$v" linux-stable-git-bare/ "linux-stable-git-$v"
```

`.git`디렉터리는 하드링크를 사용합니다. ( 누군가/어떤 것이 `git-gc` 명령을 실행하지 않는다면요 ), 그러므로 굉장히 공간상 효율적입니다.

젠투 커널이 더이상 필요하지 않기 때문에:

```bash
~$ sudo mkdir -p /etc/portage/profile/
~$ echo 'sys-kernel/vanilla-sources-4.12.8' | sudo tee --append /etc/portage/profile/package.provided
~$ sudo emerge -aC gentoo-sources vanilla-sources # ...
```

이건 아마도 나쁜 아이디어 일 수도 있습니다. 왜냐하면 이제 `depclean`명령이 일부 꼭 필요한 패키지를 제거할 수도 있기 때문입니다.

```bash
~$ equery depgraph '=gentoo-sources-4.12.8'
* dependency graph for sys-kernel/gentoo-sources-4.12.8
`--  sys-kernel/gentoo-sources-4.12.8  [~amd64 keyword]
  `--  sys-apps/sed-4.2.2  (sys-apps/sed) amd64
  `--  sys-devel/binutils-2.28-r2  (>=sys-devel/binutils-2.11.90.0.31) amd64
  `--  sys-libs/ncurses-6.0-r1  (>=sys-libs/ncurses-5.2) amd64
  `--  sys-devel/make-4.2.1  (sys-devel/make) amd64
  `--  dev-lang/perl-5.24.1-r2  (dev-lang/perl) amd64
  `--  sys-devel/bc-1.06.95-r1  (sys-devel/bc) amd64
```

아마 이 것들을 `@world`에 추가하는 것이 나을겁니다.

### ebuild 대안

만일 당신이 ebuild를 계속 사용하고 싶지만, 커널 수정은 자동으로 처리하려면 다음 페이지에 나오는 내용을 확인해보시면 방법이 떠오를겁니다. [https://github.com/jollheef/jollheef-overlay/blob/3a59396be2cbbdb1202e30ac577c3676fa5d9a85/sys-kernel/linux/linux-4.17.4.ebuild](https://github.com/jollheef/jollheef-overlay/blob/3a59396be2cbbdb1202e30ac577c3676fa5d9a85/sys-kernel/linux/linux-4.17.4.ebuild)

## 커널 설정하기

커널을 다운로드하고 컴파일, 설치하는 것은 쉽습니다: 당신은 (거의) `git pull && make all install` 만 하면 됩니다. 커널 설정은 스스로 빌드한 커널을 실행하는데 가장 어려운 작업입니다. 기본 설정(디폴트 값)으로부터 얼마나 깊이 수정하고 싶은지에 따라 수 시간까지 시간이 걸릴 수 있습니다.

설정 단계는 하드웨어와 소프트웨어로 구분할 수 있습니다.

### 하드웨어 지원과 옵션

만일 단순히 `make defconfig` 를 사용하면 정상적으로 작동하지 않는 컴퓨터가 될 수 있습니다.

뿐만 아니라, 부분적으로만 지원되는 커널이 될 수도 있습니다. 아마도 가능하지만 설정이 켜져 있지 않는 하드웨어 센서가 있을 지도 모릅니다. 필요한 모든 것들이 켜져 있는지 어떻게 확인할 수 있을까요?

하드웨어 지원을 위한 설정 옵션들입니다:

- 쉬움:
  - genkernel
  - `make allmodconfig`
  - `.config`를 우분투/페도라/등등으로부터 복사하여 사용. 예시: [http://kernel.ubuntu.com/~kernel-ppa/configs/xenial/linux/](http://kernel.ubuntu.com/~kernel-ppa/configs/xenial/linux/)
  - 사용하고자 하는 커널 버전을 사용하는 Live CD로 부팅한 후, `make localmodconfig`를 활용하고 config파일을 저장.
  - 커널 시드. 예. [http://www.elilabs.com/~pappy/](http://www.elilabs.com/~pappy/) (테스트되지 않음; 권장하지 않음)
  - [AutoKernConf](https://cateee.net/autokernconf/) (테스트되지 않음; 권장하지 않음)
- 시간 소모:
  - lspci / lsusb 를 훑으며 장치/제조사 번호와 관련 설정을 찾는 방법
  - `make menuconfig`의 모든 옵션을 훑으며 관련 있어 보이는 것을 사용으로 설정

`genkernel`, `allmodconfig`와 `localmodconfig`는 모두 유용하며 어떤 하드웨어 설정을 사용하기로 설정해야 할지 고민하기 싫을 때 사용하는 것을 권장합니다.

반면에, 커널 시드는 anti-pattern 성격을 가진다고 봅니다. 제작자들은 "시드는 정상 작동하는 현실판 '`make defconfig`'"라고 주장합니다. 저라면 차라리 업스트림에서 기본값으로 설정된 것들을 신뢰하고 실제로 대다수의 $ARCH(해당 아키텍쳐) 사용자에게 주어진 기본값이 적정한지 고민하고 변경하는 수고를 하는 편을 택하겠습니다. 기본값도 무난하고 커널 시드를 사용할 필요가 없다고 생각합니다.

"from-scratch(처음부터)" 설정하는 것들의 장점들:

- 5-10배 빠른 컴파일(선택된 것들에 의존함). 예: 쿼드코어 Intel i7 7700K CPU @ 4.5GHz, Linux 4.12, gcc 5.4.0, 모두 램디스크 상에서 `make -j4`가
  - `allnoconfig`: 55s user, 6s system, 20s wall
  - `defconfig`: 472s user, 38s system, 145s wall
  - `defconfig` + 사소한 변경점: 624s user, 47s system, 190s wall
  - [무작위 우분투 설정파일](http://kernel.ubuntu.com/~kernel-ppa/configs/xenial/linux/4.4.0-93.116/amd64-config.flavour.generic): 3950s user, 312s system, 1166s wall
- 200MB 까지 디스크 공간 절약 가능(불필요한 모듈 없을시)
- 부팅시 수 밀리초/수 초의 시간을 절약 가능
- 공략 면적 감소
- 작은 메모리 절약
- 좋은 학습 경험

대부분의 사람들의 경우, `gentoo-sources` + `genkernel` 혹은 다른 유명 배포판의 커널을 계속 사용하는게 낫다고 생각합니다.

### 소프트웨어 지원과 옵션들

systemd와 같은 소프트웨어는 커널의 특정 기능이 정상적으로 작동할 것을 요구합니다.

젠투 사용자로서 아마도 아래와 같은 메시지들을 자주 접하였을 것입니다:

```bash
ERROR: setup
    CONFIG_AUDITSYSCALL: is not set when it should be.
```

다시 말하지만, `make defconfig`는 이것을 잡아내지 못합니다.

전통적인 접근방식은 `.config`를 만들어낸 후, 계속해서 `make oldconfig` 혹은 `make olddefconfig`를 하는 방법이었습니다. `.config`파`일은 수 천줄을 가지고 있습니다. 배타적으로 켜거나 끈 옵션들이 기본값들과 함께 뒤섞여 있습니다.

문제점 1: 만일 기본값 자체가 변경되면(이 유명한 [라이너스의 불만](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b4b8cbf679c4866a523a35d1454884a31bd5d8dc)처럼) 새로운 기본값은 효과가 없습니다. 당신은 과거에 영원히 머물게 됩니다.

믿기지 않는다구요?

```bash
~/linux$ git checkout 'b4b8cbf679c4866a523a35d1454884a31bd5d8dc^' # note caret
~/linux$ make mrproper defconfig
~/linux$ grep CNN55XX .config
CONFIG_CRYPTO_DEV_NITROX_CNN55XX=m
~/linux$ git checkout 'b4b8cbf679c4866a523a35d1454884a31bd5d8dc'
~/linux$ make oldconfig
~/linux$ grep CNN55XX .config
CONFIG_CRYPTO_DEV_NITROX_CNN55XX=m
```

문제점 2: `CONFIG_TUN is required by openvpn`과 같은 주석을 다는 것이 어렵고 결과적으로 아무도 `.config`파일은 문서화하지 않게됩니다.

몇 개의 강한 반박들은:

- 수정은 완벽히 자동화되어야 한다. 인생은 짧다. 가능한 자동화할 수 있는지 시도해야한다.
- 수정은 버전 관리가 가능해야 한다.
- 수정은 문서화 되어야 한다. ( 왜 **X** 옵션이 켜져야 하는지? ).

그래서 제가 하는 방식은:

- 필요한 패키지를 `emerge`합니다.( 예. systemd, docker, openvpn )
- 모든 에러/주의 메시지를 모읍니다.
- 커널의 `scripts/config` 스크립트를 통해 필요한 옵션을 켭니다. 하드웨어 수정은 직접 하기 때문에 제 스크립트는 아래와 같이 생겼습니다.

```bash
$ per-machine hardware config
"$(dirname "$0")/hardware/$(hostname).sh"

$ /proc/config.gz
./scripts/config --enable IKCONFIG      # tristate
./scripts/config --enable IKCONFIG_PROC # boolean

$ gentoo-sources ( https://gitweb.gentoo.org/proj/linux-patches.git/tree/4567_distro-Gentoo-Kconfig.patch ):
./scripts/config --enable DEVTMPFS # boolean
./scripts/config --enable TMPFS    # boolean
./scripts/config --enable UNIX     # tristate
./scripts/config --enable SHMEM    # boolean

$ gentoo/portage:
./scripts/config --enable CGROUPS    # boolean
./scripts/config --enable NAMESPACES # boolean
./scripts/config --enable IPC_NS     # boolean
./scripts/config --enable NET_NS     # boolean
./scripts/config --enable SYSVIPC    # boolean

$ openrc/runit support
./scripts/config --enable BINFMT_SCRIPT # tristate

$ Recommended by the Gentoo Handbook: "Also select Maintain a devtmpfs file
$ system to mount at /dev so that critical device files are already available
$ early in the boot process (CONFIG_DEVTMPFS and DEVTMPFS_MOUNT)":
./scripts/config --enable DEVTMPFS       # boolean
./scripts/config --enable DEVTMPFS_MOUNT # boolean

$ required for CHECKPOINT_RESTORE
./scripts/config --enable EXPERT # boolean

$ systemd -- gentoo ebuild:
./scripts/config --enable AUTOFS4_FS           # tristate
./scripts/config --enable BLK_DEV_BSG          # boolean
./scripts/config --enable CGROUPS              # boolean
./scripts/config --enable CHECKPOINT_RESTORE   # boolean
./scripts/config --enable CRYPTO_HMAC          # tristate
./scripts/config --enable CRYPTO_SHA256        # tristate
./scripts/config --enable CRYPTO_USER_API_HASH # tristate
$ ./scripts/config --enable DEVPTS_MULTIPLE_INSTANCES # removed -- https://github.com/torvalds/linux/commit/eedf265aa003
./scripts/config --enable DMIID                # boolean
./scripts/config --enable EPOLL                # boolean
./scripts/config --enable FANOTIFY             # boolean
./scripts/config --enable FHANDLE              # boolean
./scripts/config --enable INOTIFY_USER         # boolean
./scripts/config --enable IPV6                 # tristate
./scripts/config --enable NET                  # boolean
./scripts/config --enable NET_NS               # boolean
./scripts/config --enable PROC_FS              # boolean
./scripts/config --enable SECCOMP              # boolean
./scripts/config --enable SECCOMP_FILTER       # boolean
./scripts/config --enable SIGNALFD             # boolean
./scripts/config --enable SYSFS                # boolean
./scripts/config --enable TIMERFD              # boolean
./scripts/config --enable TMPFS_POSIX_ACL      # boolean
./scripts/config --enable TMPFS_XATTR          # boolean
./scripts/config --enable ANON_INODES          # boolean
./scripts/config --enable BLOCK                # boolean
./scripts/config --enable EVENTFD              # boolean
./scripts/config --enable FSNOTIFY             # boolean
./scripts/config --enable INET                 # boolean
./scripts/config --enable NLATTR               # boolean

$ systemd -- extra things from https://cgit.freedesktop.org/systemd/systemd/tree/README
./scripts/config --enable DEVTMPFS               # boolean
./scripts/config --disable SYSFS_DEPRECATED      # boolean
./scripts/config --set-str UEVENT_HELPER_PATH ""
./scripts/config --disable FW_LOADER_USER_HELPER # boolean
./scripts/config --enable EXT4_FS_POSIX_ACL      # boolean
./scripts/config --enable BTRFS_FS_POSIX_ACL     # boolean
./scripts/config --enable CGROUP_SCHED           # boolean
./scripts/config --enable FAIR_GROUP_SCHED       # boolean
./scripts/config --enable CFS_BANDWIDTH          # boolean
./scripts/config --enable SCHEDSTATS             # boolean
./scripts/config --enable SCHED_DEBUG            # boolean
./scripts/config --enable EFIVAR_FS              # tristate
./scripts/config --enable EFI_PARTITION          # boolean
$ ./scripts/config --disable RT_GROUP_SCHED      # boolean, docker wants this
$ ./scripts/config --disable AUDIT               # boolean, conflicts with consolekit

$ chromium
./scripts/config --enable PID_NS          # boolean
./scripts/config --enable NET_NS          # boolean
./scripts/config --enable SECCOMP_FILTER  # boolean
./scripts/config --enable USER_NS         # boolean
./scripts/config --enable ADVISE_SYSCALLS # boolean
./scripts/config --disable COMPAT_VDSO    # boolean

$ qemu for kernel dev
./scripts/config --module VIRTIO_PCI    # tristate
./scripts/config --module VIRTIO_BLK    # tristate
./scripts/config --module VIRTIO_NET    # tristate
./scripts/config --module 9P_FS         # tristate
./scripts/config --module NET_9P        # tristate
./scripts/config --module NET_9P_VIRTIO # tristate

$ lm_sensors
./scripts/config --enable I2C_CHARDEV # tristate

$ cryptsetup, luks (according to gentoo wiki page)
./scripts/config --enable BLK_DEV_DM               # tristate
./scripts/config --enable DM_CRYPT                 # tristate
./scripts/config --enable CRYPTO_AES_X86_64        # tristate
./scripts/config --enable CRYPTO_XTS               # tristate
./scripts/config --enable CRYPTO_SHA256            # tristate
./scripts/config --enable CRYPTO_USER_API_SKCIPHER # tristate

$ openvpn
./scripts/config --module TUN # tristate

$ cups
./scripts/config --module USB_PRINTER # tristate

$ pulseaudio
./scripts/config --set-val SND_HDA_PREALLOC_SIZE 2048

$ Docker (useful: contrib/check-config.sh)
$ "Generally Necessary"
./scripts/config --enable NAMESPACES                   # boolean
./scripts/config --enable NET_NS                       # boolean
./scripts/config --enable PID_NS                       # boolean
./scripts/config --enable IPC_NS                       # boolean
./scripts/config --enable UTS_NS                       # boolean
./scripts/config --enable CGROUPS                      # boolean
./scripts/config --enable CGROUP_CPUACCT               # boolean
./scripts/config --enable CGROUP_DEVICE                # boolean
./scripts/config --enable CGROUP_FREEZER               # boolean
./scripts/config --enable CGROUP_SCHED                 # boolean
./scripts/config --enable CPUSETS                      # boolean
./scripts/config --enable MEMCG                        # boolean
./scripts/config --enable KEYS                         # boolean
./scripts/config --module VETH                         # tristate
./scripts/config --module BRIDGE                       # tristate
./scripts/config --module NETFILTER_ADVANCED           # boolean, implicit requirement for BRIDGE_NETFILTER
./scripts/config --module BRIDGE_NETFILTER             # tristate
./scripts/config --module NF_NAT_IPV4                  # tristate
./scripts/config --module IP_NF_FILTER                 # tristate
./scripts/config --module IP_NF_TARGET_MASQUERADE      # tristate
./scripts/config --module NETFILTER_XT_MATCH_ADDRTYPE  # tristate
./scripts/config --module NETFILTER_XT_MATCH_CONNTRACK # tristate
./scripts/config --module NETFILTER_XT_MATCH_IPVS      # tristate
./scripts/config --module IP_NF_NAT                    # tristate
./scripts/config --module NF_NAT                       # tristate
./scripts/config --enable NF_NAT_NEEDED                # boolean
./scripts/config --enable POSIX_MQUEUE                 # boolean
$ "Optional Features"
./scripts/config --enable USER_NS                      # boolean
./scripts/config --enable SECCOMP                      # boolean
./scripts/config --enable CGROUP_PIDS                  # boolean
./scripts/config --enable MEMCG_SWAP                   # boolean
./scripts/config --enable MEMCG_SWAP_ENABLED           # boolean
./scripts/config --enable LEGACY_VSYSCALL_EMULATE      # boolean
./scripts/config --enable BLK_CGROUP                   # boolean
./scripts/config --enable BLK_DEV_THROTTLING           # boolean
./scripts/config --module IOSCHED_CFQ                  # tristate
./scripts/config --enable CFQ_GROUP_IOSCHED            # boolean
./scripts/config --enable CGROUP_PERF                  # boolean
./scripts/config --enable CGROUP_HUGETLB               # boolean
./scripts/config --module NET_CLS_CGROUP               # tristate
./scripts/config --enable CGROUP_NET_PRIO              # boolean
./scripts/config --enable CFS_BANDWIDTH                # boolean
./scripts/config --enable FAIR_GROUP_SCHED             # boolean
./scripts/config --enable RT_GROUP_SCHED               # boolean
./scripts/config --module IP_VS                        # tristate
./scripts/config --enable IP_VS_NFCT                   # boolean
./scripts/config --module IP_VS_RR                     # tristate
./scripts/config --enable EXT4_FS                      # tristate
./scripts/config --enable EXT4_FS_POSIX_ACL            # boolean
./scripts/config --enable EXT4_FS_SECURITY             # boolean
$ "Network Drivers/overlay"
./scripts/config --module VXLAN                        # tristate
$ "Network Drivers/overlay/Optional (for encrypted networks)":
./scripts/config --enable CRYPTO                       # tristate
./scripts/config --enable CRYPTO_AEAD                  # tristate
./scripts/config --enable CRYPTO_GCM                   # tristate
./scripts/config --enable CRYPTO_SEQIV                 # tristate
./scripts/config --enable CRYPTO_GHASH                 # tristate
./scripts/config --enable XFRM                         # boolean
./scripts/config --enable XFRM_USER                    # tristate
./scripts/config --enable XFRM_ALGO                    # tristate
./scripts/config --module INET_ESP                     # tristate
./scripts/config --enable INET_XFRM_MODE_TRANSPORT     # tristate
$ "Network Drivers/ipvlan"
./scripts/config --enable NET_L3_MASTER_DEV            # boolean, required for IPVLAN
./scripts/config --module IPVLAN                       # tristate
$ macvlan
./scripts/config --module MACVLAN                      # tristate
./scripts/config --module DUMMY                        # tristate
$ "ftp,tftp client in container"
$ ./scripts/config --module NF_NAT_FTP                   # tristate
$ ./scripts/config --module NF_CONNTRACK_FTP             # tristate
$ ./scripts/config --module NF_NAT_TFTP                  # tristate
$ ./scripts/config --module NF_CONNTRACK_TFTP            # tristate
$ "Storage Drivers"
./scripts/config --enable BTRFS_FS                     # tristate
./scripts/config --enable BTRFS_FS_POSIX_ACL           # boolean
./scripts/config --enable BLK_DEV_DM                   # tristate
./scripts/config --enable DM_THIN_PROVISIONING         # tristate
./scripts/config --module OVERLAY_FS                   # tristate
$ From the gentoo ebuild
./scripts/config --enable SYSVIPC                      # boolean
./scripts/config --enable IP_VS_PROTO_TCP              # boolean
./scripts/config --enable IP_VS_PROTO_UDP              # boolean

$ libvirt
./scripts/config --module MACVTAP # tristate

$ sys-auth/consolekit-1.1.2
./scripts/config --enable AUDIT        # boolean, required for AUDITSYSCALL
./scripts/config --enable AUDITSYSCALL # boolean

$ SCSI disk support
./scripts/config --enable BLK_DEV_SD # tristate

./scripts/config --enable  EXT2_FS     # tristate
./scripts/config --disable EXT3_FS     # tristate, "This config option is here only for backward compatibility. ext3 filesystem is now handled by the ext4 driver"
./scripts/config --enable  EXT4_FS     # tristate
./scripts/config --enable  VFAT_FS     # tristate
./scripts/config --module  REISERFS_FS # tristate
./scripts/config --enable  XFS_FS      # tristate
./scripts/config --enable  BTRFS_FS    # tristate
./scripts/config --enable  FUSE_FS     # tristate
./scripts/config --enable  ISO9660_FS  # tristate
./scripts/config --enable  PROC_FS     # boolean
./scripts/config --enable  TMPFS       # boolean

$ USB input devices
./scripts/config --enable HID_GENERIC  # tristate
./scripts/config --enable USB_HID      # tristate
./scripts/config --enable USB_SUPPORT  # boolean
./scripts/config --enable USB_XHCI_HCD # tristate
./scripts/config --enable USB_EHCI_HCD # tristate
./scripts/config --enable USB_OHCI_HCD # tristate
./scripts/config --enable USB_UAS      # tristate, "USB attached SCSI"

$ support 32-bit executables
./scripts/config --enable IA32_EMULATION # boolean

$ GPT, EFI, UEFI
./scripts/config --enable  PARTITION_ADVANCED # boolean
./scripts/config --enable  EFI_PARTITION      # boolean
./scripts/config --enable  EFI                # boolean
./scripts/config --enable  EFI_STUB           # boolean
./scripts/config --enable  EFI_MIXED          # boolean
./scripts/config --enable  EFI_VARS           # tristate
./scripts/config --disable OSF_PARTITION      # boolean, Alpha servers
./scripts/config --disable AMIGA_PARTITION    # boolean
./scripts/config --disable SGI_PARTITION      # boolean
./scripts/config --disable SUN_PARTITION      # boolean
./scripts/config --disable KARMA_PARTITION    # boolean
./scripts/config --enable  MAC_PARTITION      # boolean

./scripts/config --enable MAGIC_SYSRQ # boolean

$ app-emulation/qemu
./scripts/config --module KVM       # tristate
./scripts/config --module VHOST_NET # tristate

$ https://lwn.net/Articles/680989/
$ https://lwn.net/Articles/681763/
./scripts/config --enable BLK_WBT    # boolean
./scripts/config --enable BLK_WBT_SQ # boolean
./scripts/config --enable BLK_WBT_MQ # boolean

$ http://algo.ing.unimo.it/people/paolo/disk_sched/
./scripts/config --module IOSCHED_BFQ # tristate

$ https://www.youtube.com/watch?v=y5KPryOHwk8
$ https://en.wikipedia.org/wiki/Active_queue_management
$ https://lwn.net/Articles/616241/
./scripts/config --enable NET_SCHED        # boolean
./scripts/config --module IFB              # tristate
./scripts/config --module NET_SCH_HTB      # tristate
./scripts/config --module NET_SCH_CBQ      # tristate
./scripts/config --module NET_SCH_HFSC     # tristate
./scripts/config --module NET_SCH_FQ       # tristate
./scripts/config --module NET_SCH_FQ_CODEL # tristate
./scripts/config --module NET_SCH_SFB      # tristate
./scripts/config --module NET_SCH_INGRESS  # tristate
./scripts/config --module NET_CLS_U32      # tristate

$ https://lwn.net/Articles/758353/
./scripts/config --module NET_SCH_CAKE # tristate
./scripts/config --module NET_ACT_MIRRED # tristate
./scripts/config --module NET_SCH_PIE # tristate

$ https://news.ycombinator.com/item?id=14813723
./scripts/config --module TCP_CONG_BBR # tristate

$ IP ECMP
./scripts/config --enable IP_ROUTE_MULTIPATH # boolean

$ source-based IP routing
./scripts/config --enable IP_MULTIPLE_TABLES # boolean

$ bridging
./scripts/config --module BRIDGE # tristate
$ multicast
./scripts/config --enable BRIDGE_IGMP_SNOOPING # boolean

$ speed up tcpdump
./scripts/config --enable BPF_JIT # boolean

$ timing packets / ptp (Precision Time Protocol)
./scripts/config --enable NETWORK_PHY_TIMESTAMPING # boolean

./scripts/config --module IP_VS   # tristate
./scripts/config --module BONDING # tristate

$ boot_delay=X support
./scripts/config --enable BOOT_PRINTK_DELAY # boolean

$ thp, compaction
./scripts/config --enable TRANSPARENT_HUGEPAGE
./scripts/config --enable TRANSPARENT_HUGEPAGE_ALWAYS

$ dev-util/bcc
./scripts/config --enable BPF_SYSCALL     # boolean
./scripts/config --module NET_CLS_BPF     # tristate
./scripts/config --module NET_ACT_BPF     # tristate
./scripts/config --enable BPF_EVENTS      # boolean
./scripts/config --enable DEBUG_INFO      # boolean
./scripts/config --enable FUNCTION_TRACER # boolean
./scripts/config --enable KALLSYMS_ALL    # boolean

$ https://lwn.net/Articles/759781/
./scripts/config --enable PSI # bool
./scripts/config --enable PSI_DEFAULT_DISABLED # bool

$ https://www.phoronix.com/scan.php?page=article&item=linux_2637_video&num=1
./scripts/config --enable SCHED_AUTOGROUP # boolean

$ and so on
```

적용:

```bash
/usr/src/linux-stable-git-4.12.8$ make mrproper defconfig
/usr/src/linux-stable-git-4.12.8$ ~/kernel-config.sh
/usr/src/linux-stable-git-4.12.8$ make olddefconfig
```

주의점 1: 가끔 수정은 암시적입니다. --- 즉, 다른 수정에 의해 켜지는 경우가 있습니다:

```bash
Symbol: TAP [=n]
Type  : tristate
  Defined at drivers/net/Kconfig:304
  Depends on: NETDEVICES [=y] && NET_CORE [=y]
  Selected by: MACVTAP [=n] && NETDEVICES [=y] && NET_CORE [=y] && MACVLAN [=n] && INET [=y] || IPVTAP [=n] && NETDEVICES [=y] && NET_CORE [=y] && IPVLAN [=n] && INET [=y]
```

만약 `CONFIG_TAP` 을 켜고 싶다면, "Selected by" 부분은 반드시 만족시켜야 합니다. `./scripts/config --enable TAP` 명령을 통해 정상적으로 작동하는 것처럼 보이지만, `make olddefconfig`이 다시 되돌립니다.

주의점 2: 일부 설정은 다른 것들에 의존합니다:

```bash
Symbol: BRIDGE_NETFILTER [=n]
Type  : tristate
Prompt: Bridged IP/ARP packets filtering
  Location:
    -> Networking support (NET [=y])
      -> Networking options
        -> Network packet filtering framework (Netfilter) (NETFILTER [=y])
(1)       -> Advanced netfilter configuration (NETFILTER_ADVANCED [=n])
  Defined at net/Kconfig:187
  Depends on: NET [=y] && BRIDGE [=n] && NETFILTER [=y] && INET [=y] && NETFILTER_ADVANCED [=n]
```

마찬가지로, 예를들어 만약 `NET=n`, `./scripts/config --enable BRIDGE_NETFILTER` 가 NET을 가능 설정으로 만들지 않는다면 `NET=n` 과 `BRIDGE_NETFILTER=n` 이 되고 말것입니다.

이런 깨진 의존성을 발견하여 해결하는 방법은:

```bash
/usr/src/linux-stable-git-4.12.8$ grep -Po '(?<=--enable )[^# ]+' ~/kernel-config.sh | sed 's/^CONFIG_//' | while read kconf; do if ! grep -q "^CONFIG_$kconf=y" .config; then echo "$kconf not set"; fi; done
```

추가할것: 모든 것을 확인하는 스크립트 작성하기

마이크로코드 업데이트를 잊지 마세요. 예: 인텔 CPU:

```bash
$./scripts/config --enable FIRMWARE_IN_KERNEL
$./scripts/config --set-str EXTRA_FIRMWARE "$(iucode_tool -L /lib/firmware/intel-ucode | grep "$(iucode_tool -S 2>&1 | grep -Po '(?<=signature ).*$')" -B 1 | grep -Po '(?<=/lib/firmware/)intel-ucode/.*')"
$./scripts/config --set-str EXTRA_FIRMWARE_DIR "/lib/firmware"
```

### 커널 빌드와 설치

`make install`은 `sys-apps/debianutils`를 필요로 합니다.

```bash
/usr/src$ eselect kernel list
/usr/src$ sudo eselect kernel set "linux-stable-git-4.12.8"
/usr/src$ cd linux
/usr/src/linux$ git am -3 ~/kernel-patches/*.patch
/usr/src/linux$ nice /usr/bin/time -v make KCFLAGS="-march=native" -j "$(nproc)" olddefconfig all

/usr/src/linux# (mountpoint -q /boot || mount /boot) && make install modules_install
/usr/src/linux# emerge -avtq '@module-rebuild'

$ if you need an initrd:
/usr/src/linux# dracut -a crypt -o zfs "/boot/initramfs-$(make kernelrelease).img" --kver "$(make kernelrelease)"

$ optional cleanup:
/usr/src/linux# eclean-kernel --list-kernels && eclean-kernel --ask --destructive --exclude config

/usr/src/linux# grub-mkconfig -o /boot/grub/grub.cfg

$ optional tools:
/usr/src/linux$ make KCFLAGS="-march=native" -j "$(nproc)" -C ./tools/power/x86/turbostat
/usr/src/linux$ make KCFLAGS="-march=native" -j "$(nproc)" -C ./tools/perf
```

## 추후 계획

- 더 나은 scripts/config 앱, 적절한 의존성 관리 기능 추가

## 비판? 칭찬?

어떤 피드백이라도 감사하게 생각합니다
