---
title: "Self Hosting Gokapi"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: false
toc: true
readingTime: true
description: "Gokapi is a lightweight server to share files, which expire after a set amount of downloads or days."
---

> Gokapi is a lightweight server to share files, which expire after a set amount of downloads or days.
>
> https://github.com/Forceu/gokapi

<!--more-->
## Setup with docker

`docker pull f0rc3/gokapi:latest`

```
docker run -d --name gokapi -v {host-gokapi-data}:/app/data -v {host-gokapi-config}:/app/config -p 53842:53842 f0rc3/gokapi:latest
```

Open http://localhost:53842/setup for initial setup

> Setup variables
>
> https://gokapi.readthedocs.io/en/latest/setup.html

yourdomain.com/admin for admin panel

 
