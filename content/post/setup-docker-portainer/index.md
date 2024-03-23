---
title: "Setup Docker and Portainer"
date: 2023-02-20T19:00:00+08:00
draft: false
tags: ['self host', 'docker']
categories: ['Guide']
comments: true
toc: true
readingTime: true
description: "Setup docker and portainer"
---
Setup docker and portainer
<!--more-->

## Installing Docker

```
#Install a few prerequisite packages which let apt use packages over HTTPS:
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common

#Add the GPG key for the official Docker repository
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

#Add the Docker repository to APT sources
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

$ sudo apt install docker-ce
$ sudo systemctl status docker
```

## Install Docker Compose

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
#replace 1.29.2 with latest version from https://github.com/docker/compose/releases

$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version
```

## Install Portainer

```
$ cd ~/

$ docker volume create portainer_data

$ docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

```

`$ iptables -L DOCKER`

Check Docker port

By default, docker will create iptables rules which would bypass firewall settings

To fix this,

https://github.com/chaifeng/ufw-docker

> Add following line to /etc/ufw/after.rules,
>
> before line `COMMIT`
>
> ```
> # BEGIN UFW AND DOCKER
> *filter
> :ufw-user-forward - [0:0]
> :ufw-docker-logging-deny - [0:0]
> :DOCKER-USER - [0:0]
> -A DOCKER-USER -j ufw-user-forward
> 
> -A DOCKER-USER -j RETURN -s 10.0.0.0/8
> -A DOCKER-USER -j RETURN -s 172.16.0.0/12
> -A DOCKER-USER -j RETURN -s 192.168.0.0/16
> 
> -A DOCKER-USER -p udp -m udp --sport 53 --dport 1024:65535 -j RETURN
> 
> -A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 192.168.0.0/16
> -A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 10.0.0.0/8
> -A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 172.16.0.0/12
> -A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 192.168.0.0/16
> -A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 10.0.0.0/8
> -A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 172.16.0.0/12
> 
> -A DOCKER-USER -j RETURN
> 
> -A ufw-docker-logging-deny -m limit --limit 3/min --limit-burst 10 -j LOG --log-prefix "[UFW DOCKER BLOCK] "
> -A ufw-docker-logging-deny -j DROP
> 
> COMMIT
> # END UFW AND DOCKER
> ```

