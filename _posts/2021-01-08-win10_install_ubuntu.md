---
layout: post
title: Win10安装Ubuntu子系统
date: 2021-01-08 
tags: ubuntu
---
###  介绍
在win10系统上安装Ubuntu子系统及GUI。
###  安装Ubuntu子系统
1. 以管理员身份打开PowerShell  
2. 为Linux启用win10子系统
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
3. 启用虚拟机功能
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
4. 重启计算机，打开`启用或关闭Windows功能`检查是否启用win10子系统和虚拟机功能。
5. 下载Linux内核更新程序[x64版本](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
6. 将WSL2设置为默认版本
```
wsl --set-default-version 2
```
7. 打开应用商店，安装Ubuntu。
8. 安装成功，检查版本是否为version 2。  
![](/images/resources/04fee92127d34205a892d9d25760d847.png)
### 安装GUI
1. 更新系统
```
sudo apt update && sudo apt -y upgrade
```
2. 安装xfce4
```
sudo apt-get install xrdp
sudo apt-get install xfce4
sudo apt-get install xfce4-goodies
```
3. 修改配置文件 
```
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession
```
4. 编辑`/etc/xrdp/startwm.sh`文件
```
sudo nano /etc/xrdp/startwm.sh
  # 注释下面两行
#test -x /etc/X11/Xsession && exec /etc/X11/Xsession
#exec /bin/sh /etc/X11/Xsession
  # 添加下面两行
# xfce
startxfce4
```
5. 启动xrdp
```sudo /etc/init.d/xrdp start```
### 打开window远程桌面连接
输入`localhost:3390`, 输入用户名密码即可登录。
### 常见问题
Q. 浏览器打不开出现一下错误
```
Failed to execute to default Web Browser.
```
A. 重新安装`firefox`浏览器
```sudo apt-get install firefox```


