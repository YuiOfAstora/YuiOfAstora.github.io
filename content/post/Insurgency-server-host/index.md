---
title: "Insurgency Sandstorm 开服指南"
date: 2023-01-26T18:55:00+08:00
draft: false
tags: ['self host', 'game']
categories: ['Guide','Docs']
comments: true
toc: true
readingTime: true
description: "本文是对[官方开服指南的汉化,同时有所增删"
---
> 本文是对[官方开服指南](https://insurgencysandstorm.mod.io/guides/server-admin-guide)的汉化,同时有所增删
>
> 配置文件参考:
> https://github.com/JonathanChuyan/sandstorm-server-config
> https://github.com/zDestinate/INS_Sandstorm
> [视频教程](https://www.bilibili.com/video/BV1A44y1C7it/)
>
> <iframe src="//player.bilibili.com/player.html?aid=975109598&bvid=BV1A44y1C7it&cid=396238073&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
> 最后更新日期 2022-10-26

<!--more-->

## 前置需求(Windows )

开始服务器安装前，先确保你已安装以下前置

[Visual C++ 2015  运行库](https://www.microsoft.com/en-us/download/details.aspx?id=53587)

[Visual C++ 2017运行库](https://aka.ms/vs/15/release/vc_redist.x64.exe)

## SteamCMD

### 命令行界面

使用该命令行工具安装升级服务器文件

查看V社官方下载安装教程:

- [**Windows**](https://developer.valvesoftware.com/wiki/SteamCMD:zh-cn#Windows)
- [**Linux**](https://developer.valvesoftware.com/wiki/SteamCMD:zh-cn#Linux)

如果你使用的是windows,将下载的steamcmd压缩包文件解压到合适的路径,安装过程中会经常需要回到此路径进行操作

大多数SteamCMD命令都需要知道应用程序ID,沙暴服务器端的应用ID是581330(注意，服务器端应用ID和玩家游戏客户端的ID是不同的，这里需要的是服务器端)

接下来安装沙暴服务器端:

1. 启动SteamCMD，初次启动需要点时间下载更新
2. 在SteamCMD命令行窗口中输入`login anonymous`并回车，这样可以无用户匿名登录下载许多游戏服务器端
3. 输入`app_update 581330 validate`并回车，将会开始下载安装沙暴服务器端(以后如果游戏有了更新，同样使用该命令更新服务器端)
4. 安装完成后输入`quit`回车退出SteamCMD

安装完后的文件位置在**SteamCMD安装位置/steamapps/common**内，具体参考自己SteamCMD的安装路径

### 图形化界面（Windows）

如果不想用命令行，也可以使用开源GUI应用下载安装沙暴服务器文件

https://github.com/DioJoestar/SteamCMD-GUI

## 启动服务器

编写一个启动脚本,

### Windows

1.  在沙暴服务器安装路径sandstorm_server(之前提到的steamapps/common里)创建一个文本文档
   其实在哪编写都无所谓，只要你能在正确的路径调用启动,为了方便这里直接在安装路径里创建了 
2.  编写启动命令(具体怎么写见下方）,直接把下方的命令复制进去就行，什么也不需要改，现在我们只需要初次启动一下服务器来生成基本的游戏文件,后面再去修改启动脚本即可 
3.  快捷键大法CTRL + Shift + S保存文件，另存为start.bat,  注意是 `.bat后缀文件`

### Linux

1. cd 到安装目录(steamapps/common/sandstorm_server内)
2. 编写并创建start.sh文件 保存并`chmod +x start.sh` 给个执行权限(具体怎么写见下方,直接把下方的命令复制进去就行，什么也不需要改，现在我们只需要初次启动一下服务器来生成基本的游戏文件,后面再去修改启动脚本即可)

| 平台    | 命令                                                         |
| ------- | ------------------------------------------------------------ |
| Windows | InsurgencyServer.exe Oilfield?Scenario=Scenario_Refinery_Push_Security?MaxPlayers=28 -Port=27102 -QueryPort=27131 -log -hostname="My Server" |
| Linux   | Insurgency/Binaries/Linux/InsurgencyServer-Linux-Shipping Oilfield?Scenario=Scenario_Refinery_Push_Security?MaxPlayers=28 -Port=27102 -QueryPort=27131 -log -hostname="My Server" |

运行脚本,启动一下游戏,看到命令行显示地图已经加载后就可以直接关掉了,接下来我们开始修改启动脚本

## 修改启动脚本



对于启动脚本命令，大致格式如下



```
可执行程序位置 空格 服务器启动后的初始地图场景?其他Travel参数?其他Travel参数 空格 -其他命令行参数 空格 -其他命令行参数
```



首先是Travel参数(NWI给的名字就叫这玩意),每个参数间无空格分隔，而是使用?分隔, 上面的Maxplayer最大玩家数, 游戏端口号, Query端口号都是travel参数

然后是命令行参数,每个参数以 - 开头并用空格分隔, 比如上面的-log(启动日志), -hostname（服务器房间名）

对应到前面的命令就是

```
服务器应用程序位置 炼油厂地图?炼油厂地图模式?最大玩家数=28 -游戏端口号=27102 -Query端口号=27131 -启用日志 -服务器房间名="我的沙暴服务器"
```

首先我们可以更改服务器房间名,玩家在浏览社区服务器的时候，你的房间会以该名称显示,比如这里我们叫  游骑兵#精英特训; 最大玩家数设为8，游戏端口号和Query端口保持官方默认

启动脚本(Linux)则应为

```
Insurgency/Binaries/Linux/InsurgencyServer-Linux-Shipping Oilfield?Scenario=Scenario_Refinery_Push_Security?MaxPlayers=8 -Port=27102 -QueryPort=27131 -log -hostname="游骑兵#精英特训"
```

### 命令行参数

| 参数                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| -log                   | 启用日志窗口                                                 |
| -hostname="我的服务器" | 设置服务器房间名是                                           |
| -EnableCheats          | 允许作弊以供测试                                             |
| -Port=端口号           | 玩家加入游戏所使用的端口                                     |
| -QueryPort=端口号      | Steam查询服务器所使用的端口,以让服务器能正常出现在游戏的社区服务器列表中 |

### Travel参数



跟在地图场景代码后,以?分隔

| 参数       | 描述                           |
| ---------- | ------------------------------ |
| password   | 给服务器上锁，需要密码才能加入 |
| MaxPlayers | 服务器最大玩家数               |

## 端口与防火墙放行



至少需要两个端口, 玩家加入端口,查询端口,如果使用rcon,则还需要rcon端口(这个在配置文件里指定,后面会讲)



官方默认的是27102, 27131,27105,可以自行调整



这里笔者就全部按官方默认了,



如果使用的是云主机，请到对应平台商提供的管理面板新建防火墙放行规则



首先是玩家加入端口,需要放行TCP, UDP（TCP,UDP都必须放行）; Query端口需要放行TCP , UDP (好像只放行UDP也可以)  RCON端口只需要放行UDP



## 添加管理员名单



可以添加指定steam玩家作为管理员,管理员在加入游戏后按下管理员面板键(自行到游戏内按键绑定里查找)即可进行踢人,封禁,换图等操作



找到沙暴服务器安装目录(steamapps/common/sandstorm_server),



### Windows



1.  进入Insurgency文件夹,新建Config文件夹然后进入 
2.  在Config文件夹内新建Server文件夹并进入 
3.  创建Admins.txt，将玩家的steam 64 ID写入，每一行一个steam 64 ID
   完整路径样例: steamapps\common\sandstorm_server\Insurgency\Config\Server\Admins.txt 



### Linux



1.  进入Insurgency目录, 新建目录 `mkdir -p Config/Server`
2. `cd Config/Server`
3.  创建Admins.txt，将玩家的steam 64 ID写入，每一行一个steam 64 ID
   完整路径样例  steamapps/common/sandstorm_server/Insurgency/Config/Server/Admins.txt 



如果你有多个Admins名单，可以使用在启动脚本中加入 参数-AdminList选择要使用哪一个, 比如要使用



steamapps/common/sandstorm_server/Insurgency/Config/Server/Admins2.txt，则应在启动脚本中加入 `-AdminList=Admins2`,不指定-AdminList参数则默认读取Admins.txt



steam 64 ID可以通过该[网站](https://steamid.io/)查询到



## 添加服务器公告



服务器可以在玩家加载游戏的时候显示编辑好的公告内容(Message of the Day)。将写好的公告直接放入Motd.txt内,该文件默认存放路径为(没有就新建) Insurgency/Config/Server/Motd.txt (与前文的Admins.txt在同一路径)。



如果你有多个Motd文件，在启动脚本里使用`-motd`指定要使用哪一个, 如-motd=MyOtherMOTD.txt



## 远程控制(Rcon)



服务器管理员可以通过Rcon远程管理服务器而无需加入游戏。



要启用rcon，需要在启动脚本中加入参数: `-Rcon -RconPassword=rcon密码 -RconListenPort=27015` ;也可以在Game.ini中配置(后文会讲)



官方给出的Rcon默认监听端口号为27015（可以自行调整 ），注意将端口号进行防火墙放行，放行协议为UDP



### Rcon 命令



输入`help`可以获取所有命令及其用法和描述



必要参数用“<”和“>”括起来，而可选参数用“[”和“]”括起来。 “net ID”通常是指用户的 Steam ID。

| **命令**               | 参数                              | 功能                                                         |
| ---------------------- | --------------------------------- | ------------------------------------------------------------ |
| ban                    | <id/netid/名字> [时长分钟] [原因] | 封禁玩家                                                     |
| banid                  | [时长分钟] [原因]                 | 以steam ID封禁玩家，不需要玩家正在服务器游玩                 |
| gamemodeproperty       | [new value]                       | 获取或设置地图模式属性                                       |
| help                   |                                   | 显示所有可用命令                                             |
| kick                   | <id/netid/name> [原因]            | 将玩家踢出服务器                                             |
| listban                |                                   | 显示服务器已封禁的所有玩家                                   |
| listgamemodeproperties | [property filter]                 | 显示目前可用的游戏模式属性                                   |
| listplayers            |                                   | 显示目前正在服务器游玩的玩家                                 |
| maps                   | [level filter]                    | 显示可用的地图                                               |
| permaban               | <id/netid/name> [原因]            | 永久封禁玩家                                                 |
| restartround           | [0 = 不换边, 1 = 换边]            | 重新开始当前回合                                             |
| say                    |                                   | 像所有玩家发送一条消息，玩家将在左上角看到一条带[Admin]前缀的消息 |
| scenarios              | [level filter]                    | 显示所有可用地图场景                                         |
| travel                 | <目标地图的完整代码>              | 将游戏地图场景立刻改为其他地图                               |
| travelscenario         | <场景代码>                        | 更改到指定场景.                                              |
| unban                  |                                   | 为用户解封                                                   |



示例`travel Tell?Scenario=Scenario_Tell_Checkpoint?Lighting=Day`



## 启用经验获取



服务器默认是没有经验获取的，需要使用绑定Steam生成的Token才行



进入该网站[GameStats Token Generator](https://gamestats.sandstorm.game/) ,点击Connect using steam进行授权

点击Generate Token生成GameStats token

然后便可看到类似这样的一行



```
-GameStatsToken=14172XXXXXXXXXXXXX
```



直接将其作为参数加入到启动脚本中即可



一个token可以给多个服务器使用

## 地图轮换池



默认情况下，服务器会轮换所有可用的PVP对抗地图



你可以通过MapCycle.txt来指定服务器的地图轮换, 该文件存放在Insurgency/Config/Server/MapCycle.txt(没有就新建,和前文的Admins.txt 在同一路径下),每一行一个地图



如果你有多个地图池, 在启动脚本中使用-MapCycle=MyOtherMapCycle指定



MapCycle文件中最简单的格式就是直接将地图场景代码写入，每一行一个,如：



```plain
Scenario_Crossing_Skirmish
Scenario_Hideout_Skirmish
Scenario_Precinct_Skirmish
Scenario_Refinery_Skirmish
Scenario_Farmhouse_Skirmish
Scenario_Summit_Skirmish
```



如果你想指定一些特殊模式，比如硬核Checkpoint，则需要添加Mode参数，格式如下



```
(Scenario="Scenario_Town_Checkpoint_Security",Mode="CheckpointHardcore")
```



## 所有地图场景代码(14张图)



注意不是所有地图都有Outpost ,Push ,Survival等模式



#### **Crossing (Canyon)**

| **Scenario****Name**                    | **Description**     |
| --------------------------------------- | ------------------- |
| Scenario_Crossing_Checkpoint_Insurgents | Checkpoint 叛军     |
| Scenario_Crossing_Checkpoint_Security   | Checkpoint 安全部队 |
| Scenario_Crossing_Domination            | Domination          |
| Scenario_Crossing_Firefight_West        | Firefight West      |
| Scenario_Crossing_Frontline             | Frontline           |
| Scenario_Crossing_Push_Insurgents       | Push 叛军           |
| Scenario_Crossing_Push_Security         | Push 安全部队       |
| Scenario_Crossing_Skirmish              | Skirmish            |
| Scenario_Crossing_Team_Deathmatch       | Team Deathmatch     |



#### **Farmhouse**

| **Scenario****Name**                     | **Description**     |
| ---------------------------------------- | ------------------- |
| Scenario_Farmhouse_Checkpoint_Insurgents | Checkpoint 叛军     |
| Scenario_Farmhouse_Checkpoint_Security   | Checkpoint 安全部队 |
| Scenario_Farmhouse_Domination            | Domination          |
| Scenario_Farmhouse_Firefight_East        | Firefight East      |
| Scenario_Farmhouse_Firefight_West        | Firefight West      |
| Scenario_Farmhouse_Frontline             | Frontline           |
| Scenario_Farmhouse_Push_Insurgents       | Push 叛军           |
| Scenario_Farmhouse_Push_Security         | Push 安全部队       |
| Scenario_Farmhouse_Skirmish              | Skirmish            |
| Scenario_Farmhouse_Team_Deathmatch       | Team Deathmatch     |
| Scenario_Farmhouse_Survival              | Survival            |



#### **Hideout (Town)**

| **Scenario****Name**                   | **Description**       |
| -------------------------------------- | --------------------- |
| Scenario_Hideout_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Hideout_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Hideout_Domination            | Domination            |
| Scenario_Hideout_Firefight_East        | Firefight East        |
| Scenario_Hideout_Firefight_West        | Firefight West        |
| Scenario_Hideout_Frontline             | Frontline             |
| Scenario_Hideout_Push_Insurgents       | Push Insurgents       |
| Scenario_Hideout_Push_Security         | Push Security         |
| Scenario_Hideout_Skirmish              | Skirmish              |
| Scenario_Hideout_Team_Deathmatch       | Team Deathmatch       |
| Scenario_Hideout_Survival              | Survival              |



#### **Hillside (Sinjar)**

| **Scenario****Name**                    | **Description**                |
| --------------------------------------- | ------------------------------ |
| Scenario_Hillside_Checkpoint_Insurgents | Checkpoint Insurgents          |
| Scenario_Hillside_Checkpoint_Security   | Checkpoint Security            |
| Scenario_Hillside_Domination            | Domination                     |
| Scenario_Hillside_Firefight_East        | Firefight East                 |
| Scenario_Hillside_Firefight_West        | Firefight West                 |
| Scenario_Hillside_Frontline             | Frontline                      |
| Scenario_Hillside_Push_Insurgents       | Push Insurgents                |
| Scenario_Hillside_Push_Security         | Push Security (INS2014 layout) |
| Scenario_Hillside_Skirmish              | Skirmish                       |
| Scenario_Hillside_Team_Deathmatch       | Team Deathmatch                |
| Scenario_Hillside_Survival              | Survival                       |



#### **Ministry**

| **Scenario****Name**                    | **Description**       |
| --------------------------------------- | --------------------- |
| Scenario_Ministry_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Ministry_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Ministry_Domination            | Domination            |
| Scenario_Ministry_Firefight_A           | Firefight             |
| Scenario_Ministry_Skirmish              | Skirmish              |
| Scenario_Ministry_Team_Deathmatch       | Team Deathmatch       |



#### **Outskirts (Compound)**

| **Scenario****Name**                     | **Description**       |
| ---------------------------------------- | --------------------- |
| Scenario_Outskirts_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Outskirts_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Outskirts_Firefight_East        | Firefight East        |
| Scenario_Outskirts_Firefight_West        | Firefight West        |
| Scenario_Outskirts_Frontline             | Frontline             |
| Scenario_Outskirts_Push_Insurgents       | Push Insurgents       |
| Scenario_Outskirts_Push_Security         | Push Security         |
| Scenario_Outskirts_Skirmish              | Skirmish              |
| Scenario_Outskirts_Team_Deathmatch       | Team Deathmatch       |
| Scenario_Outskirts_Survival              | Survival              |



#### **Precinct**

| **Scenario****Name**                    | **Description**       |
| --------------------------------------- | --------------------- |
| Scenario_Precinct_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Precinct_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Precinct_Firefight_East        | Firefight East        |
| Scenario_Precinct_Firefight_West        | Firefight West        |
| Scenario_Precinct_Frontline             | Frontline             |
| Scenario_Precinct_Push_Insurgents       | Push Insurgents       |
| Scenario_Precinct_Push_Security         | Push Security         |
| Scenario_Precinct_Skirmish              | Skirmish              |
| Scenario_Precinct_Team_Deathmatch       | Team Deathmatch       |
| Scenario_Precinct_Survival              | Survival              |



#### **Refinery (Oilfield)**

| **Scenario****Name**                    | **Description**       |
| --------------------------------------- | --------------------- |
| Scenario_Refinery_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Refinery_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Refinery_Firefight_West        | Firefight West        |
| Scenario_Refinery_Frontline             | Frontline             |
| Scenario_Refinery_Push_Insurgents       | Push Insurgents       |
| Scenario_Refinery_Push_Security         | Push Security         |
| Scenario_Refinery_Skirmish              | Skirmish              |
| Scenario_Refinery_Team_Deathmatch       | Team Deathmatch       |



#### **Summit (Mountain)**

| **Scenario****Name**                  | **Description**       |
| ------------------------------------- | --------------------- |
| Scenario_Summit_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Summit_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Summit_Firefight_East        | Firefight East        |
| Scenario_Summit_Firefight_West        | Firefight West        |
| Scenario_Summit_Frontline             | Frontline             |
| Scenario_Summit_Push_Insurgents       | Push Insurgents       |
| Scenario_Summit_Push_Security         | Push Security         |
| Scenario_Summit_Skirmish              | Skirmish              |
| Scenario_Summit_Team_Deathmatch       | Team Deathmatch       |
| Scenario_Summit_Survival              | Survival              |



#### **Power Plant (PowerPlant)**

| **Scenario****Name**                      | **Description**       |
| ----------------------------------------- | --------------------- |
| Scenario_PowerPlant_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_PowerPlant_Checkpoint_Security   | Checkpoint Security   |
| Scenario_PowerPlant_Domination            | Domination            |
| Scenario_PowerPlant_Firefight_East        | Firefight East        |
| Scenario_PowerPlant_Firefight_West        | Firefight West        |
| Scenario_PowerPlant_Push_Insurgents       | Push Insurgents       |
| Scenario_PowerPlant_Push_Security         | Push Security         |
| Scenario_PowerPlant_Survival              | Survival              |



#### **Tideway (Buhriz)**

| **Scenario****Name**                   | Description           |
| -------------------------------------- | --------------------- |
| Scenario_Tideway_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Tideway_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Tideway_Domination            | Domination            |
| Scenario_Tideway_Firefight_West        | Firefight West        |
| Scenario_Tideway_Frontline             | Frontline             |
| Scenario_Tideway_Push_Insurgents       | Push Insurgents       |
| Scenario_Tideway_Push_Security         | Push Security         |



#### **Bab**

| **Scenario****Name**               | **Des**cription       |
| ---------------------------------- | --------------------- |
| Scenario_Bab_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Bab_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Bab_Domination            | Domination            |
| Scenario_Bab_Firefight_East        | Firefight East        |
| Scenario_Bab_Outpost               | Outpost               |
| Scenario_Bab_Push_Insurgents       | Push Insurgents       |
| Scenario_Bab_Push_Security         | Push Security         |



#### **Citadel**

| **Scenario****Name**                   | Description           |
| -------------------------------------- | --------------------- |
| Scenario_Citadel_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Citadel_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Citadel_Domination            | Domination            |
| Scenario_Citadel_Firefight_East        | Firefight East        |
| Scenario_Citadel_Outpost               | Outpost               |
| Scenario_Citadel_Push_Insurgents       | Push Insurgents       |
| Scenario_Citadel_Push_Security         | Push Security         |
| Scenario_Citadel_Survival              | Survival              |



高城的地图模式列表不知道为什么官方从来没有在开服指南里写出过,这里笔者也没有去一一测试，就只给出笔者所玩过的模式

#### Tell

| **Scenario****Name**                | Description           |
| ----------------------------------- | --------------------- |
| Scenario_Tell_Checkpoint_Insurgents | Checkpoint Insurgents |
| Scenario_Tell_Checkpoint_Security   | Checkpoint Security   |
| Scenario_Tell_Outpost               | Outpost               |



如果你想指定地图是白天还是黑夜, 格式如下(不加此参数,默认为白天),



```
(Scenario="Scenario_Town_Checkpoint_Security",Lighting="Night")
```



Lighting参数值为Day或Night



如果你想指定服务器启动地图的模式,在启动脚本中其后加上 `?Lighting=Night



## Mod



沙暴的Mod社区托管在Mod.io，想要为服务器加mod需要Mod.io的token



先注册Mod.io的账户



点击头像进入用户中心,选择API access



在OAuth 2 management的 generate access token下给其命名,然后给Read读取权限(Write写入权限没必要)，点击Create token生成



点击Copy Token进行复制



编辑



steamapps/common/sandstorm_server/Insurgency/Saved/Config/LinuxServer/Engine.ini

如果你是Windows服务器，启动后没有自动生成Insurgency\Saved\Config\WindowsServer\Engine.ini，请自行创建相应文件夹和文件

（Windows服务器就是steamapps/common/sandstorm_server/Insurgency/Saved/Config/WindowsServer/Engine.ini）



加入一下代码



```plain
[/Script/ModKit.ModIOClient]
bHasUserAcceptedTerms=True
AccessToken=Token复制到这里
```



然后保存更改



在启动脚本中加入 `-Mods`参数启用mod



要指定服务器使用哪些mod,在Insurgency/Config/Server/Mods.txt内写入Mod ID (ID 去 mod.io的页面找),每一行一个



若有多个Mod列表，在启动脚本中加入`-ModList=MyCustomModList.txt`进行指定



在启动脚本中指定要使用的mod: `-CmdModList="mod ID1,mod ID2,mod ID3"`



加入 `ModDownloadTravelTo=`参数指定Mod下载好后要加载的地图(必要) , 这是服务器下载完Mod启动时真正会加载的地图,格式如



```plain
Canyon?Scenario=Scenario_Crossing_Checkpoint_Security?Port=7777?QueryPort=27015?MaxPlayers=8 -log -hostname="" -Mods -ModDownloadTravelTo=Canyon?Scenario=Scenario_Crossing_Checkpoint_Security
```



## Mutators



官方提供了一些整活模式(一些是以前的模式，但是由于玩的人不多就删除了,可通过mutators调出来)，命名为Mutators（突变体的意思，官方mutator这名字选的怪怪的，就不直译了）; 同时一些mod启用也需要指定mutators参数，对应代码看mod介绍页面



可以在启动脚本中加入`-mutators=mutator代码名1,mutator代码名2`来指定

| 代码名                 | **Mutator 名字**     | **描述**                                                     |
| ---------------------- | -------------------- | ------------------------------------------------------------ |
| AllYouCanEat           | All You Can Eat      | 以100补给点开始游戏                                          |
| AntiMaterielRiflesOnly | Anti-Materiel Only   | 只允许使用反器材步枪                                         |
| BoltActionsOnly        | Bolt-Actions Only    | 只允许使用栓动式步枪                                         |
| Broke                  | Broke                | 以0补给点开始游戏                                            |
| BudgetAntiquing        | Budget Antiquing     | 仅限于旧的和廉价的武器                                       |
| BulletSponge           | Bullet Sponge        | 增加生命值                                                   |
| Competitive            | Competitive          | 装备更贵，每一回合时间更短，占点更快                         |
| CompetitiveLoadouts    | Competitive Loadouts | 玩家装备变成PvP的装备枪械                                    |
| FastMovement           | Fast Movement        | 行动更快                                                     |
| Frenzy                 | Frenzy               | 敌人只会用近战，但是有特殊敌人（比如自爆哥）                 |
| FullyLoaded            | Fully Loaded         | 仅有武器大师一个职业，能够使用所有武器装备（望远镜有bug,叫不出火力支援） |
| Guerrillas             | Guerrillas           | 以5补给点开始游戏                                            |
| Gunslingers            | Gunslingers          | 敌人使用左轮                                                 |
| Hardcore               | Hardcore             | 硬核模式，移动更慢，占点更慢                                 |
| HeadshotOnly           | Headshots Only       | 只有打头才造成伤害                                           |
| HotPotato              | Hot Potato           | 敌人死亡时会掉落手雷                                         |
| LockedAim              | Locked Aim           | 武器总是对准屏幕中央                                         |
| MakarovsOnly           | Makarovs Only        | 只能使用马卡洛夫手枪                                         |
| NoAim                  | No Aim Down Sights   | 不允许开镜                                                   |
| NoDrops                | No Drops             | 敌人不掉落武器                                               |
| PistolsOnly            | Pistols Only         | 仅能使用手枪                                                 |
| Poor                   | Poor                 | 以有限的少量补给点数开始游戏                                 |
| ShotgunsOnly           | Shotguns Only        | 仅能使用霰弹枪                                               |
| SlowCaptureTimes       | Slow Capture Times   | 占点需要更长时间                                             |
| SlowMovement           | Slow Movement        | 移动变慢                                                     |
| SoldierOfFortune       | Soldier of Fortune   | 随着分数增加获取补给点数                                     |
| SpecialOperations      | Special Operations   | 以30补给点开始游戏                                           |
| Strapped               | Strapped             | 以1补给点开始游戏                                            |
| Ultralethal            | Ultralethal          | 所有人一枪即死                                               |
| Vampirism              | Vampirism            | 吸血,获得与对敌人造成的伤害同等的血量                        |
| Warlords               | Warlords             | 以10补给点开始游戏                                           |



## 游戏配置



游戏大部分配置都在 steamapps\common\sandstorm_server\Insurgency\Saved\Config\LinuxServer\Game.ini （steamapps\common\sandstorm_server\Insurgency\Saved\Config\WindowsServer\Game.ini）内

如果你是Windows服务器，启动后没有自动生成Insurgency\Saved\Config\WindowsServer\Game.ini，请自行创建相应文件夹和文件

每部分都有其对应的值



时长单位一般都为秒



```plain
[/Script/Insurgency.INSGameMode]
```



该部分为游戏的通用配置



对应的属性和值如下

| **变量**                  | **默认值** | **描述**                                                     |
| ------------------------- | ---------- | ------------------------------------------------------------ |
| bKillFeed                 | False      | 启用右上角击杀反馈                                           |
| bKillFeedSpectator        | True       | 对观战者和回放启用击杀反馈                                   |
| bKillerInfo               | True       | 对被击杀者显示击杀者信息                                     |
| bKillerInfoRevealDistance | False      | 显示被击杀距离                                               |
| TeamKillLimit             | 3          | 最大TK次数，TK队友超过此限制次数将被踢出                     |
| TeamKillGrace             | 0.2        | TK宽限计时器（不明）                                         |
| TeamKillReduceTime        | 90         | TK计数每多少秒减少1；比如如果玩家TK了两次，90秒内未再TK队友，TK计数会降1 |
| bDeadSay                  | False      | 允许存活玩家看到死亡玩家的信息                               |
| bDeadSayTeam              | True       | 允许存活玩家看到死亡玩家的信息(队内信息)                     |
| bVoiceAllowDeadChat       | False      | 允许存活玩家听到死亡玩家的语音                               |
| bVoiceEnemyHearsLocal     | True       | 附近的敌人能听到语音                                         |



多人PVP 模式配置



#### **[/Script/Insurgency.INSMultiplayerMode]**

| 变量                                  | **默认值** | **描述**                                                  |
| ------------------------------------- | ---------- | --------------------------------------------------------- |
| bKillFeedGameStartingIntermissionTime | 5          | 在开始游戏前的准备时间等待额外玩家的额外时长              |
| WinTime                               | 5          | 进入回合结算前的静止时长                                  |
| PostRoundTime                         | 15         | 回合结算信息的时长                                        |
| PostGameTime                          | 15         | 游戏结束结算信息的时长                                    |
| bAutoAssignTeams                      | True       | 自动分配队伍                                              |
| bAllowFriendlyFire                    | True       | 启用友伤                                                  |
| FriendlyFireModifier                  | 0.2        | 友军伤害比例                                              |
| FriendlyFireReflect                   | 0          | 友伤反弹比例                                              |
| bAutoBalanceTeams                     | True       | 启用队伍自动平衡                                          |
| AutoBalanceDelay                      | 10         | 检测队伍平衡的间隔时长                                    |
| bMapVoting                            | True       | 启用下一局地图投票                                        |
| bUseMapCycle                          | True       | 启用地图轮换池(MapCycle)，如果为False，则无限循环当前地图 |
| bVoiceIntermissionAllowAll            | True       | 允许各回合的间隔时间里两队玩家听到对方语音(欢乐嘲讽)      |
| IdleLimit                             | 150        | 玩家挂机被踢出的时长限制                                  |
| IdleLimitLowReinforcements            | 90         | 低波数增援情况下，玩家挂机时长限制                        |
| IdleCheckFrequency                    | 30         | 每多长时间检查挂机玩家                                    |



例



```plain
[/Script/Insurgency.INSGameMode]
bKillFeed=True
bKillerInfoRevealDistance=True   
 
[/Script/Insurgency.INSMultiplayerMode]
bAllowFriendlyFire=False
```



各游戏模式的配置



每个模式对应的部分如下



- **Push**: `[/Script/Insurgency.INSPushGameMode]`
- **Frontline**: `[/Script/Insurgency.INSFrontlineGameMode]`
- **Skirmish**: `[/Script/Insurgency.INSSkirmishGameMode]`
- **Survival**: `[/Script/Insurgency.INSSurvivalGameMode]`
- **Firefight**: `[/Script/Insurgency.INSFirefightGameMode]`
- **Checkpoint**: `[/Script/Insurgency.INSCheckpointGameMode]`
- **Outpost**: `[/Script/Insurgency.INSOutpostGameMode]`
- **Team Deathmatch:**`[/Script/Insurgency.INSTeamDeathmatchGameMode]`



以下General 部分的设置放置在合适的位置可以通用于全局



#### **General ([/Script/Insurgency.INSGameMode])**

| **Variable**               | **Default** | **Description**                                    |
| -------------------------- | ----------- | -------------------------------------------------- |
| ObjectiveCaptureTime       | Varies      | 占领敌对点位的时长                                 |
| ObjectiveResetTime         | -1          | 没人在点时，占点进度多久开始倒退 负数禁用          |
| ObjectiveSpeedup           | 0.25        | 每多一个人可以增加的占点速度倍数                   |
| ObjectiveMaxSpeedupPlayers | 4           | 最多多少人能加速占点(超过此人数，占点速度不再提升) |



#### **General ([/Script/Insurgency.INSMultiplayerMode])**

| **Variable**           | **Default** | **Description**                                              |
| ---------------------- | ----------- | ------------------------------------------------------------ |
| MinimumPlayers         | 1           | 每队需要多少玩家才能开始游戏                                 |
| RoundLimit             | Varies      | 最大回合数                                                   |
| WinLimit               | Varies      | 需要赢多少回合算本局游戏胜利                                 |
| GameTimeLimit          | -1          | 一场游戏最多持续多长 负数禁用                                |
| PreRoundLimit          | 10          | 游戏开局时的静止时长                                         |
| RoundTime              | Varies      | 回合时长                                                     |
| OverTime               | 60          | 超过回合时长时，若点内还在争夺的加时时长                     |
| TeamSwitchTime         | 10          | 换队时长                                                     |
| SwitchTeamsEveryRound  | Varies      | 换边频率, 0 从不，1 每一回合换一次边， 2 每两回合换一次边，以此类推 |
| bAllowPlayerTeamSelect | True        | 允许玩家换边                                                 |
| bBots                  | False       | 启用AI队友                                                   |
| BotQuots               | Varies      | AI队友填补到每边多少人数                                     |
| InitialSupply          | 15          | 起始补给点数                                                 |
| MaximumSupply          | 15          | 最大可获取补给点数                                           |
| bSupplyGainEnabled     | False       | 启用补给点数获取(启用后通过设置SupplyGainFrequency,可以让玩家每获得一定分数就获得1补给点) |
| bAwardSupplyInstantly  | False       | 补给无冷却                                                   |
| SupplyGainFrequency    | 150         | 每多少分获得1补给点                                          |



#### **Push [/Script/Insurgency.INSPushGameMode]**

| **Variable**               | **Default** | **Description**                                              |
| -------------------------- | ----------- | ------------------------------------------------------------ |
| RoundTimeExtension         | 300         | 每占领一个点，回合时间延长多久                               |
| AttackerWavesPerObjective  | 5           | 进攻方每占领一个点，获得多少波增援                           |
| AttackerWaveDPR            | 0.25        | 进攻方死亡人数达到百分多少消耗一波增援                       |
| AttackerWaveTimer          | 20          | 进攻方波数增援冷却时长(每多久一波增援)                       |
| DefenderWavesPerObjective  | 5           | 防守方每占领一个点，获得多少波增援（这里官方貌似写错了，写的进攻方） |
| DefenderWaveDPR            | 0.25        | 防守方死亡人数达到百分多少消耗一波增援                       |
| DefenderWaveTimer          | 35          | 防守方波数增援冷却时长(每多久一波增援)                       |
| LastStandSetupDelay        | 10          | 延迟最后一个防守方重生区被禁用                               |
| AdvanceAttackerSpawnsDelay | 30          | 延迟进攻方重生                                               |



#### **Frontline [/Script/Insurgency.INSFrontlineGameMode]**

| **Variable**       | **Default** | **Description**            |
| ------------------ | ----------- | -------------------------- |
| StartingWaves      | 15          | 起始增援波数               |
| CapturingBonusWave | 2           | 每占一个点获得多少增援     |
| RegressSpawnsTimer | 10          | 失去点以后给玩家的撤退时间 |



#### **Skirmish [/Script/Insurgency.INSSkirmishGameMode]**

| **Variable**              | **Default** | **Description**                            |
| ------------------------- | ----------- | ------------------------------------------ |
| DefaultReinforcementWaves | 5           | 起始波数                                   |
| CaptureBonusWaves         | 1           | 在己方弹药库完好的情况下占点获得的奖励波数 |



#### **Survival [/Script/Insurgency.INSSurvivalGameMode]**

| **Variable**                           | **Default**                                                  | **Description**                                              |
| -------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| RoundTimeExtension                     | 300                                                          | 占了一个点后 回合延长多久                                    |
| NumWaves                               | 7                                                            | 多少次成功占点回合结束（包括撤离）；有的地图游玩时实际值可能会与设置不符 |
| bEnableExtractionObjective             | True                                                         | 是否启用撤离点                                               |
| ExtractionObjectiveHoldTime            | 150                                                          | 撤离成功需要守点多久                                         |
| ExtractionSpawnStopTime                | 0                                                            | 撤离守点时，还剩多少时间就停止生成AI                         |
| BotDPRRespawnFinal                     | 0.1                                                          | 最后一个点时 AI死亡比例达到多少就重生                        |
| BotDPRRespawnFirst                     | 0.3                                                          | 第一个点时 AI 死亡比例达到多少就重生                         |
| MinimumBotsPerCompletedObjective       | 0.5                                                          | 反击时AI重生延迟，以最小玩家数计算，每完成一个点加多少AI（倍数） |
| `MaximumBotsPerCompletedObjective`     | 1.0                                                          | 以最大玩家数计算，每完成一个点加多少AI                       |
| `bResetLoadoutOnNewRound`              | False                                                        | 失败后玩家装备是否重置                                       |
| `ObjectiveDefendDistance`              | 2000                                                         | AI应该离目标多近，然后才开始保护目标而不是四处游荡           |
| `BotMinimumSpawnRange`                 | 3000                                                         | AI在生成时必须离所有玩家的最小距离                           |
| `BotMaximumSpawnRange`                 | 5000                                                         | AI重生时离玩家的最大距离不能大于BotRespawnDistance           |
| `BotRespawnDistance`                   | 10000                                                        | 离玩家多远时 AI重生在附近的玩家周 ，不能小于BotMaximumSpawnRange |
| `BotSpawnDelay`                        | 10                                                           | 敌人在游戏开始多久后才出现                                   |
| `BotRespawnDelay`                      | 1                                                            | 敌人重生延迟                                                 |
| `BotRepositionDelay`                   | 1                                                            | 敌人位置重置时间                                             |
| `bUseSpecialWaves`                     | True                                                         | 是否使用特殊敌人波数                                         |
| `SpecialWaveFrequency`                 | 2                                                            | 如果特殊敌人启用，每多少波出现一次                           |
| `SurvivalWaveConfigAssetPath`          | /Game/Game/Data/Gamemodes/SurvivalWaveConfig_Default.SurvivalWaveConfig_Default | 敌人配置路径                                                 |
| `SurvivalNightTimeWaveConfigAssetPath` | /Game/Game/Data/Gamemodes/SurvivalWaveConfig_Night.SurvivalWaveConfig_Night | 夜间地图敌人配置路径                                         |
|                                        |                                                              |                                                              |



PVE通用设置



#### **General Coop [/Script/Insurgency.INSCoopMode]**

| **Variable**         | **Default** | **Description** |
| -------------------- | ----------- | --------------- |
| AIDifficulty         | 0.5         | AI 难度         |
| bUseVehicleInsertion | True        | 允许使用皮卡车  |
| FriendlyBotQuota     | 4           | 右方AI填补数量  |
| MinimumEnemies       | 6           | 最小敌人数量    |
| MaximumEnemies       | 12          | 最大敌人数量    |



#### **Checkpoint [/Script/Insurgency.INSCheckpointGameMode]**

| **Variable**                            | **Default** | **Description**                         |
| --------------------------------------- | ----------- | --------------------------------------- |
| DefendTimer                             | 90          | 占点后AI发起反击时，玩家需要守点的时长  |
| DefendTimerFinal                        | 180         | 最后一个点占领后延长的守点时间          |
| RetreatTimer                            | 10          | 强迫Ai在失去点后撤退的时长              |
| RespawnDPR                              | 0.1         | 死亡玩家比例达到多少时 才复活AI         |
| RespawnDelay                            | 20          | AI重生延迟                              |
| PostCaptureRushTimer                    | 30          | 点被炸掉后AI rush该点的时间             |
| CounterAttackRespawnDPR                 | 0.2         | 反击时 玩家死亡比例达到多少才进行AI重生 |
| CounterAttackRespawnDelay               | 20          | 反击时AI重生延迟                        |
| ObjectiveTotalEnemyRespawnMultiplierMin | 1           | 按最小玩家数重生AI的倍数                |
| ObjectiveTotalEnemyRespawnMultiplierMax | 1           | 按最大玩家数重生AI的倍数                |
| FinalCacheBotQuotaMultiplier            | 1.5         | 若最后一个点为弹药库 增加AI数量的倍数   |



**部分mutator配置相关**



#### Headshots Only



```
[/Script/Insurgency.Mutator_HeadshotOnly]
```

| **INI Entry**     | **Default** | **在启动脚本中加入参数** | **Description**  |
| ----------------- | ----------- | ------------------------ | ---------------- |
| bCheckMeleeDamage | false       | 无                       | 近战爆头是否算入 |



#### Hot Potato



```
[/Script/Insurgency.Mutator_HotPotato]
```

| **INI Entry**        | **Default**                                                  | 在启动脚本中加入参数         | **Description**              |
| -------------------- | ------------------------------------------------------------ | ---------------------------- | ---------------------------- |
| GrenadeClass         | /Game/Game/Actors/Projectiles/BP_Projectile_M67.BP_Projectile_M67_C | 无                           | 死亡时掉落的手雷种类         |
| ThrowbackWeaponClass | /Game/Game/Actors/Weapons/Grenade/BP_Grenade_M67.BP_Grenade_M67_C | 无                           | 扔回掉落的手雷时变成哪种手雷 |
| bIgnoreHeadshots     | false                                                        | ?HotPotato_bIgnoreHeadshots= | 如果被爆头则不掉落手雷       |
| bBotsOnly            | false                                                        | ?HotPotato_bBotsOnly=        | 只有AI掉落手雷               |



#### Vampirism



```
[/Script/Insurgency.Mutator_Vampirism]
```

| **INI Entry**      | **Default** | **Travel URL Parameter**     | **Description**              |
| ------------------ | ----------- | ---------------------------- | ---------------------------- |
| bCountFriendlyFire | false       | Vampirism_bCountFriendlyFire | 把友伤算入吸血               |
| MaxHealth          | 1000        | Vampirism_MaxHealth          | 攻击其他玩家最大能获取的血量 |



#### Rcon配置



```plain
[Rcon]
bEnabled=True  //True启用
Password=密码 
ListenPort=27015 //端口号
```

| **Settings**             | **Default** | **Description**                                              |
| ------------------------ | ----------- | ------------------------------------------------------------ |
| bAllowConsoleCommands    | True        | 启用控制台命令,启用后，任何未知的 rcon 命令将被解释为控制台命令。 |
| bUseBroadcastAddress     | True        | 如果启用，则 rcon将监听所有可用的网络设备。                  |
| IncorrectPasswordBanTime | 30          | 如果客户端已达到最大尝试次数，则禁止客户端尝试连接到 rcon 的时间（以分钟为单位）。 |
| ListenAddressOverride    | 0.0.0.0     | 如果 bUseBroadcastAddress 为 False，则这是 rcon 绑定到的网络 IP。 |
| MaxPasswordAttempts      | 3           | 密码最大失败次数，超过此次数将被Ban                          |



#### 投票



如果要启用玩家投票（目前只有踢人，不能像叛乱2那样游戏中投票换图），加入



```plain
[/Script/Insurgency.TeamInfo]
bVotingEnabled=True
TeamVoteIssues=/Script/Insurgency.VoteIssueKick
```



投票有关配置



#### [/Script/Insurgency.VoteIssueKick]
