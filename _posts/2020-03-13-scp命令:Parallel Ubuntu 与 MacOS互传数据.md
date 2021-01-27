---
title: scp命令:Parallel Ubuntu 与 MacOS互传数据
published: true
tag: linux
---

Linux上的配置过程

```bash
sudo apt isntall openssh -server
/etc/init.d/ssh start
sudo apt install net-tools
ifconfig
```
之后能够拿到linux的ip地址，记住这个ip,之后要用到。（之前看的教程说用 ip address命令也能拿到ip 但是显示的结果很奇怪= = 就换了个方式）

配置完后打开MacOS Terminal 

从Linux向MacOS传数据
```bash
scp Linux登录用户名@Linux的ip地址:源文件路径 目标存放路径
```

从MacOS向Linux传数据

```bash
scp 本地文件路径 Linux登录用户名@Linuxip地址:Linux路径
```
所有的路径都写成绝对路径