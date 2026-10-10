---
title:  "Managing Dotfiles with Stow"
date:   2021-10-11
url: 'stow'
---

## Preface

This post walks through ways to manage dotfiles in stages: manually, with Git, with symbolic links, and with GNU stow.

## Dotfiles

On Unix-related systems (Unix, BSD, Linux, etc.), putting a `.` in front of a file name makes it a hidden file that isn't shown by plain `ls`. Taking advantage of this, plain text files containing various program settings have traditionally been named with a leading `.`.

For example, suppose that while building a program called Hello,

you decide that a text file containing settings to override at run time must be named `.hello` (there's no need for a settings file to show up in a basic command like `$ls`, so you prefix it with '.'),

and you decide on `~` (the user's home directory) as the path to check for that file (if the user wants particular settings, the program should run with those rather than the defaults).

Users can then create a file called `~/.hello` and write the settings they like in it.

Since most settings files are dotfiles (files starting with a dot) like this, settings files as a whole came to be called dotfiles.

### The Home Directory

In the early days, there was no **convention** to speak of about where settings files should be stored. So most programs looked for their settings files in the home directory, and users stored them under the home directory too.

For example, vim manages its basic settings through a file called `.vimrc` under the home directory, and bash likewise has `.bashrc` under the home directory.

> **rc?**
>
> The suffix '**rc**', often attached to settings files, is said to come from '**run command**'. This 'run command' reportedly originates from 'runcom' in MIT's CTSS (Compatible Time-Sharing System) in 1965.
>
> However, Eric S. Raymond consistently refers to it as '**run control**' in his book 'The Art of Unix Programming'.
>
> Other minority theories say it stands for "**resource control**" or "**runtime configuration**".

### ~/.config/ ?

Those of you a little familiar with Linux systems may be puzzled at this point, because most programs store their settings files in 'the .config directory under the home directory (`~/.config/`)'.

In particular, inside this directory, each program gets a directory named after it, and all of that program's settings files are managed inside that directory.

For example, if you use the email client mutt or neomutt, you may create the settings file directly under your home directory, but many of you probably create `~/.config/mutt` and store `muttrc` under it.

Or if you use the i3 window manager, you control i3's appearance and behavior with a single `config` file under the `~/.config/i3` directory.

This `.config` directory comes from the [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) established by the XDG foundation. The specification was created to keep the home directory clean, since it was getting cluttered because each program required its settings files in a different location.

So software that faithfully implements the XDG specification must also be able to read its settings files from a directory named after the software under the .config directory.

> **XDG**
>
> The XDG foundation (now: freedesktop.org / formerly: X Desktop Group) was founded in March 2000 by Havoc Pennington, a Gnome developer working at Red Hat.
>
> XDG does a variety of work to make the X Window System and Wayland environments on Linux and other Unix-like systems interoperable.

With this approach, since the files sit under a hidden parent directory, there's no longer any need to hide the settings files themselves with a leading dot. But they're still 'settings files', so they're still called dotfiles, which has stuck as the term for settings files.

### The Result

As a result, the user's home directory ends up looking like this.

```bash
$ tree -a ~
.
├── .bashrc
└── .vimrc
└── .config
    └── mutt
        └── muttrc
    └── i3
        └── config
```

### Porting

If you only ever use a single computer, nothing in this post is useful to you.

The problem arises when you need to bring your personal settings to a new PC or laptop. The simplest way is to put each of the files above on removable storage like a USB drive and paste them onto the new device. (Of course, some people use email;)

Actually, this method has no problem with moving the settings files themselves, apart from being a bit tedious. But another problem appears when you modify or change the settings files. For example, say that when you first created your `.vimrc` file, you didn't know how to use the vim editor well, so it contained only very basic settings. Over time, though, you got used to vim's various features and started using vim plugins along the way. You can't save a new version to a USB drive every time you change a setting.

This problem is very similar to another well-known problem. It's effectively a 'version control problem'. And we have a very effective solution for it.

## Git

Create a Git repository and store your settings files in it, neatly organized.

```bash
$ tree -a ~/Git/dotfiles
.
├── .git
├── .bashrc
└── .vimrc
└── .config
    └── i3
        └── config
    └── mutt
        └── muttrc
```

To make copying easy, the directory structure mirrors where the files need to go. The files at the top level get copied to the home directory, and the files in the `.config` directory get copied under `~/.config`, so the porting problem is neatly solved.

What about version control? Whenever it occurs to you, check how different the settings file you're currently using is from the one in the Git repository with a command like `$diff ~/.bashrc ~/Git/dotfiles/.bashrc`, and apply whatever changed to Git. The version control problem is solved too.

**For a typical user, this setup is honestly all you need.**

However, people who manage a lot of machines, or distro hoppers like me, find even copying and pasting the files tedious every time they set up a new machine.

## Symlinks

Linux has 'hard links' and 'symbolic links'. What we're interested in is the 'symbolic link', or 'symlink' for short. A symlink works very much like a pointer in C. After you create a symbolic link that looks like a file, when a user accesses that symbolic link (thinking it's a file), they're directed to the actual file the link points to.

So the user feels like they accessed the file they asked for, but in reality they followed a signpost standing in its place and modified the file it refers to.

A symlink can be created with the following syntax.

When you want the file `A.rc` in your home directory to actually be a link to the file `B.rc` in your Git directory,

```bash
$ ln -s ~/Git/B.rc ~/A.rc
```

Using this, you can link the settings files you've put in Git to the locations where they're supposed to be.

But there's still one unsolved problem. Once the number of programs using settings files grows past 10 or 20, when will you ever create and manage links for all those files?

When the tedium reaches this level, we go looking for another solution. We need to be able to create symbolic links automatically.

>
> The order of the files given as arguments is always confusing. A very easy-to-remember hint is that **Linux commands are often written in the order 'what already exists', 'what doesn't exist yet'**.
>
> Examples →`$mv existing_file not_existing_yet`,`$cp existing_file not_existing_yet`,`$ln existing_file not_existing_yet`

There are two alternatives.

1. Git Bare Repository
2. GNU Stow Software

Take your time looking at both and pick the one you like better.

## Git Bare Repository

`git init` turns the current directory into a Git-managed directory and, at the same time, creates a directory called `.git`. This `.git` is the brain of the `git` command, holding all the commit information and file hashes. So in ordinary situations, you use `git init` to make the current directory your actual working environment.

Managing settings files (dotfiles), which is what we're doing here, doesn't actually need that whole structure. All that matters is which files to track (keep watching and manage changes to). In other words, no work, such as adding new features, needs to happen under this directory.

And Git already has a feature for managing this kind of directory: the **bare repository**.

A bare repository is normally used to manage many people's work in a remote repository. So it only tracks added and deleted files; no work is done inside the repository itself.

Managing dotfiles with a bare repository looks like this.

```bash
$ git init --bare $HOME/.dotfiles
$ echo "alias config='/usr/bin/git \
        --git-dir=$HOME/.dotfiles  \
        --work-tree=$HOME'" >> $HOME/.bashrc
$ source $HOME/.bashrc
If you use a different shell, add it to your shell's config file instead of .bashrc.

$ config config --local status.showUntrackedFiles no

For example, to manage .vimrc:
$ config add .vimrc
$ config commit -m 'added .vimrc'

$ config push
Naturally, do this after setting up a remote repository (github, gitlab, etc.).
```

In other words, you set up the `config` command to replace the `git` command you'd normally type, and then manage the Git repository through the `config` command.

Since it's a bare repository, you configure it so that the Git directory itself lives here, while the actual working environment is over there. (`alias config`)

To install the settings files you're managing this way on an actual new computer, run the following commands.

```bash
$ echo ".dotfiles" >> .gitignore
$ git clone --bare <REPOSITORY-URL> $HOME/.dotfiles
$ echo "alias config='/usr/bin/git \
            --git-dir=$HOME/.dotfiles/ \
            --work-tree=$HOME'" >> $HOME/.bashrc
$ source $HOME/.bashrc
If you use a different shell, add it to your shell's config file instead of .bashrc.

$ config config --local status.showUntrackedFiles no
$ config checkout
```

As you may have already noticed, this approach makes heavy use of `git checkout`. A new PC environment is no different from a new branch, so you check out the remote repository into `$HOME`, which you've designated as this PC's working environment.

## Stow

### Basic Concept

stow was originally [a program made as a symbolic link manager](https://www.gnu.org/software/stow/). To borrow the example from the GNU stow project, it lets you create symbolic links under `/usr/local/bin` that point to `/usr/local/stow/emacs/bin` and `/usr/local/stow/perl/bin`.

**stow's basic behavior is to look at the structure and files under the current directory, then go up one directory and set up symbolic links mirroring them.**

### Additional Concepts

There are a few more things you need to know.

1. Directory folding

stow uses a concept called **directory folding**. Suppose the directory structure you want to apply exists as `/usr/lib/test/A/A.rc`. If the `/usr/lib/` directory doesn't yet contain any files or directories other than the `test` directory, then moving to `/usr/lib/test/` and running `$stow` creates `/usr/lib/A` itself as a symbolic link pointing to `/usr/lib/test/A`.

It compares the parent directory with the given directory structure from the top down, and as soon as it hits the first point where they differ, it simply creates a symbolic link there and stops. In other words, it does not **copy** the directory structure all the way down and create symbolic links only for the files at the bottom.

Here's an example that shows this.

```bash
$ tree -a .
.
└── test
    └── A
        └── .config
            └── A.rc
In this situation,

$ cd test
then,

$ stow A
run this, and

$ cd ..
when you come back out and check, it has been applied like this.

$ tree -a .
.
├── .config -> test/A/.config
└── test
    └── A
        └── .config
            └── A.rc
```

As you can see above, rather than `.config/A.rc -> test/A/.config/A.rc`, it linked `.config` itself.

2. The package concept

The package concept isn't difficult. As you've already seen above, you can target a specific directory inside the current directory, as in `$stow A`. When stow was originally developed, its purpose was to make it easy to install various perl packages containing libraries, commands, and manual pages. That is, under a directory called A there were directories like lib, bin, and man, each containing actual files. In this case, A, the top directory containing the whole structure, can be treated as a package.

According to the definition in the stow manual, "a package is a collection of files and directories that you wish to manage as a unit".

That's why, in the example above, you can run it as `$stow A`.

### Options

**But so far this is practically no different from symbolic links. stow's strength lies in its options.** The most representative stow options are below.

> `-S DIR`, (`--stow=DIR`, specifies the source directory/package)

Running stow at all means you're trying to put something in, so **this option flag itself can be omitted**. In other words, simply listing source directories/packages is understood the same as having `-S` turned on. However, you must always specify the source directory/package you're trying to put in, even if it's `.` or `*`. When combining it with `-D`, `-R`, and so on, you must turn it on explicitly.

> `-t DIR`(`--target=DIR`)

If the option isn't given, the target directory defaults to the directory one level up.

When the source directory is your current location and the target directory is your home directory, you can use these options as `$stow *` or `$stow -t ~ *`. When the `-t` option is used, the first path is the target directory, and the path(s) after it are source directories.

**Even if you only need to link a single settings file, there's no problem if you create a directory named after the program that needs that file and store the file under it.**

Other very useful options are below.

- Use the `-v`(`--verbose=N`) option to print detailed results, so you can double-check what changed from the output.
- You can also add the `-n`(`--no`, `--simulate`) option to do a dry run first, then actually run the command without the `-n` option.

> **The difference between `$stow .` and `$stow *`**
>
> `.` treats the current directory itself as the package. That is, `A/A.rc` inside the current directory is linked into the parent directory as `A -> ./test/A`, and `B.rc` as `B.rc -> ./test/B.rc`.
>
> `*` treats the directories inside the current directory as packages. So if you run it in a directory that also contains files, it fails to recognize those as packages and spits out an error like this: `stow: ERROR: The stow directory stow does not contain package B.rc`
>
> In the end, you check that `$stow -nvt ~ *` gives the result you expect, then run `$stow -vt ~ *`.

Furthermore, when you only want to set up certain directories, `$stow -vt ~ this_dir that_dir also_dir` ignores the remaining directories and creates symbolic links only for the settings files under this_dir, that_dir, and also_dir.

In other words, you can clone the Git repository anywhere you like and apply/manage your settings files from there.

### Examples

Below are the results of running various commands against the following `test` directory.

```bash
$ tree -a .
.
└── test
    ├── A
    │   └── .config
    │       └── A.rc
    ├── B
    │   └── .config
    │       └── B.rc
    └── C
        └── C.rc
```

For each result,

- I moved into the `test` directory,
- ran the command in the item's heading,
- and copied the result.
- Then, before the next example, I deleted all the created links.

I recommend looking at each command and guessing the result before reading it.

### **`$ stow C`**

```bash
$ tree -a .
.
├── C.rc -> test/C/C.rc
└── test
    ├── A
    │   └── .config
    │       └── A.rc
    ├── B
    │   └── .config
    │       └── B.rc
    └── C
        └── C.rc
```

### **`$ stow B`**

```bash
$ tree -a .
.
├── .config -> test/B/.config
└── test
    ├── A
    │   └── .config
    │       └── A.rc
    ├── B
    │   └── .config
    │       └── B.rc
    └── C
        └── C.rc
```

>
> You can see that because the parent directory didn't have a `.config`, it linked `.config` itself rather than `B.rc`.

### **`$ stow A`**

```bash
$ tree -a .
.
├── .config -> test/A/.config
└── test
    ├── A
    │   └── .config
    │       └── A.rc
    ├── B
    │   └── .config
    │       └── B.rc
    └── C
        └── C.rc
```

>
> The same happens when running just `A`.

### **`$ stow -nv A`**

```bash
$ stow -nv A
LINK: .config => test/A/.config
WARNING: in simulation mode so not modifying filesystem.
```

>
> Preview the result with the `n` and `v` options

### **`$ stow -nvS A`**

```bash
$ stow -nvS A
LINK: .config => test/A/.config
WARNING: in simulation mode so not modifying filesystem.
```

>
> Turning on `S` changes nothing, since it was already implicitly on

### **`$ stow -nvt A B`**

```bash
$ stow -nvt A B
LINK: .config/B.rc => ../../B/.config/B.rc
WARNING: in simulation mode so not modifying filesystem.
```

>
> With the `t` option, you can freely set the target directory instead of always the directory right above. Here the destination is set to `A`.

>
> The arguments are applied in the order `target_directory source_directory`. Thanks to this order, you can list several source directories to run.
>
> ex) `$stow -vt ~ Dir1 Dir2 Dir3`

>
> Since the target directory was set to `A`, the `.config` shown here is `A/.config`! That's why you get the result in the example below.

### **`$ stow -vt A B`**

```bash
$ tree -a .
.
└── test
    ├── A
    │   └── .config
    │       ├── A.rc
    │       └── B.rc -> ../../B/.config/B.rc
    ├── B
    │   └── .config
    │       └── B.rc
    └── C
        └── C.rc
```

>
> You can actually run it by removing the `n` option.

>
> As mentioned above, since the target was set to `A`, you can see that it went inside the `A` directory, found that a `.config` directory already existed, and created only a new link for `B.rc`.

### **`$ stow -nvt .. *`**

```bash
$ stow -nvt .. *
LINK: .config => test/A/.config
UNLINK: .config (reverts previous action)
MKDIR: .config
LINK: .config/A.rc => ../test/A/.config/A.rc
LINK: .config/B.rc => ../test/B/.config/B.rc
LINK: C.rc => test/C/C.rc
WARNING: in simulation mode so not modifying filesystem.
```

>
> Look closely at the output: on the first line, while processing package A, stow notices the parent directory has no `.config` at all and lazily links `.config` itself; then, while processing package B, it realizes it needs that directory again, panics, and runs `UNLINK`. After that, it creates the `.config` directory and links each `rc` file.

### **`$ stow -vt .. *`**

```bash
$ tree -a .
.
├── .config
│   ├── A.rc -> ../test/A/.config/A.rc
│   └── B.rc -> ../test/B/.config/B.rc
├── C.rc -> test/C/C.rc
└── test
    ├── A
    │   └── .config
    │       └── A.rc
    ├── B
    │   └── .config
    │       └── B.rc
    └── C
        └── C.rc
```

>
> This command ends up being the same as running `$stow *` inside the `test` directory.

### Summary

To sum up, if you organize things under Git like this:

```bash
$tree -a ~/Git/dotfiles
.
└── Stow
    ├── Bash
    │   └── dot-bashrc
    ├── i3
    │   └── .config
    │       ├── i3
    │       │   └── config
    └── Neomutt
        └── .config
            └── neomutt
                └── neomuttrc
```

then move into that Git directory with `$cd ~/Git/dotfiles` and run `$stow -vt ~ *`,

```bash
$ tree -a ~
.
├── .bashrc -> ~/Git/dotfiles/Stow/Bash/.bashrc
└── .config
    └── i3 -> ~/Git/dotfiles/Stow/i3/.config/i3
    └── neomutt -> ~/Git/dotfiles/Stow/Neomutt/.config/neomutt/neomuttrc
```

and it's applied like this.

In the end, by using stow to create symbolic links pointing to the settings files stored in Git, simply changing the settings files in Git also changes the settings applied on your system, and version control becomes easy (since you're editing the files that are already in Git).

>
> If you didn't have a dotfiles repository yet and are only now trying stow, take a look at the `--adopt` option. (`$stow --adopt -nvt ~ *`)
>
> stow might help you move the files into the current directory without copying/moving them by hand!

If you want to remove symbolic links you've already created, or remove them for any other reason, use the `$stow -D` option! As when creating them, make good use of the `-n` option. The command you'll probably end up using is `$stow -D ${TARGET_PACKAGES}` after moving into the Git directory with your settings files! It works out which files it would have applied, then goes around finding them and removing the links.

## Others' Repository

[Browse other people's dotfiles repositories](https://dotfiles.github.io/)

> **NixOS**
>
> If you don't stop there and keep looking for an even more hardcore approach, you'll run into the Nix package manager or NixOS itself.
>
> Nix goes beyond settings files: it lets you store system-related configuration, such as the programs used across your whole system and the programs your user uses, in `*.nix` files, then load those `*.nix` files and apply them as-is when setting up a new PC.
