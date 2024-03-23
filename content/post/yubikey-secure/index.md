---
title: "Using Yubikey to secure credentials"
date: 2023-03-02T14:46:47+08:00
draft: false
tags: ['intersting stuff']
categories: ['Guide']
comments: true
toc: true
readingTime: true
description: "Using a hardware security key to protect account credentials"
---

Using a hardware security key to protect account credentials
<!--more-->

[Get a Yubikey](https://www.yubico.com/why-yubico/) 

![yubikeys](https://pinry.yuiofastora.com/media/2/5/25d2a6cdb507a55baf1a6b31bebfb77c/yubikeys.png)

## Setup a Yubikey 5 NFC

Insert Yubikey into PC(Windows 10, 11), go to `System Settings>Accounts>Sign-in options>Security key`,

reset the key first then create a new pin.

![resetPIN](https://pinry.yuiofastora.com/media/4/a/4ac68a458ba51dd1b9cac0f8032a9b86/resetPIN.png)

Using [Yubikey manager](https://www.yubico.com/support/download/yubikey-manager/) to configure FIDO2, OTP and PIV functionality. (Run as administrator to config FIDO2)

![yubikey manager](https://pinry.yuiofastora.com/media/8/b/8b7304517aa02de38faa69515b01267c/yubikeymanager.png)

## Using yubikey for 2-Step Verification

### Google:

Go to https://myaccount.google.com/security,

add a security key under 2 step verification

![google](https://pinry.yuiofastora.com/media/b/6/b63c53ffcaa2279cf6075621042c9238/google.png)

### Github:

![github](https://pinry.yuiofastora.com/media/0/f/0f0c3f9472f48848aaf18bb84b6729a7/github.png)

### Twitter:

![twitter](https://pinry.yuiofastora.com/media/e/7/e7a83ed51c3f949c788bfaf8460f0f88/twitter.png)

## Securing 2FA OTP

Using [Yubikey authenticator](https://www.yubico.com/products/yubico-authenticator/) to secure 2FA OTP credentials.

![YubikeyAuthenticator](https://pinry.yuiofastora.com/media/0/0/00ffabbbd4f0c08006c8c8dc4aa4b58c/yubikeyauthenticator.png)

## Securing Windows Login

### Passwordless Login 

#### Login with Azure AD

Requirements:

1. Azure Active Directory web applications
2. Azure Active Directory joined Windows 10 devices (Windows 10 1909 and later)
3. Hybrid Azure Active Directory joined Windows 10 devices (Windows 10 2004 and later)

[Compatible devices](https://support.yubico.com/hc/en-us/articles/360016913619-YubiKeys-for-Microsoft-Azure-AD-Passwordless-Sign-In-Guide)

Guides: https://alven.tech/passwordless-with-windows-10-and-yubikey/

#### Using Yubikey for windows Hello(yubikey 5 series incompatible)

Get the UWP app(No longer available)

https://apps.microsoft.com/store/detail/yubikey-for-windows-hello/9NBLGGH511M5

Get the offline package from [limbenjamin](https://limbenjamin.com/articles/yubikey-passwordless-windows-local-account-login.html)

### Local account securing

> If your user account is local and not managed by Azure Active Directory (AAD) or Active Directory (AD), you can add a layer of protection beyond passwords with the YubiKey.
>
> Before installing the Yubico Login for Windows software, please make a note of your Windows username and password. If you do not have your UN/PW, you will not be able to log in once Yubico Login for Windows has been installed
>
> - Once Yubico Login for Windows has been configured, there is:
>   - No Windows Password Hint
>   - No way to reset passwords
>   - No Remember Previous User/Login function.

Install [Yubico Login for Windows software](https://www.yubico.com/products/computer-login-tools/)

- If u're signed in with a Microsoft account, switch to a local account instead. `System settings>Accounts>Your info`

- Windows' automatic login is not compatible

- Set a password if there isn't one

- Set a windows hello PIN in case of any thing goes wrong when configuring

- Better to have at least 2 security keys in case of a key lost

- Check ur user name:  powershell `whoami` command

  ![whoami](https://pinry.yuiofastora.com/media/2/b/2bc1cc9ac7b613d9740efb6e221a6510/whoami.png)

  In this case, the username is yui

Start Login Configuration of Yubico Login for windows

![login config](https://pinry.yuiofastora.com/media/3/5/35537438e44b2b8d1e734f8e53c78532/loginConfig.png)

Choose a slot and select `Manually input secret`, select create backup device for each user if u have at least 2 keys(recommended). Generating recovery code is recommended **if u only have one key**.

![](https://pinry.yuiofastora.com/media/a/6/a640ba30ed7c2758d53804f2ab2e3612/config.png)

We're using slot 2 here since slot 2 is for long touch, we can set slot 1 for windows password later(Optional).

Generate a random hex string for secret, 

https://www.browserling.com/tools/random-hex

40 digits is enough here.

Input the generated secret

![](https://pinry.yuiofastora.com/media/d/1/d1bdd4084d2a09531dca66af84ffe9bc/secret.png)

Follow the instructions and secure the recovery code(if there is one)

Restart and login, now ur PC is secured with Yubikey

> Optional:
>
> If u're tired of entering password every time, u can set a strong password, then store as static password into ur yubikey slot 1(we've used slot 2 before)
>
> ![](https://pinry.yuiofastora.com/media/9/5/95de2c9708a82809e59c5fb31b94c8a6/slot1.png)
>
> Start the yubikey manager, go to Applications>OTP, configure slot 1(short touch) for static password. Choose allow any character and input ur windows password. Now u can touch the key to enter the password automatically when login.

If everything works, remove ur windows hello PIN. After that u can only use security key to access ur PC.
