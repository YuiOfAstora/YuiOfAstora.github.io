---
title: "Oneplus 7Pro unlock"
date: 2023-01-28T19:00:00+08:00
draft: true
tags: ['Oneplus','Phone']
categories: ['Guide']
comments: true
toc: true
readingTime: true
description: "Unlock Oneplus 7pro and get root access"
---

Unlock the bootloader for Oneplus 7Pro and get Root access with magisk. 

Using MsmDownloadTool to unbrick the device.

<!--more-->

## Unlock the bootloader

**Backup the files first**. Unlock the bootloader would cause a `factory reset` which wipes out the entire device storage. 

1. Install drivers

    [google usb driver](https://files.yuiofastora.com/s/T8Pfwx2YaDTwpzn)

    Oneplus usb driver

    MSMtool driver

    Android SDK Platform Tools

    Install drivers on PC(Windows).

2. Enable OEM unlocking

    `Enabling Developer options`:

    Go to `Settings`>`About phone`, and tap `Build number` for few times quickly.

    Go to `Settngs`>`System`>`Developer options`, Enable `OEM unlocking`

3. Reboot into bootloader

​		Power off the phone, hold `volume +`,` volume - `and `power` button at the same time, waiting boot into bootloader.

​		Connect the phone to PC. Open `device manager`, there should be an `Android Phone` device with `Oneplus Android Bootloader Interface` connected. 

> If there is a yellow warning sign and windows could not 

## Switching to Oxygen OS from Hydrogen OS

