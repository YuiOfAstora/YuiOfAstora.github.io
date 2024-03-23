---
title: "Self Hosting A Status Monitoring Page"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "Self host status page with docker"
---

Self host status page with docker

<!--more-->

## Uptime Kuma

> Self-hosted monitoring tool
>
> https://github.com/louislam/uptime-kuma

### Install With Docker



```
docker run -d \
    --restart=always \
    -p 3001:3001 \
    -v uptime-kuma:/app/data \
    --name uptime-kuma \
    louislam/uptime-kuma:1
```

Setup Reverse Proxy with Nginx

## Statping-ng

> Status Page & Monitoring Server
>
> https://github.com/statping-ng/statping-ng

### Install With Docker

```
docker run -d \
  -p 8080:8080 \
  -v /mydir/statping:/app \
  --restart always \
  --name statping \
  adamboutcher/statping-ng
```

Setup Reverse Proxy with Nginx

## dash.

>https://github.com/MauriceNino/dashdot
>
>**dash.** (or **dashdot**) is a modern server dashboard, running on the latest tech, designed with glassmorphism in mind. It is intended to be used for smaller VPS and private servers.

```
docker container run -d -it \
  -p 80:3001 \
  -v /:/mnt/host:ro \
  --privileged \
  mauricenino/dashdot
```

