---
title: "Self Hosting Pinry"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "The open-source core of Pinry, a tiling image board system for people who want to save, tag, and share images, videos and webpages in an easy to skim through format."
---

> The open-source core of Pinry, a tiling image board system for people who want to save, tag, and share images, videos and webpages in an easy to skim through format.
>
> https://github.com/pinry/pinry

<!--more-->

```
docker pull getpinry/pinry
```

```
# this should be an abs-path not relative path like "."
export DATA_PATH=/abs/path/to/your/data/directory

sudo docker run -d=true -p=80:80 \
    -v=${DATA_PATH}:/data \
    getpinry/pinry
```

Create superuser

```
docker exec -it clever_hermann python manage.py createsuperuser --settings=pinry.settings.docker
```

