---
title: VPS安装开源软件 FileBrowser网盘
date: 2018-02-05 17:06:56
categories:
- 技术
- vps
tags:
- linux
- vps
- linux软件
- filebrowser
- 网盘
---


> 项目主页：[FileBrowser](https://filebrowser.github.io/)

在VPS服务器上传下载文件一开始是不太方便的，刚购买VPS的那段时间，最先使用scp命令上传文件、apache2下载文件，后来了解到有相关的FTP GUI客户端FileZilla，稍微方便些，但是使用体验还是不太好，偶然在一篇博文上看到一个开源的服务器端网盘，使用体验堪比百度云，分享给大家

##### 下载
[ release页面 ](https://github.com/filebrowser/filebrowser/releases)  大家选择相应的系统版本下载
个人使用的系统为linux系列的`Ubuntu 16.04 LTS amd64`，故选择列表中的`linux-amd64-filebrowser.tar.gz`

<!-- more -->

ssh连接上vps后

```
wget https://github.com/filebrowser/filebrowser/releases/download/v1.5.2/linux-amd64-filebrowser.tar.gz
```
新建程序所在文件夹 filemanager
```
sudo mkdir filemanager
```
移动至程序目录
```
sudo mv linux-amd64-filebrowser.tar.gz filemanager/
```
cd至目录后解压
```
sudo tar -xzf linux-amd64-filebrowser.tar.gz
```
将新建的目录移动至`/home`下
```
sudo mv filemanager/ /home
```

##### 配置环境变量

编辑
```
sudo nano ~/.bashrc
```
移动至行尾后新增
```
export PATH=$PATH:/home/filemanager
```
然后`Ctrl+O`写入，`Ctrl+C`退出，之后刷新

```
source ~/.bashrc
```

新建网盘文件所在目录

```
sudo mkdir /home/dropbox
```
##### 运行

在网盘文件目录下运行`filebrowser `，在浏览器中打开 `vps的ip + 提示的端口`即可

也可以自定义端口号和网盘文件所在目录

```
filebrowser --port 3065 --scope  /home/dropbox
```



默认用户为：`admin`

默认密码为：`admin`