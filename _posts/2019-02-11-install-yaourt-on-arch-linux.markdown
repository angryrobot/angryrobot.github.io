---
layout: post
title: 'Install Yaourt on Arch Linux'
date: '2019-02-11 01:41:00'
categories: os
---

*<u>This tutorial assumes that you have a working knowledge of the linux system.</u>*

### About

Yaourt is a command line interface program which complete pacman for installing software on Archlinux.

### Get It

It’s recommended to read about [AUR](https://wiki.archlinux.org/index.php/AUR) before, particularly about the packages needed to use it.

You can install yaourt from AUR:

```bash
$ git clone https://aur.archlinux.org/package-query.git
$ cd package-query
$ makepkg -si
$ cd ..

$ git clone https://aur.archlinux.org/yaourt.git
$ cd yaourt
$ makepkg -si
$ cd ..
```

Examples

Search and install:

```bash
$ yaourt
```

Sync database, upgrade packages, search aur and devel (all packages based on dev version) upgrades:

```bash
$ yaourt -Syu --devel --aur
```

Build package from source:

```bash
$ yaourt -Sb
```

Check, edit, merge or remove *.pac* files:

```bash
$ yaourt -C
```

Get a PKGBUILD (support splitted package):

```bash
$ yaourt -G
```

Build and export package, its sources to a directory:

```bash
$ yaourt -Sb --export
```

Backup database:

```bash
$ yaourt -B
```

Query backup file:

```bash
$ yaourt -Q --backupfile
```

[https://archlinux.fr/yaourt-en](https://archlinux.fr/yaourt-en)

