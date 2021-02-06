---
title: Linux Grub多系统开机引导程序
published: true
tags: linux
categories: Linux
---

参考资料:

[1]: https://linux.cn/article-8807-1.html	"Linux 开机引导和启动过程详解"



用`fdisk`命令删掉了所有的硬盘分区，然后`reboot`了Ubuntu虚拟机，后果就是开机出现：![Screen Shot 2021-02-02 at 18.06.19](/Users/ncc-1701-enterprise/Library/Application Support/typora-user-images/Screen Shot 2021-02-02 at 18.06.19.png)

 系统提示无法找到分区，进入`rescue mode`



