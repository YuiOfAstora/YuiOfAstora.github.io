---
title: "Linux Software list"
date: 2023-01-26T18:56:47+08:00
draft: true
tags: ['linux']
categories: ['Notes']
comments: false
toc: true
readingTime: false
---
Personal list

Mint

<!--more-->

## Fonts

[Jetbrains Mono](https://www.jetbrains.com/zh-cn/lp/mono/)  English font for developers

[Fira Code](https://github.com/tonsky/FiraCode/wiki/Linux-instructions#installing-with-a-package-manager)       Front-end developers prefered font

[cascafia code](https://github.com/microsoft/cascadia-code)   Default font of Windows terminal

Meslo Nerd Font  Recommended font of powerlevel10k

## Input

google-pinyin fcitx (cloud pinyin addon)

Enable fcitx in `System Settings>Input Method`

![fcitx](https://pinry.yuiofastora.com/media/b/7/b763170138900527087a1b360a9d74b8/fcitx.png)

![pinyin](https://pinry.yuiofastora.com/media/a/1/a18a7308cd4110b61193f995dc348cf6/pinyin.png)

Add input method and configure clouf pinyin addon

`$ sudo apt install fcitx-module-cloudpinyin`

Restart fcitx to take effects





Markdown Editor

- Typora https://typora.io/

Git client

- Github Desktop https://github.com/shiftkey/desktop/releases 

Password manager

- Bitwarden https://bitwarden.com/

Screenshot tool

- Flameshot `$ sudo apt install flameshot`  To launch via command `$ flameshot gui`

Proxy Tools

- CFW https://github.com/Fndroid/clash_for_windows_pkg/releases

- proxychains4 `sudo apt install proxychains4`

  Configure `/etc/proxychians4.conf`

  ```
  socks5 127.0.0.1 $Port
  http 127.0.0.1 $Port
  ```

AppImage Manager

- AppImageLauncher https://github.com/TheAssassin/AppImageLauncher

Email client 

- Thunderbird https://www.thunderbird.net/en-US/

Grub customize

```
$ sudo add-apt-repository ppa:trebelnik-stefina/grub-customizer
$ sudo apt update
$ sudo apt install grub-customizer
```

Grub Themes https://www.gnome-look.org/browse?cat=109

Linux QQ 

- New NT https://im.qq.com/linuxqq/index.shtml



Obsidian

Visual stuidio code

Blender 

Chrome

latte dock 

mcOS BS Layout for Latte Dock https://store.kde.org/p/1399346

WPS office

Spotify

LinuxQQ

zeal
