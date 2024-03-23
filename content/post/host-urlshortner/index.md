---
title: "Self Hosting A URL Shortener"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "Self host URL shortener with docker"
---

Self host URL shortener with docker

<!--more-->

> A PHP-based self-hosted URL shortener that can be used to serve shortened URLs under your own domain.
>
> https://github.com/shlinkio/shlink

## Setup with Docker

`docker pull shlinkio/shlink:stable`

```
docker run -d\
    --name my_shlink \
    -p 8080:8080 \
    -e DEFAULT_DOMAIN=doma.in \
    -e IS_HTTPS_ENABLED=true \
    -e GEOLITE_LICENSE_KEY=kjh23ljkbndskj345 \
    shlinkio/shlink:stable

If you don't provide a GeoLite2 license key, Shlink will simply not geolocate visits.
```

##  Generate a GeoLite2 license key

- Create a MaxMind [account for GeoLite2](https://www.maxmind.com/en/geolite2/signup).

- Generate a [license key](https://www.maxmind.com/en/accounts/current/license-key). Its **important** that you save the value, as you will not be able to recover it afterwards.

Via environment variable: use `GEOLITE_LICENSE_KEY` to set your license key.

generate a new API

`docker exec -it my_shlink shlink api-key:generate`

## Setup shlink client

`docker pull shlinkio/shlink-web-client`

```
docker run -d\
  --name shlink-web-client \
  -p 8000:80 \
  -e SHLINK_SERVER_URL=https://doma.in \
  -e SHLINK_SERVER_API_KEY=6aeb82c6-e275-4538-a747-31f9abfba63c \
  shlinkio/shlink-web-client
```

