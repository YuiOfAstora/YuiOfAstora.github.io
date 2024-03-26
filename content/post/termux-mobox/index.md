---
title: "Run x86 windows in Termux"
date: 2022-03-24 17:00:00+0800
image: cover.png
draft: true
description: Mobox is a project designed to run windows x86 applications in Termux using Box64 and Wine.
tags: ['Termux','Linux','Android']
categories: ['Guide','Notes'] 
toc: true #show table of contents
readingTime: true
comments: true
---
## Prerequisites

- Termux App
Termux's latest version is no longer available in Google Play Store, install from [F-droid](https://f-droid.org/) instead.
- Termux X11
Install Termux X11 fo GUI access, get latest nightlt release from [github repo](https://github.com/termux/termux-x11/releases)
- Input Bridge

## Turn off phantom processor killer

## Install mobox

Open termux and run command\
`curl -s -o ~/x https://raw.githubusercontent.com/olegos2/mobox/main/install && . ~/x`

Grant storage access when android system prompts

Choose wow64 version for both 32 and 64 bits support

```
Select an option
1) Install previous mobox with box86
2) Install new mobox wow64 version

Selected number: 2
```

## Setup mobox

Start mobox using command\
`mobox`

Enter settings
- Dynarec settings\
Enter `45` for max performance

- Wine prefix settings(+ESYNC)\
Enable esyn with root(For rooted devices)

- DXVK settings\
`d3d11.maxFeatureLevel` set d3d version\
`dxgi.maxFrameRate` set max framerate\
`dxgi.syncInterval` set vsync\

- System settings\
Change fallback resolution\
Change primary cores amount\
Change locale (`zh_CN` for Chinese simp)
Chaneg HUD preset

- Root settings\
OOM Adjuster (Prevent termux kill)
Disable phantom process killer

- Device compatibility settings
Switch service.exe startup
