---
title:  "젠투 리눅스 바이너리 패키지"
date:   2024-01-11
url: 'gentoo-binary'
---

## Preface

[핸드북](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Optional:_Adding_a_binary_package_host)에서 명시하듯
23년 12월 부로, 젠투에서 바이너리 패키지 설치를 공식 지원합니다.

이번 업데이트에 따라 VPS에 새로운 젠투 머신을 설치하며 과정에서 바이너리 패키지를 사용하는 방법을 정리합니다.

## `binrepos.conf`

이제 `/etc/portage/repos.conf/gentoo.conf`와 더불어, 바이너리 패키지를 위한 `/etc/portage/binrepos.conf/gentoo.conf`를 작성합니다.
(예시에서는, 카이스트 미러를 활용합니다)

```file
[binhost]
priority = 9999
sync-uri = http://ftp.kaist.ac.kr/gentoo/releases/amd64/binpackages/17.1/x86-64/
```

## Make binary default

기본값으로 바이너리 패키지를 설치하고자 하시는 분들은 `/etc/portage/make.conf`에 해당 설정을 추가합니다.

```file
FEATURES="${FEATURES} getbinpkg"
```

## `Emerge`

이제 `portage` 패키지 매니저에서 바이너리 패키지 관련 옵션을 제공합니다.

- `--getbinpkg`, `-g`: 원격 바이너리 패키지 호스트로부터 바이너리 패키지를 다운로드. 찾을 수 없는 경우 일반(소스-베이스) 패키지를 다운로드합니다
- `--getbinpkgonly`, `-G`: `--getbinpkg`, `-g`와 유사하지만 바이너리 패키지를 찾을 수 없는 경우 실패함.

## Error: `OpenPGP` signing and verification fail

이전 후 첫 패키지 설치 과정에서, 무결점 확인 오류가 발생하는 경우가 있습니다.

로컬 환경의 신뢰할 수 있는 GnuPG, OpenPGP 키를 업데이트하는 소프트웨어 `getuto`를 실행하여 해결합니다.

```console
# getuto
```

부가적인 사항은 해당 [위키 페이지](https://wiki.gentoo.org/wiki/Binary_package_guide)에서 확인하시기 바랍니다.
