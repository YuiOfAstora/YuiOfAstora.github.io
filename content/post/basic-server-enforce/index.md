---
title: "Basic Server Security Enforcing"
date: 2023-01-26T18:46:00+08:00
draft: false
tags: ['linux', 'self host']
categories: ['Guide']
comments: true
toc: true
readingTime: true
---
Enforing server security with disabling root login, adding ssh key authentication, setup 2FA.  

Tested env: Ubuntu 20.04
<!--more-->

https://github.com/midoks/mdserver-web

## Add a new user with super privileges

### Create a new user and add home directory

```
$ useradd username
$ mkdir /home/username
$ mkdir /home/username/.ssh
$ chmod 700 /home/username/.ssh
#Set shell of new user to bash
$ usermod -s /bin/bash username
```

### Use SSH key for authentication

Generate SSH key

Use `$ ssh-keygen` or tools

`ssh-keygen -t rsa -m PEM -b 4096 -C "azureuser@myserver"`

Copy the content of generated ssh public key to server

`$ echo 'public_key_string' >> /home/username/.ssh/authorized_keys`

Set permissions

```
# Set files permissions to owner read only 
$ chmod 400 /home/username/.ssh/authorized_keys
$ chown username:username /home/username -R
```

Try to login with new user 

Set password of new user

`$ passwd username`

Edit sudo

`$ visudo`

```
#Make sure there is sudo group
%sudo ALL=(ALL:ALL) ALL
```

Add new user to sudo group

`$ usermod -aG sudo username`

Logout and relogin to take effects

Set only SSH key authentication 

`$ nano /etc/ssh/sshd_config`

```
# Disable root user login
PermitRootLogin no
# Disable password authentication login
PasswordAuthentication no
AllowUsers username
# Only alow IPV4
AddressFamily inet
```
> ```
> AllowUsers
> This keyword can be followed by a list of user name patterns, separated by spaces.  If specified, login is allowed only for user names that match one of the patterns. Only user names are valid; a numerical user ID is not recognized.  By default, login is allowed for all users.If the pattern takes the form USER@HOST then USER and HOST  are separately checked, restricting logins to particular users from particular hosts.  HOST criteria may additionally contain addresses to match in CIDR address/masklen format.  The allow/deny users directives are processed in the following order: DenyUsers,AllowUsers.
> https://man7.org/linux/man-pages/man5/sshd_config.5.html#:~:text=AllowUsers%20This%20keyword%20can%20be,is%20allowed%20for%20all%20users.
> ```

Restart ssh service
`$ service ssh restart`

## Set 2FA

> https://s.yuiofastora.com/pRxZA

Install Google Pluggable Authentication Module (PAM)

`$ sudo apt install libpam-google-authenticator`

Start the setup

`$ google-authenticator`

Restart the server

Edit PAM config

`$ sudo nano /etc/pam.d/sshd`

Add the following line to the bottom of the file

```
. . .
# Standard Un*x password updating.
@include common-password
auth required pam_google_authenticator.so
auth required pam_permit.so
```

Edit SSH config

`$ nano /etc/ssh/sshd_config`

Set `ChallengeResponseAuthentication` to `yes`

Also add line

`AuthenticationMethods publickey,password publickey,keyboard-interactive`

Edit PAM config again

`$ sudo nano /etc/pam.d/sshd`

Comment out `@include common-auth`

Restart SSHD service

`$ sudo systemctl restart sshd.service`