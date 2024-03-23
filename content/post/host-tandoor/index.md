---
title: "Self Hosting A Recipe Manager-Tandoor Recipes"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Notes']
comments: true
toc: true
readingTime: true
description: "Host tandoor"
---

Self host recipe manager with docker

<!--more-->

>  Django application to manage, tag and search recipes using either
>  built-in models or external storage providers hosting PDFs, Images or
>  other files.
>  https://github.com/TandoorRecipes/recipes
>  https://docs.tandoor.dev

## Install And Setup Postgresql As Database Source

Create database and user for Tandoor(Alter Superuser needed).

`pg_trgm` and `unaccent` extension required, install `postgresql-contrib`.

Modify config file to allow specific IP accessing the database.

Edit `postgresql.conf` :

```
listen_addresses = '*'
```

Edit `pg_hba.conf`

example config:

```
#TYPE   DATABASE        USER            ADDRESS                 METHOD
host    all             all             0.0.0.0/0               trust  #allow all access from any ip even without correct password
host    all             all             192.168.1.1/32          md5    #allow access from 192.168.1.1 using password with md5 encoding
host    all             all             192.168.1.1/32          md5    #allow access from 192.168.1.1 using password with plain text
```

## Setting Up With Docker

```
docker run -d \
    -v "$(pwd)"/staticfiles:/opt/recipes/staticfiles \
    -v "$(pwd)"/mediafiles:/opt/recipes/mediafiles \
    -p 8435:8080 \
    -e SECRET_KEY=YUOR_BASE64_SECRET_KEY \
    -e DB_ENGINE=django.db.backends.postgresql \
    -e POSTGRES_HOST=YOUR_DATABASE_HOST \
    -e POSTGRES_PORT=5432 \
    -e POSTGRES_USER=DB_USER \
    -e POSTGRES_PASSWORD=DB_PASSWORD \
    -e POSTGRES_DB=DB_NAME \
    -e TIMEZONE=Asia/Shanghai \
    --name tandoor \
    vabene1111/recipes
```

Running with port 8435 exposed.

Check time zones: https://timezonedb.com/time-zones

## Setting Reverse Proxy with Nginx



```
location / {
	proxy_set_header X-Forwarded-Proto $scheme;     		        #required, otherwise there will be CSRF error
    proxy_set_header Host $http_host;                   			     #try $host instead if this doesn't work
	proxy_pass http://127.0.0.1:8080;
	proxy_redirect http://127.0.0.1:8080 https://recipes.domain.com;	#replace port and domain
}
```
