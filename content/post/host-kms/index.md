---
title: "Self Hosting A KMS Server"
date: 2023-01-26T19:00:47+08:00
draft: false
tags: ['self host', 'windows', 'docker']
categories: ['Guide']
comments: true
toc: true
readingTime: true
description: "Host kms service with docker"
---
Self host kms service with docker.

> https://github.com/Py-KMS-Organization/py-kms

<!--more-->

## Setup With Docker

```
docker run -d \
	--name kms \
	-p 1688:1688 \
	-v /data/py-kms:/data \
 	pykmsorg/py-kms
```
## Office Activation

Go to the installation folder(Normallu C:\Program Files (x86)\Microsoft Office\Office16)

`cscript ospp.vbs /dstatus` Check the last 5 characters of installed product key: XXXXX. If multiple keys existed, uninstalling all keys needed.

`cscript ospp.vbs /unpkey:XXXXX` Uninstall key

`cscript ospp.vbs /inpkey:XXXX` Setup key

`cscript ospp.vbs /sethst:kms.server.com` Setup kms server

`cscript ospp.vbs /act` Activate


## Support List

- Windows Vista
- Windows 7
- Windows 8
- Windows 8.1
- Windows 10 ( 1511 / 1607 / 1703 / 1709 / 1803 / 1809 )
- Windows 10 ( 1903 / 1909 / 20H1, 20H2, 21H1, 21H2 )
- Windows 11 ( 21H2 )
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019
- Windows Server 2022
- Microsoft Office 2010 ( Volume License )
- Microsoft Office 2013 ( Volume License )
- Microsoft Office 2016 ( Volume License )
- Microsoft Office 2019 ( Volume License )
- Microsoft Office 2021 ( Volume License )

https://py-kms.readthedocs.io/en/latest/Keys.html

## Windows

1. Press`Win + R` to open `Run` ，Enter `powershell` then Press `Ctrl + Shift + Enter` to run as administrator.

2. Type `slmgr.vbs /upk`then press`Enter` to uninstall current key.

3. Type`slmgr /ipk XXXXXX`   `XXXXXX` is the key to be installed.

4. Type`slmgr /skms kms.yuiofastora.com` to set KMS server

5. Type`slmgr /ato` to start activation.

6. Check activation information

   Type `slmgr.vbs -dlv`

   or`wmic path softwarelicensingservice get OA3xOriginalProductKey`


## Office

1. Install VOL edition Office，recommend deploying with Office Tool

    May need sepecific version of  .NetFramework support

2. Start `Office Tool Plus`,click `deploy`. Uninstall current office products first. 

   Add products `Office 专业增强版 2019 - 批量许可证`

   在下方的应用程序设置里勾选要安装的Office应用，这里就只选择了`Word, Excel, PowerPoint`

   添加所需语言

   右侧的部署设置里体系结构选择`64位`(除非是很老很老的电脑才选择32位)，通道选择 `Office 2019 企业长期版`，部署模式选择`安装`，部署完成后选择`无`即可，安装模块选择`Office部署工具`

   其他设置都无需改动，保持默认

   点击`开始部署`进行安装，安装时间视网络情况可能较长，耐心等待安装完成

3. 安装完成后点击菜单栏的激活选项，展开许可证管理，选择产品为`Office专业增强版 2019 - 批量许可证[ProPlus2019volume]` 。激活前建议先清除干净本机的激活状态。，点击安装许可证旁的菜单按钮，选择清除激活状态。点击安装许可证，等待安装完成，如无意外，右侧会显示产品密钥安装成功。

4. Set KMS server

   Fill in the values below

   KMS host: `kms.yuiofastora.com`

   KMS port: `1688`

   save config

5. Check actication information

   Open and create any Word, Excel or PPT file，click Files > Account，Check Office actication information.

Get keys from[Py-kms](https://py-kms.readthedocs.io/en/latest/Keys.html) or [Microsoft](https://learn.microsoft.com/zh-cn/windows-server/get-started/kms-client-activation-keys)
