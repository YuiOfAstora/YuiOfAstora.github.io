---
title: "Self Hosting An Email Server With iRedMail"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['docker', 'self host']
categories: ['Guide']
comments: true
toc: true
readingTime: true
description: "Self host email service"
---

Self host email service

<!--more-->

## Requirements

Ports should be open:

https://docs.iredmail.org/network.ports.html

**RAM >= 2 GB**

**2vcpus**

Using Ubuntu 20.04 LTS for this time

## Enable Google’s TCP BBR

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p

#Check whether bbr is enabled
sysctl net.ipv4.tcp_available_congestion_control
#output should be
net.ipv4.tcp_congestion_control = bbr

#Check whether bbr is started
lsmod | grep bbr

# Output should be
tcp_bbr                20480  14
```

## Setting up hostname

```
nano /etc/hosts
#add line
127.0.0.1 mail.mydomain.com mail

nano /etc/hostname
#Change to
mail

sudo reboot
```

Check hostname

```
hostname
#Output shoulde be mail
hostname -f
#Output should be mail.mydomain.com
```

## Install

Download the latest version

```
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.6.2.tar.gz
tar -xf iRedMail.tar.gz
cd iRedMail-1.4.2
bash iRedMail.sh
```

Follow the setting instructions

Select database type to MariaDB

Set mail domain name `mydomain.com`instead of `mail.mydomain.com`

```
***********************************************************************************
* Storgae base directory:
* Mailboxes:
* Daily backup of SQL/LDAP databases:      /var/mail
* Store mail accounts in:				   MariaDB
* Web server:                              Ngnix
* First mail domain name:                  mydomain.com
* Mail domain admin:                       postmaster@mydomain.com
* Additional components:                   Rondcubenail netdata iRedAmin Fail2ban

< Question > Continue? [y|N]
```



## Get SSL cert

Set DNS record

`mail IN A`

| Type | Host | Value           | Priority | TTL        |
| ---- | ---- | --------------- | -------- | ---------- |
| A    | mail | Host IP address | NA       | Auotomatic |

Check DNS recordhttps://www.boce.com/dns/https://dnschecker.org/



Use ACME scripts for SSL cert

```
curl https://get.acme.sh | sh
#Could be personal mail, or just use default xxxx@xxxx.com
~/.acme.sh/acme.sh --register-account -m xxxx@xxxx.com
~/.acme.sh/acme.sh  --issue -d mail.mydomain.com  --webroot  /var/www/html
```

If timeout when CA processes and checks the domain,

switch to letsencrypt

`./acme.sh --set-default-ca --server letsencrypt`

Run

`~/.acme.sh/acme.sh  --issue  -d mail.mydomain.com  --webroot  /var/www/html` again

Install the certification file to specific location

```
#Works on Debian/Ubuntu, FreeBSD and OpenBSD
.acme.sh/acme.sh --installcert -d mail.mydomain.com --key-file /etc/ssl/private/iRedMail.key --fullchain-file /etc/ssl/certs/iRedMail.crt

#If RHEL/CentOS
./acme.sh --installcert -d mail.mydomain.com --key-file /etc/pki/tls/private/iRedMail.key --fullchain-file /etc/pki/tls/certs/iRedMail.crt
```

Reload services

```
service nginx force-reload
service postfix reload
service dovecot reload
```

Remove greylist

edit `/opt/iredapd/settings.py`

delet `greylisting` from

```
plugins = ["reject_null_sender", "wblist_rdns", "reject_sender_login_mismatch", "greylisting", "throttle", "amavisd_wblist", "sql_alias_access_policy"]
```

then

`service iredapd restart`

## Setting Up DNS Records

### Setting PTR

### Setting MX record

`IN MX mail.example.com`

| Type | Host | Value            | Priority | TTL  |
| ---- | ---- | ---------------- | -------- | ---- |
| MX   | @    | mail.example.com | 10       | 3600 |

Get the config from `iRedMail.tips` file(inside the iRedMail Installer directory)

### Setting SPF record

> SPF or Sender Policy Framework is a DNS TXT record that is used to specify which IP addresses and/or servers are allowed to send email “from” that particular domain. It is designed to detect forging sender addresses during the delivery of the email.

add text record

`v=spf1 a mx ip4:0.0.0.0 ~all`

> - v=spf1 – The version of SPF being used, this is the only version available
> - mx – This means all hosts listed in the MX records are allowed to send emails for your domain and all other hosts are disallowed.
> - ip4:0.0.0.0 – This is the IP address of the mail server or domain that is authorized to send email for that domain. Replace the IP in the example with your mail server’s public IP address.
> - ~all – The “all” tag tells the receiving server how it should handle messages sent from a domain that is not listed in the SPF record. The tilde (~) with the ‘a’ is a soft fail, this would mark a server that is not listed in the SPF as spam, you can use -a for a flat out rejection.

### Setting DKIM Records

> DKIM or Domain Keys Identified Mail is an email authentication method that allows mail services to check that an email was indeed sent and authorized by the owner of that domain. In order to achieve this, you give the email a digital signature. This DKIM signature is a header that is added to the message and is secured with encryption.
>
> The difference between DKIM and SPF is that DKIM is used to verify that the contents of the message are trustworthy. This is used to check that the email hasn’t been tampered with since the moment it left the initial mail server.

### Setting DMARC Records

> DMARC or Domain-based Message Authentication Reporting and Conformance extends DKIM and SPF to determine the authenticity of an email message.
>
> Most major email providers perform DMARC check by default before accepting the email. By performing these checks, the email provider knows what authentication methods should be present (SPF, DKIM, etc.) and reject messages that fail the checks as specified by the domain’s DMARC record.
>
> Unlike SPF and DKIM that doesn’t provide reports on sources sending mails, you can get aggregated reports when DMARC is implemented.



`v=DMARC1; p=quarantine; pct=100; rua=mailto:dmarc-reports@mydomain.com  `

The above record states that any email which fails the authentication with the “From” domain is put it in the spam folder 100% of the time and activity is reported to the dmarc-reports@mydomain.com. You can also flat out reject any email that fails the DMARC authentication by setting ‘p=reject’.

```
p=reject    #reject mails
p=none      #do nothing
p=quarantine  #flag as spam
```

### Check Status

Check DNS record

https://dnschecker.org/

Check DNS blacklist

https://www.boce.com/blacklist/

https://www.dnsbl.info/

Check DKIM

https://dmarcian.com/dkim-inspector/
`nslookup -type=txt dkim._domainkey.mydomain.com`

Check DMARC

https://dmarcian.com/dmarc-inspector/

Check SPF

https://dmarcian.com/spf-survey/

## Access EMail

```
https://mail.mydomain.com/mail                 #mailbox entrance
https://mail.mydomain.com/netdata              # Server status monitor
https://mail.mydomain.com/iredadmin            # Admin panel
```

Add new email user

Use third-party email clients

| Protocol | address           | ports & secured ports       |
| -------- | ----------------- | --------------------------- |
| IAMP     | mail.mydomain.com | 143,993(SSl/TLS)            |
| POP3     | mail.mydomain.com | 110,995                     |
| SMTP     | mail.mydomain.com | 25,587(STARLS),465(SSl/TLS) |
```
* POP3 service: port 110 over STARTTLS (recommended), or port 995 with SSL.
* IMAP service: port 143 over STARTTLS (recommended), or port 993 with SSL.
* SMTP service: port 587 over STARTTLS.
  If you need to support old mail clients with SMTP over SSL (port 465),
  please check our tutorial: https://docs.iredmail.org/enable.smtps.html
* CalDAV and CardDAV server addresses: https://<server>/SOGo/dav/<full email address>

For more details, please check detailed documentations:
https://docs.iredmail.org/#mua
```

## Test Spammyness of Emails

https://www.mail-tester.com/

## Fail2ban

Default max tries is 5 times for postfix

Unban ip with fail2ban

## Credits

https://v2rayssr.com/iredmail.html

https://upcloud.com/resources/tutorials/install-secure-private-email-server-modoboa
