---
title: 关于小米5s刷 Resurrection Remix OS 部分应用没有声音 & 刷机步骤
date: 2017-11-11 20:58:24
categories:
- 安卓
- rom
tags:
- 安卓
- rom
- Resurrection Remix OS
- 小米5s
---

#### 问题解决
刷成功后使用了半天,功能基本正常
之后发现微信/网易云音乐等应用没有声音,但是铃声/通知/游戏等应用是有声音的,在XDA论坛的一篇博文上找到了解决的方法,需要刷一个补丁包
补丁包页面:
[capricornCNFirmware](https://github.com/fjtrujy/capricornCNFirmware/releases)
点击download下的zip链接

<!-- more -->

<img src="http://oph0392na.bkt.clouddn.com/2017-11-11-WX20171111-211736@2x.png" height="1430" width="784">


下载后重启进入`Recovery`,选择sdcard中的zip包刷入即可

这里附上刷机的步骤

#### 解锁BootLoader
进入小米解锁网站 [申请解锁小米手机](http://unlock.update.miui.com)
填写资料申请,通过后下载解锁软件,根据软件提示解锁手机
[[教程] 小米手机5s 解锁（线刷必需）与上锁 详细小白教程](http://www.miui.com/thread-6055459-1-1.html)

#### 需要下载的文件
卡刷需要第三方的Recovery
下载页面 [TWRP](https://dl.twrp.me/capricorn)
选择带有`.img`的链接
![WX20171111-213756@2x](http://oph0392na.bkt.clouddn.com/2017-11-11-WX20171111-213756@2x.png)

下载adb
[adb-Windows](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)
[adb-Mac](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip)

下载卡刷包
[Resurrection Remix OS](https://sourceforge.net/projects/resurrectionremix/files/capricorn/Nougat/)
选择列表中最新的一个即可

#### 步骤

解压缩下载的adb压缩包后,打开cmd命令行或Terminal终端,cd到解压缩后的目录

打开手机设置－关于手机－然后点击MIUI版本号7次，就打开的“开发者模式”，然后再返回“设置”，点击更多“设置”　就能找到“开发者选项”,进入后打开usb调试选项

之后连接手机到电脑,如手机出现权限弹框点击同意,之后命令行下输入`adb devices`,多试几次,出现如下样式表示连接成功
![WX20171111-220325@2x](http://oph0392na.bkt.clouddn.com/2017-11-11-WX20171111-220325@2x.png)
命令行输入` adb reboot bootloader`然后回车,手机重启进入fastboot模式
命令行`fastboot devices`,显示已连接的手机代号
**macOS 10.13 下此命令不可用 提示`ERROR: Unable to create a plug-in (e00002be)` 建议使用Windows平台**
命令行`fastboot flash recovery 下载的TWRP文件名`
类似`fastboot flash recovery twrp-x.x.x-x-capricorn.img`

之后按住 音量上/加键+电源键,长按直到出现 `MI` logo后松手即进入

将下载的卡刷包传入手机
命令行`adb push filename.zip /sdcard/`,filename.zip替换为卡刷包的文件名
类似:`adb push RR-N-v5.8.5-20171011-capricorn-Weekly.zip /sdcard/`

选择[清除]-[高级清除] ,勾选 [Cache] [System] [Data] 滑动确认

之后返回首页,选择[安装] 浏览目录选择下载的卡刷包 滑动输入即可,不要忘记刷入声音相关的补丁包






