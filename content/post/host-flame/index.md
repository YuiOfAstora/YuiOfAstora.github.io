---
title: "Self Hosting Flame"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: false
toc: true
readingTime: true
description: "Self Hosting Flame"
---

> Flame is self-hosted startpage for your server. Its design is inspired (heavily) by [SUI](https://github.com/jeroenpardon/sui). Flame is very easy to setup and use. With built-in editors, it allows you to setup your very own application hub in no time - no file editing necessary.
>
> https://github.com/pawelmalak/flame

<!--more-->
## Setup with Docker

```
docker pull pawelmalak/flame
```

```
# run container
docker run -d -p 5005:5005 -v /path/to/data:/app/data -e PASSWORD=flame_password pawelmalak/flame
```

