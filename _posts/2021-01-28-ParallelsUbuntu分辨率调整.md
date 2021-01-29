---
title: ParallelsUbuntu分辨率调整
published: true
tags: linux
---

在Parallels虚拟机上安装了ubuntu kylin 16.04 LTS系统，刚开始安装成功的时候，分辨率只有800*600。去系统设置里面改动，可是设置里面只有800*600一个选项。

我的解决方法：

（1）打开终端，输入xrandr，只有800*600

（2）在终端输入sudo xdiagnose，输入密码，弹出xdiagnose设置框，将debug下的三个选项全部勾选，然后选择“应用”。完成后关闭xdiagnose设置框。

3）输入reboot重启虚拟机。分辨率已经修改成功。可在“系统设置”——>"显示"中选择合适的分辨率。在终端输入xrandr也会看到不再只有800*600