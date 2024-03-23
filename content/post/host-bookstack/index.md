---
title: "Self Hosting Bookstack"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: false
toc: true
readingTime: true
---

Self host bookstack

<!--more-->

> BookStack is a simple, self-hosted, easy-to-use platform for organising and storing information.
>
> https://www.bookstackapp.com/
>
> https://github.com/linuxserver/docker-bookstack

## Setup with Docker

```
---
version: "2"
services:
  bookstack:
    image: lscr.io/linuxserver/bookstack
    container_name: bookstack
    environment:
      - PUID=1000
      - PGID=1000
      - APP_URL=
      - DB_HOST=bookstack_db
      - DB_PORT=3306
      - DB_USER=bookstack
      - DB_PASS=<yourdbpass>
      - DB_DATABASE=bookstackapp
    volumes:
      - /path/to/data:/config
    ports:
      - 6875:80
    restart: unless-stopped
    depends_on:
      - bookstack_db
  bookstack_db:
    image: lscr.io/linuxserver/mariadb
    container_name: bookstack_db
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=<yourdbpass>
      - TZ=Europe/London
      - MYSQL_DATABASE=bookstackapp
      - MYSQL_USER=bookstack
      - MYSQL_PASSWORD=<yourdbpass>
    volumes:
      - /path/to/data:/config
    restart: unless-stopped
```



The default username is [admin@admin.com](mailto:admin@admin.com) with the password of **password**, access the container at [http://dockerhost:6875](http://dockerhost:6875/).
