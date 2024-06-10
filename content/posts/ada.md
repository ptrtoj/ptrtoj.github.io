---
title:  "Ada on MacOS"
date:   2024-06-10
url: 'ada'
---

[Work in Progress, but the important tips are already included!]

## Bulid Toolchain

You'll need basic build tools such as `gprbuild`, `gprmake`, and those are distributed with GCC.

On Apple silicon you should use [simonjwright/distributing-gcc](https://github.com/simonjwright/distributing-gcc).

Download release with the name `aarch64`.

Follow the wiki. Bug basically you'll do

- Right Click the downloaded file to install
- Add the path `/opt/gcc-VERSION-aarch64/bin` to `$PATH`

Then, you can check the installation with `$ which gprbuild`. Output should show the location of your `gcc`(default: `/opt/gcc-VERSION-aarch64/bin`).

## Done (Test)

You can type below under the filename of 'hello.adb'

```ada
with Ada.Text_IO;
use Ada.Text_IO;

procedure Hello is
begin
   Put_Line ("Hello WORLD!");
end Hello;
```

And now you can

```bash
$ gcc -c hello.adb
$ gnatbind hello
$ gnatlink hello
$ ./hello
> Hello WORLD!
```

OR, simply

```bash
$ gnatmake hello.adb
$ ./hello
> Hello WORLD!
```

## Alire (If you want)

You can clone [alire-project/alire](https://github.com/alire-project/alire).

(You may want to switch to the release branch via `$ git switch -c remotes/origin/release/2.0`)

And then, on MacOS you don't have `bash`, but have `zsh`, so run `$ ALIRE_OS=macos gprbuild -j0 -p -P alr_env`.

Lastly, your `alr` binary is generated under the git repository's `bin` directory. So you have to edit the path.

(In the future, when the new release come out, you can simply switch to the release branch and run `alr biuld`)

**MOST IMPORTANTLY**, You should run `$ alr toolchain --select` and select gnat version `gnat_external=VERSION [Detected at /opt/gcc-VERSION-aarch64/bin/gnat` one, which we installed above. Also, for the gprbuild, `gprbuild=VERSION [Detected at /opt/gcc-14.1.0-aarch64/bin/gprbuild]`.

## Done (Test)

Move to the desired dev directory, and run

```bash
$ alr get hello
$ cd hello[tab]
$ alr run
> Hello, World!
```