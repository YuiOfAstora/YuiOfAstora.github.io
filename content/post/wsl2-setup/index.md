---
title: "WSL2 Setup"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['wsl', 'linux', 'windows']
categories: ['Notes']
comments: true
toc: true
readingTime: true
---

## Update windows 11

## Update GPU Driver

## Fresh Install

Run powershell as Administrator

```powershell
wsl --install -d Ubuntu
```

Get other distros from MS store

## Install apps

```
sudo apt install gedit -y
sudo apt install gimp -y

# File Browser
sudo apt install nautilus -y

sudo apt install x11-apps -y
```

To install the Google Chrome for Linux:

1. Change directories into the temp folder: `cd /tmp`
2. Use wget to download it: `sudo wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb`
3. Get the current stable version: `sudo dpkg -i google-chrome-stable_current_amd64.deb`
4. Fix the package: `sudo apt install --fix-broken -y`
5. Configure the package: `sudo dpkg -i google-chrome-stable_current_amd64.deb`

To launch, enter: `google-chrome`

## Set Proxy

Use [Fiddler Classic](https://www.telerik.com/fiddler/fiddler-classic) winconfig to remove Appcontainer network traffic block

Check IP Address with:

```bash
cat /etc/resolv.conf
```

run proxy software on windows (set 'Allow LAN')

https://help.ubuntu.com/community/AptGet/Howto#Setting_up_apt-get_to_use_a_http-proxy

execute in WSL

```bash
export http_proxy=http://IPaddress:proxyport
```

or use proxychains
## Beautify Windows Terminal

https://youtu.be/VT2L1SXFq9U



## Fix [process exited with code 4294967295]

Network issues (VPN)

run command to reset windows network

```powershell
netsh winsock reset
```

Restart windows

## [Fix Display not open / GUI not working](https://github.com/microsoft/WSL/issues/4106#issuecomment-876470388)

### Setting the DISPLAY variable for WSL2

```bash
export DISPLAY=$(route.exe print | grep 0.0.0.0 | head -1 | awk '{print $4}'):0.0
```

also put in `~/.bashrc`

### Install [vcxsrv](https://sourceforge.net/projects/vcxsrv/) on Windows

### Start XLaunch on Windows

find shortcut, right lick and select properties

Append `-ac` after target path

launch via edited shortcut

- Select Multiple Windows (default)
- Select Start no client (default)
- **Check✅ Disable access control**
- **Uncheck ☐ Native opengl**
- Save configuration on startup folder, location `C:\Users\"YOUR_USERNAME"\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`

All set 

Launch GUI apps on WSL.