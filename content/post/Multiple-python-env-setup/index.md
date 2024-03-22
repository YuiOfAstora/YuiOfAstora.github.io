---
title: "Multiple Python Env Setups"
date: 2023-01-26T18:58:00+08:00
draft: false
tags: ['python']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "Install and manage mutliple python environments using pyenv"
---
Install and manage mutliple python environments using pyenv
<!--more-->

## Install pyenv

https://k0nze.dev/posts/install-pyenv-venv-vscode/

## Use pyenv

List all installable python versions

`pyenv install -l`

Install specified versions of python

`pyenv install x.x.x`

List current installed python versions

`pyenv versions`

Set specified versions for  current shell session

`pyenv shell x.x.x`

Set python version for global usage

`pyenv global x.x.x`

Set python version for current path

`pyenv local x.x.x`

Create virtual environment in .venv and activate it

`python -m venv .venv`

`.\.venv\Scripts\activate`

`get-command pip`

Unset global and use only system Python
`pyenv global --unset`

