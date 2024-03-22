---
title: "Install ZSH"
date: 2023-01-26T18:52:00+08:00
draft: false
tags: ['linux', 'ubuntu']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "Install ZSH and customzie theme"
---
Install ZSH and customzie theme

Tested env: Kubuntu 20.04

<!--more-->

## Install ZSH

`$ sudo apt-get install zsh`

change default shell to zsh

`$ chsh -s $(which zsh)`

log out the current user and log back in to take effect

## Install oh-my-zsh

`$ wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh`

`sh install.sh`

## Install powerlevel10k theme

Install Meslo Nerd Font

Clone file from github and add to zshrc

`$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`.

`$ source .zshrc`

## Plugins for oh-my-zsh

`extract` Uncompress tool

`z` jump to frequently used directories

`colored-man-pages` Colorful man pages

`sudo` Press ESC twice to add sudo for last command



`zsh-syntax-highlighting`

`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

Set `plugins=(zsh-syntax-highlighting)` in `~/.zshrc`



`zsh-autosuggestions`

`git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions`

Set `plugins=(zsh-autosuggestions)` in `~/.zshrc`

