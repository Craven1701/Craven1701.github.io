---
title: 【Linux命令】其他的linux命令
published: true
tags: linux
---

#### dpkg
dpkg 是Debian package的简写，为”Debian“ 操作系统 专门开发的套件管理系统，用于软件的安装，更新和移除。
```bash
dpkg -L package # 查看与该package关联的文件 用于查看软件安装到的位置
dpkg -l package # 查看包的版本
dpkg -r package # 移除软件 保留配置
dpkg -P packagP # 移除软件 不保留配置
```
其他参数自己以后要用再补充吧

#### 一些零碎的功能

```bash
gsettings set org.gnome.desktop.screensaver lock-enabled false # 关闭自动锁屏
```



