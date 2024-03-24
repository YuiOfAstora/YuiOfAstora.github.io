---
title: "Using Termux to Deploy Linux Desktop on rooted Android device"
date: 2022-03-24 14:00:00+0800
image: cover.png
draft: true
description: Using powerful toolkit to run linux desktop on Android
tags: ['Android','Ubuntu','Linux']
categories:
    - Guide
toc: true #show table of contents
readingTime: true
comments: false
---
## Prerequisites

- A rooted Android devices
We're using chroot which is faster than proot. You can also install a linux distribution without root access which is not discussed here
- Termux App
Termux's latest version is no longer available in Google Play Store, install from [F-droid](https://f-droid.org/) instead.
- Termux X11
Install Termux X11 fo GUI access, get latest nightlt release from [github repo](https://github.com/termux/termux-x11/releases)

## First time setup

Open Termux and update packages\
`pkg update && pkg upgrade`

Install tsu and pulseaudio\
`pkg install tsu pulseaudio`
