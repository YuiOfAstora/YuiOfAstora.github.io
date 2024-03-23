---
title: "Self Hosting Littlelink Custom"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "LittleLink Custom is a modification of LittleLink, an open source project with the purpose of enabling people to host their own alternatives to services like Linktree and many.link."
---

> LittleLink Custom is a modification of LittleLink, an open source project with the purpose of enabling people to host their own alternatives to services like Linktree and many.link.
>
> https://littlelink-custom.com/docker

<!--more-->

## Setup with Docker

`docker pull julianprieber/littlelink-custom`

Create persistent storage for llc

`docker volume create llc`

```
docker run --detach \
    --name littlelink-custom \
    --hostname littlelink-custom \
    --env HTTP_SERVER_NAME="www.example.xyz" \
    --env HTTPS_SERVER_NAME="www.example.xyz" \
    --env SERVER_ADMIN="admin@example.xyz" \
    --env TZ="Europe/Berlin" \
    --env PHP_MEMORY_LIMIT="512M" \
    --env UPLOAD_MAX_FILESIZE="16M" \
    --publish 80:80 \
    --publish 443:443 \
    --restart unless-stopped \
    --mount source=llc,target=/htdocs \
    julianprieber/littlelink-custom
```

Login to the admin panel with default credentials

- **email:** `admin@admin.com`
- **password:** `12345678`

Access user panel from mydomain.com/home

## Reverse Proxy

Target URL should be https://localhost:port 

Using port 443(HTTPS_SERVER) instead of 80