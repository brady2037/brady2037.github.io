---
title: 关于mac下gradlew命令不能使用
date: 2017-10-30 09:56:48
categories: 
- 技术
- android
- gradle
tags:
- android
- gradle
---

今天项目编译错误后需要看详细日志,发现gradlew命令提示找不到
```
bash: gradlew: command not found
```
Google了一下找到了解决的方法

macOS下使用gradlew命令需要再命令前加上`./`
比如:`./gradlew --info assembleDebug`
之后会提示权限:
```
bash: ./gradlew: Permission denied
```
运行:
```
chmod +x gradlew
```
之后就可以使用了

