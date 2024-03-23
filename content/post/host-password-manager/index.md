---
title: "Self Hosting A Password Manager"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "Host bitwarden"
---
Self host password manager with docker.

> https://github.com/dani-garcia/vaultwarden

<!--more-->
## Setup With Docker

Generate admin token

`openssl rand -base64 48`

```
docker run -d \
	--name bitwarden \
	-p 9090:80 \
	-v {host-data-folder}:/data \
 	-e ADMIN_TOKEN=Token_here \
 	vaultwarden/server
```
Accessing admin panel from `"yourdomain.com/admin"`
Setup Reverse Proxy with Nginx