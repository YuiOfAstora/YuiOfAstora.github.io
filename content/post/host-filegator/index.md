---
title: "Self Hosting Filegator "
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: false
toc: true
readingTime: true
description: "FileGator is a free, open-source, self-hosted web application for managing files and folders."
---

> FileGator is a free, open-source, self-hosted web application for managing files and folders.
>
> https://filegator.io/

<!--more-->

## Setup with Docker

`docker pull filegator/filegator`

```
docker run -p 8000:80 -d filegator/filegator
visit: http://127.0.0.1:8000 login as admin/admin123
```

