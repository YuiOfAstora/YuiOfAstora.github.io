---
title: "Delete apt GPG key"
date: 2023-01-26T18:50:00+08:00
draft: false
tags: ['linux', 'ubuntu']
categories: ['Notes']
comments: false
toc: true
readingTime: true
description: "Delete GPG keys with apt-key"
---
Delete GPG keys with `apt-key`

Tested env: Ubuntu 20.04

<!--more-->

## List all GPG keys

` $ sudo apt-key list `

```
yui@yui-kubuntu:~$ sudo apt-key list
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
/etc/apt/trusted.gpg
--------------------
pub   rsa2048 2016-09-25 [SC]
      4AC4 41BE 68B4 ADAB 7439  FBF9 BA30 0B77 55AF CFAE
uid           [ unknown] Abner Lee <abner@typora.io>

/etc/apt/trusted.gpg.d/danielrichter2007-ubuntu-grub-customizer.gpg
-------------------------------------------------------------------
pub   rsa1024 2010-10-08 [SC]
      59DA D276 B942 642B 1BBD  0EAC A8AA 1FAA 3F05 5C03
uid           [ unknown] Launchpad PPA for Daniel Richter

/etc/apt/trusted.gpg.d/google-chrome.gpg
----------------------------------------
pub   rsa4096 2016-04-12 [SC]
      EB4C 1BFD 4F04 2F6D DDCC  EC91 7721 F63B D38B 4796
uid           [ unknown] Google Inc. (Linux Packages Signing Authority) <linux-packages-keymaster@google.com>
sub   rsa4096 2021-10-26 [S] [expires: 2024-10-25]

/etc/apt/trusted.gpg.d/microsoft.gpg
------------------------------------
pub   rsa2048 2015-10-28 [SC]
      BC52 8686 B50D 79E3 39D3  721C EB3E 94AD BE12 29CF
uid           [ unknown] Microsoft (Release signing) <gpgsecurity@microsoft.com>

/etc/apt/trusted.gpg.d/steam.gpg
--------------------------------
pub   rsa2048 2012-12-07 [SC]
      BA18 16EF 8E75 005F CF5E  27A1 F24A EA9F B054 98B7
uid           [ unknown] Valve Corporation <linux@steampowered.com>
sub   rsa2048 2012-12-07 [E]

/etc/apt/trusted.gpg.d/ubuntu-keyring-2012-cdimage.gpg
------------------------------------------------------
pub   rsa4096 2012-05-11 [SC]
      8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092
uid           [ unknown] Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>

/etc/apt/trusted.gpg.d/ubuntu-keyring-2018-archive.gpg
------------------------------------------------------
pub   rsa4096 2018-09-17 [SC]
      F6EC B376 2474 EDA9 D21B  7022 8719 20D1 991B C93C
uid           [ unknown] Ubuntu Archive Automatic Signing Key (2018) <ftpmaster@ubuntu.com>

```

## Delete entry

Delete entry in /etc/apt/sources.list.d/

## Delete keys

remove typora.io

`$ sudo apt-key del "4AC4 41BE 68B4 ADAB 7439  FBF9 BA30 0B77 55AF CFAE"`

or use the last 8 chars

`$ sudo apt-key del 55AFCFAE`



`$ sudo apt update`