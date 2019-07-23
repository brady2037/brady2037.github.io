---
title: Cmder win10下报错 Can't register shortcut
date: 2017-12-06 11:05:50
categories:
- 技术
- windows
- 软件
tags:
- windows
- Cmder
- 软件
- 问题解决
---

> 搬运自-Github：[Can't register shortcut](https://www.noisyfox.io/android-captive-portal.html)

![](https://cloud.githubusercontent.com/assets/15866600/11176103/ed471ab8-8c39-11e5-9262-125a52ca4943.PNG)

冲突的原因是无法设置 Ctrl + ` 热键

`Win + Alt + P` 打开设置,在 `Keys & Macro` 树状菜单下Hotkey一栏下找到 Ctrl + `
Description为Minimize/Restore(Quake-style hotkey also)，选中后在下方Choose hotkey中增加alt键位,点击右下角 Save settings 即可

<!-- more -->

![](http://oph0392na.bkt.clouddn.com/TIM%E6%88%AA%E5%9B%BE20171206132956.png)

