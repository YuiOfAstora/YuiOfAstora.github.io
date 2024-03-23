---
title: "Self Hosting A Bookmark Manager"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['self host', 'linux', 'docker']
categories: ['Notes']
comments: false
toc: true
readingTime: true
description: "Host linkding using docker"
---
Self host a bookmark manager with docker.

> https://github.com/sissbruecker/linkding

<!--more-->

## Setup with Docker

```
docker run -d \
	--name linkding \
	-p 9090:9090 \
	-v {host-data-folder}:/etc/linkding/data \
	sissbruecker/linkding:latest
```

Setup user

```
docker exec -it linkding python manage.py createsuperuser --username=joe --email=joe@example.com
```

Setup Reverse Proxy with Nginx
Add
```
location /linkding {
    ...
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```
to config