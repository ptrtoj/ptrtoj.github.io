---
title:  "Gentoo Linux Binary Packages"
date:   2024-01-11
url: 'gentoo-binary'
---

## Preface

As stated in the [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Optional:_Adding_a_binary_package_host),
as of December 2023, Gentoo officially supports installing binary packages.

Following this update, I installed a new Gentoo machine on a VPS, and this post summarizes how to use binary packages along the way.

## `binrepos.conf`

In addition to `/etc/portage/repos.conf/gentoo.conf`, we now write `/etc/portage/binrepos.conf/gentoo.conf` for binary packages.
(This example uses the KAIST mirror.)

```file
[binhost]
priority = 9999
sync-uri = http://ftp.kaist.ac.kr/gentoo/releases/amd64/binpackages/17.1/x86-64/
```

## Make binary default

If you want binary packages to be installed by default, add the following setting to `/etc/portage/make.conf`.

```file
FEATURES="${FEATURES} getbinpkg"
```

## `Emerge`

The `portage` package manager now provides options related to binary packages.

- `--getbinpkg`, `-g`: Downloads binary packages from the remote binary package host. If one can't be found, it downloads the regular (source-based) package.
- `--getbinpkgonly`, `-G`: Similar to `--getbinpkg`, `-g`, but fails if a binary package can't be found.

## Error: `OpenPGP` signing and verification fail

During the first package installation after the switch, an integrity verification error may occur.

Fix it by running `getuto`, a tool that updates the trusted GnuPG/OpenPGP keys in your local environment.

```console
# getuto
```

For more details, see the [wiki page](https://wiki.gentoo.org/wiki/Binary_package_guide).
