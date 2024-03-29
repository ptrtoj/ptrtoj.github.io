---
title:  "유닉스 역사"
date:   2019-01-01
url: 'unix'
---

## 서문

UNIX 역사에 대해서는 따로 궁금하지 않으신 분들은 바로 ‘[리눅스 배포판 종류]({{< ref "distro" >}})’로 이동하시면 됩니다.

## UNIX

UNIX(이하 '유닉스')의 역사를 설명하기 위해서는 1960년대 중반까지 거슬러 올라갑니다. 당시 MIT, AT&T의 벨 연구소, GE(제너럴 일렉트릭)은 GE-645 메인프레임에서 사용하기 위한 Multics(이하 '멀틱스')라는 실험적인 시-분할 운영 체제(현대에 당연한 다중 동시 사용자, 멀티 태스킹이 가능한 운영 체제의 조상) 개발에 참여 중이었습니다. 멀틱스의 크기나 복잡도에 실망한 벨 연구소는 차츰 연구에서 손을 떼기 시작했습니다만, 마지막까지 연구에 관심을 갖던 켄 톰슨, 데니스 리치, 덕 매킬로이, 조 오산나는 그렇지 않았습니다. 넷은 시기적으로 일렀을 뿐이라는 판단 아래, 조금 더 작은 규모로 실험을 계속 해보고자 했습니다.

> UNIX® ?
>
> → UNIX(유닉스)는 [The Open Group](https://www.opengroup.org/)의 공식 상표입니다.
>
> 이는 [제7차, 오픈 그룹 기본 제원](https://pubs.opengroup.org/onlinepubs/9699919799/) (POSIX.1-2008 또는 IEEE Std 1003.1 - 2008)에 부합하는 컴퓨터 운영 체제 및 도구를 가리킬 때 사용됩니다.

1969년 켄 톰슨은 거의 사용되지 않던 DEC(Digital Equipment Corporation)사의 PDP-7을 연구소에서 발견합니다. 이 PDP-7을 통해 톰슨, 리치 등이 계층 파일 시스템(Hierarchical File System)을 구현합니다. 이것이 유닉스 계열 운영 체제에 아직도 전해져내려오는 `/`, `/bin`, `/usr`, `/etc` 등이 됩니다. 거기서 멈추지 않고 컴퓨터 프로세스, 디바이스 파일(`/dev` 아래), 커맨드 라인 인터프리터, 어셈블러, 에디터, 쉘 등을 개발해냅니다. 마지막으로 더글라스 매킬로이가 TMG 컴파일러-컴파일러를 PDF-7 어셈블리어로 포팅하며 최초의 유닉스 고레벨 언어를 탄생시킵니다. 톰슨은 이 도구를 활용해 B 프로그래밍 언어의 첫 버전을 개발합니다.

이렇게 탄생한 첫 운영 체제는 벨 연구소의 지원으로 개발된 것도 아니었고, 멀티 태스킹이 가능하지도 않았으며, 이름마저도 없었습니다. 단지 기술자들이 이루고자 했던 목표를 달성하기 위해 밤낮을 가리지 않고 노력한 결과물일 뿐이었습니다. 처음 지어진 이름은 Unics(**Uni**plexed **I**nformation and **C**omputing **S**ervice; 유넉스로 읽음)였지만, 이후 "어차피 아무도 기억 못할테니까"라는 농담을 덧붙이며 브라이언 커니건이 유닉스(UNIX)라는 이름의 출처가 본인이었다고 주장합니다. 데니스 리치와 덕 매킬로이도 나중에 커니건에게 작명의 공을 돌렸습니다.

이제 PDP-7보다 더 큰 하드웨어에서 멀티태스킹까지 가능한 운영체제를 만들어보고자 했던 컴퓨터 연구원들은 당시 법률 관련 부서에서 문서 편집기가 필요하다(당시 벨 연구소는 수많은 특허 출원을 위해 변호사/변리사들의 특허 관련 서류 작성량이 어마어마했다고 알려짐)는 점을 알고, PDP-11을 사주면 문서 편집을 위한 환상적인 소프트웨어를 제작해 줄 것임을 약속하여 재정 지원을 받아냅니다.

1970년 처음으로 PDP-11/20을 받게 된 연구원들은 순식간에 roff로 알려진 문서 양식 편집 소프트웨어를 만들고 문서 에디터까지 추가합니다. 이 프로그램들은 모두 PDP-11/20 어셈블리어로 작성되었습니다. 벨 연구소에서는 이 프로그램들을 활용해 편리하게 특허 관련 서류들을 작성하기 시작했습니다. (roff는 금방 troff로 진화함)

벨 연구소의 다른 부서에서 DEC PDP-11을 구매하게 되었을 때는 이미 DEC에서 제공하는 자체 운영 체제가 아니라 유닉스를 활용하기로 결정합니다. 유닉스의 복잡성은 계속 커져가는데 사용하고자 하는 부서와 연구팀은 증가하면서 매뉴얼의 필요성이 대두됩니다. 이로써 1971년 11월 3일 유닉스 프로그래밍 매뉴얼이 만들어집니다. 현재까지 사용되는 `man` 명령어의 시작이 이때였습니다.

1973년 유닉스 버전 4가 고레벨 언어인 C로 다시 쓰입니다. 이로써 어셈블리어 자체는 버전 8 까지도 살아남았으나, 소프트웨어의 이식성(다른 명령어 세트를 가진 컴퓨터에서도 컴파일을 통해 쉽게 사용할 수 있는 기능)이 제시됩니다. 같은 해, AT&T의 유닉스 버전 5가 출시되면서 교육 기관에 라이선스를 제공하게 됩니다 (기업에는 1976년 유닉스 버전 6부터 라이선스가 제공됨 - 상업 라이선스는 미화 2만달러, 2020년 가치로 96,190$로 알려짐). 외부에 제공되기 시작한 버전 5, 버전 6를 통해 [라이언이 쓴 소스 코드를 포함 UNIX 버전 6 주석](https://www.amazon.com/-/ko/dp/1573980137/)이라는 전설적인 책을 남길 수 있게 됩니다.

## BSD

애초에 BSD는 유닉스 복사본도, 심지어 확연히 다른 버전인 것도 아니었습니다. 단지 AT&T가 소유하는 소스 코드 위에 몇 개의 추가적으로 도움이 될 유틸리티를 추가했을 뿐이었습니다.

1975년 켄 톰슨은 휴식차 벨 연구소를 떠나 방문 교수겸 버클리, 캘리포니아 대학에 옵니다. 이 때 유닉스 버전 6의 설치를 돕고 시스템에 파스칼을 이식하기 시작했습니다. 대학원생이었던 척 해일리와 **빌 조이**는 톰슨의 파스칼을 개선하는 한편 향상된 텍스트 에디터인 **ex**를 개발합니다. 이런 개선 사항에 관심이 생긴 주변 대학들이 생기자 조이는 첫 번째 버클리 소프트웨어 배포판(1BSD)를 제작하여 1978년 3월 9일 처음으로 배포합니다. (1BSD는 유닉스 버전 6의 애드온 정도였으며 하나의 독립적이고 완전한 형태의 운영 체제가 아니었습니다.)

2BSD는 1979년 5월 발매됩니다. 1BSD로부터 업데이트를 하였고 2개의 신규 프로그램을 포함하였습니다. 하나는 조이가 쓴 **vi 텍스트 에디터**이고 다른 하나는 C 쉘이었습니다.

## 유닉스 전쟁

### 서막

유닉스를 만든건 AT&T였으나, 1980년대에 들어서자 캘리포니아 대학, 버클리 컴퓨터 시스템 연구소가 비상업적 유닉스 개발자들을 이끌고 있는 형국이 되었습니다.

1984년, System V를 기준으로 만들고자 AT&T가 노력하던 시점에 일부 제조사들이 모여 X/Open Standard(X/열린 표준)를 구성하면서 본격적으로 갈등으로 전환됩니다. X/Open Standard는 유닉스의 통일성을 더욱 증대시키고자 BSD 유닉스의 리드 아래에 썬 마이크로시스템즈 등이 함께 통일된 시스템 구축을 위해 시작되었습니다. 문제는 현재까지 네트워크 기술의 기준으로 자리잡고 있는 TCP/IP등이 System V에서는 구현이 안되어 있었으나 BSD 4.2에는 이식되어 있었다는 점입니다.

1985년, AT&T는 System V Interface Definition(SVID; 시스템 5 인터페이스 정의) 기준을 정립합니다. 그리고 다른 유닉스 기반 운영 체제들이 System V를 기준으로 브랜드화 하기를 요구합니다.

1988년 IEEE는 POSIX 제원을 설립하여 BSD와 System V 플랫폼 간의 호환성을 위한 기준들을 규정합니다. POSIX는 미 정부에 의해 다양한 시스템의 기준으로 강제되었습니다. X/Open Standard에 썬 시스템(SUN Microsystems)이 참여했고, 썬 시스템의 라이선스가 두려웠던 일부 제조사들은 따로 모여 Open Software Foundation(OSF; 자유 소프트웨어 재단)을 설립합니다. 이에 뒤질세라 1988년 AT&T는 UNIX International(UI; 국제 유닉스)를 만듭니다. 그리고 AT&T는 썬과 협업을 불사하며 다른 배포 버전보다 앞서나가기 위해 공격적인 전략을 펼칩니다. (그런데 썬이 System V.4로 독자노선..)

### 고소

이런 다툼이 1990년대까지 그대로 이어집니다. 그런데 BSD는 별 신경 쓰지 않고 본인들 운영 체제 개발에만 몰두합니다. 운영 체제를 POSIX 규격에 맞게 변형하기 시작하고 네트워크 관련 코드를 더 깔끔하게 정리합니다.

그런데, 돈 실리, 마이크 카렐스, 빌 졸리츠, 트렌트 하인 등 BSD 개발자들이 캘리포니아 대학을 나와 버클리 소프트웨어 디자인(BSDi)를 설립합니다. BSDi는 인텔 플랫폼을 위해 제작된 BSD 유닉스의 상업 버전을 팔기 시작합니다. 이때, AT&T의 무료 코드인양 홍보를 합니다.

1992년 결국, AT&T가 BSDi + 다른 BSD 관계자들을 고소합니다.

캘리포니아 대학이 맞고소를 하였습니다.

빌 졸리츠는 BSDi를 떠나 386BSD를 개발하고자 떠났고, 이 386BSD가 현재 FreeBSD, OpenBSD, NetBSD의 조상격입니다.

1994년 1월 버클리 측의 판정승으로 소송이 종료됩니다. 합의 조건은 "AT&T측이 다가오는 4.4BSD 출시와 관련해 버클리측이나 사용자, 배포자들을 더 이상 고소하지 않을 것"입니다.

> **리눅스 개발을 시작할 당시 386BSD가 있었다면 아마도 리눅스는 생겨나지 않았을지도 모릅니다.**
> **→ Linus Torvalds, 1993**

### 결과

소송으로 인해 BSD의 개발 속도가 둔화되었음은 따로 설명하지 않아도 당연한 부분입니다. 거기에 그치지 않고 잠재적 소비자가 될 수 있었던 많은 고객들이 추가적인 소송에 직접 휘말리거나 별 의미 없는 피해를 볼 것이 두려워 많은 점유율을 확보하는데 실패하였습니다.

> 리눅스는 종국에 x86 유닉스 시장을 차지할 궤도에 올랐다.
>
> … (중략) …
>
> 가까운 미래에 NT 보다는 오히려 리눅스가
> SCO의 가장 큰 위협이 될 것이라고 본다
>
> → [마이크로소프트 기밀 메모](https://en.wikipedia.org/wiki/Halloween_documents), 1998

와중에 1991년 라이너스 토르발즈라는 핀란드의 대학원생이 리눅스라는 커널을 개발하였습니다. 사용자들도 소송으로 시끄러운 BSD를 떠나 리눅스 커널을 들여다보기 시작합니다. 리눅스가 우연하다면 우연한 계기로 큰 부분을 점유하기 시작합니다.

## BSD 종류

### 개요

BSD는 리눅스 배포판들과는 다르게 하나의 운영 체제를 모두 제작합니다. 다시 말하면, 각종 소프트웨어 제작자의 패키지를 다운 받아 배포판 위에 설치할 수 있는 패키지 매니저를 제공하는 리눅스 배포판의 개념과 다릅니다. BSD는 설치할 수 있는 공식 레파지토리에 적극적인 검토 과정을 거쳐 안정적이라고 판단되는 패키지들을 모두 포함하여 배포됩니다. (슬랙웨어를 생각하시면 편합니다.)

> **BSD '배포판'??**
>
> 사실, BSD 진영에서는 '배포판'이라는 말을 싫어합니다. 아래에서도 적었지만 BSD는 하나의 전체 운영 체제를 개발하여 배포하기 때문입니다. BSD 진영에서는 계속해서 '운영 체제'로 부를 것을 요구합니다.

BSD 계열은 현재까지 명맥을 이어가는 배포판을 꼽아보면 굵직한 삼형제가 가장 유명합니다. FreeBSD, NetBSD, OpenBSD가 바로 그것입니다.

삼형제로 갈라지게된 이유는 운영체제 개발의 목적이 달랐기 때문입니다. FreeBSD는 일반 사용자에게 적합한 BSD로 나아가고자 했고, NetBSD는 그 어떤 하드웨어에서도 실행 가능한 BSD로, OpenBSD는 호환성을 조금 포기하더라도 가장 안전한 BSD로 가고자 했습니다.

시기상으론 NetBSD가 제일 먼저 386BSD로부터 포크되어 나왔습니다. 거의 동시에 FreeBSD가 386BSD의 느린 개발 속도에 불만을 품은 사용자들로부터 개발되었습니다. 조금의 시간이 지나고 Theo de Raadt가 NetBSD 개발진과의 목표, 의견차이를 좁히지 못하고 OpenBSD로 포크해 나옵니다.

물론, 위의 3개의 배포판만 있는 것이 아니라 다양한 소규모 배포판이 있습니다. 예를 들어, DragonFlyBSD, GhostBSD, NomadBSD, FuryBSD 등입니다. 각각 일반 사용자 편이성 확대, 그래픽 환경 설치 편이성 확대, 보안을 위한 USB용 운영 체제, 일반 사용자 편이성 확대를 위해 제작되었습니다.

그리고 각종 서버 특화 운영 체제를 개발할 때, 안정성에 방점을 찍는 BSD 계열을 적극적으로 활용합니다. 예를 들어, pfSense, TrueNas, OPNsense 등이 BSD 계열로부터 제작되었습니다.

그러나 많은 분들이 불편해하시는대로 BSD는 **일상용 운영 체제**로는 극히 일부 결함이 있습니다. 현재로서 가장 대표적인 것은 DRM 지원 불가로 인한 넷플릭스 시청을 할 수 없다는 점 (정작 넷플릭스 서버 시설은 FreeBSD를 활용하는 것으로 잘 알려져있다고...), 최신 출시 하드웨어 지원이 적다는 점 등이 있습니다.

### 개인 의견

### FreeBSD

#### 패키지 관리자 명령어

'BSD는 실제 UNIX 계보이다', 'FreeBSD는 리눅스 배포판과 달리 전체 OS의 형태로 배포된다' 등의 자주 듣는 이유를 벗어나, FreeBSD의 패키지 관리 명령어가 말그대로 `pkg`라는 점에서 가장 큰 매력을 느꼈습니다. `apt`, `apt-get`, `dnf`, `yum`, `emerge`, `pacman`, `xbps-install`, `eopkg`, `zypper`, `nixpkg`, `cards` 등 결국 유사하거나 같은 기능을 하는 것들임에도 온갖 미사여구 스타일의 이름을 사용하는 것에 어느 순간 환멸이 들어, 가장 간단한 패키지 매니저 명령어를 찾는 것을 기준으로 배포판 옮겨다니기를 멈추기로 했습니다. (Alpine Linux의 `apk`도 고려해 볼만 합니다. 다만, 데스크탑 환경 운영체제로 사용하기에 아직 무리가 많아 보입니다. 컨테이너 이미지 용도로 자주 씁니다.) OpenBSD의 `pkg_add` 명령어도 좋지만, 한 술 더 뜨는 FreeBSD의 `pkg`명령어가 단연코 가장 아름답고 UNIX 철학에 부합한다고 생각합니다.

### NetBSD

NetBSD가 필요 할만큼 다양한 하드웨어를 일반인이 접할 기회가 적어서, 굳이 오래 사용해본 적이 없습니다.

### OpenBSD

우선, 제가 기기를 운용하는 목적의 제 1 목표가 '보안'인 것은 아닙니다. OpenBSD는 보안적인 측면에서 강한 매력을 가진 것이 사실입니다. 추가적인 매력으로는 다른 모든 BSD 계열 혹은 리눅스 배포판들과 달리, 새로운 유저를 유입하기 위해 홍보를 하는데도 관심이 없고, 포럼 등에서도 유저들에게 친화적이기를 강제하지도 않습니다. 너무 단순하거나 쉬운 질문의 경우 드러내놓고 무시하는 경향이 보일 정도입니다.

다시 말해서, OpenBSD는 철저히 관심사가 동일한 '괴짜(Geek)'들이 모여 개발하는 BSD 입니다. (그래서 NetBSD의 창립자 중 하나였던 De Raadt가 같은 창립자들의 마지막 결재 하에, 쫓겨났을지도; -94년 12월) 그러한 경향성의 문제로 ZFS 같은 혁신적인 파일시스템도 아직 포팅이 되지 않고 있습니다.

> ZFS 포팅 관련해서는 내부적으로 다른 의견이 있습니다.
> 일부 리눅스 배포판에서도 겪고 있듯이, 인력 부족을 이유로 들기도 합니다.
> ZFS의 경우 엄청난 양의 소스 코드를 자랑하는데 이를 OpenBSD 기준의 코드 검사를 충족하려면 지금 공헌하는 인원수로는 엿부족인 것도 사실이긴 합니다.