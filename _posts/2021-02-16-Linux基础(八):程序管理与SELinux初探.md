---
title: 【正经教程】Linux基础(八)：程序管理与SELinux初探
published: true
tags: linux
categories: Linux
---

## （一）进程的基本概念

### 进程
在Linux系统中，触发任何一个事件时，系统都会讲它定义成为一个进程，并且给予这个进程一个ID，这个ID称为PID，同时依据触发这个进程的用户与相关属性关系，给予这个PID一组有效的权限设置。

#### 【进程的产生】
“执行一个程序或者命令” 就可以触发一个事件而取得一个PID。PID的权限决定了该命令是否可以最终执行任务。
程序被用户的执行操作触发后，会加载到内存中成为一个个体，这就是进程。
- 进程有`给予执行者`的权限/属性等参数。（root和普通用户执行同一个程序，产生的进程所获取的权限可能不一样）（所以不同用户登录shell获取的PID也是不同的。）

  

|      | 定义 |
|---|---|
| 程序 | 通常为二进制程序放置在存储媒介中（如硬盘、光盘、软盘、磁带等），以物理文件的形式存在 |
| 进程 | 程序被触发后，①执行者的权限与属性、②程序的程序代码与③所需数据等都会被加载到内存中，操作系统并给予这个内存内的单元一个标识符(PID)，可以说，进程就是一个正在运行中的程序。 |

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-18 at 12.36.04.png)


#### 【子进程与父进程】

每一个子进程都有一个PID，而它的父进程则是由PPID来判断（Parent PID）。由父进程衍生出来的其他进程在一般状态下，也会沿用父进程的相关权限。

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-18 at 12.37.51.png)


#### 【Linux的过程调用: fork-and-exec】

进程都会通过父进程以复制(fork)的方式产生一个一模一样的子进程(暂存进程)，然后被复制出来的子进程再以 `exec` 的方式来执行实际要进行的进程，最终就成为一个子进程的存在。 
![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-18 at 12.43.57.png)

#### 【系统或网络服务:常驻在内存的进程】
**常驻在内存中的进程被称为 `服务` **
1. 系统本身所需的服务：crond, atd, syslog等
2. 负责网络联机的服务:  Apache, named, postfix, vsftpd 网络服务的特点在于，它会启动一个可以负责网络监听的端口(port)，以及提供外部环境的客户端（client）的连接请求。

#### 命令的后台执行 `&`
```bash
cp file1 file2 & # 加上&在末尾，这个复制命令就会放到后台去执行了。
```

## (二)工作管理
这个部分的具体命令写在 `Linux基础命令(七):工作管理与进程管理` 下面

### 终端机下工作管理

**工作管理Job Control** 指的在Bash环境下，当我们登录bash shell之后，在单一终端机下同时进行多个工作的行为管理。(只能管理当前shell的子进程)

【注意】：执行任务管理的操作中,其实每个任务都是**`目前bash的子进程`**,即彼此之间是有相关性的,我们无法用任务管理的方式由tty1的环境去管理tty2的bash

#### 前台foreground和后台background

**前台**:出现命令提示符让用户操作的环境

**后台**:其他不需要用户时刻关注的工作可以放入后台（放入后台的工作无法用`ctrl+C`来终止，也不再接受terminal和shell的输入，想要拽回前台就使用`bg/fg`）

### 脱机管理-进程交给系统后台
与前文所提到的工作管理不同，使用`&`是将命令交给终端机后台并非系统后台，一旦终端机下线or脱机，后台工作会停止运行。如果需要脱机or注销后还能让工作继续，应该使用的是脱机管理，将进程交给系统后台。(比如前面讲例行性工作的时候所用到的`at`命令。)

## (三)进程管理
### 常用signal

| 代号 | 名称    | 内容                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 1    | SIGHUP  | 启动被终止的进程,可让该PI O重新读取自己的配置文件,类似重新启动 |
| 2    | SIGINT  | 相当于用键盘输入[ctrl]-C 来中断一个进程的运行                |
| 9    | SIGKILL | 代表强制中断一个进程的执行,如果该进程执行到一半,那么尚未完成的部分可能会有[半成品]产生,类似vim会有.filename.swp保留下来 |
| 15   | SIGTERM | 以正常的方式结束进程来终止该进程。由于是正常的终止,所以后续的操作会将它完成。不过,如果该进程已经发生问题,就是无法使用正常的方法终止时,输入这个信号也是没有用的 |
| 17   | SIGSTOP | 相当于用键盘输入[ctrl]-z来暂停一个进程的运行                 |

更多信息请关注 `man7 signal`


### 进程的执行顺序---`PRI`与`Nice`值

**PRI**=priority 内核自动分配动态调整，用户无法干涉(即使是root)，`ps`命令可以查看，PRI越小，表示该进程的优先级越高，越能尽早执行

**NI**=nice 我们可以人为设置的部分 也可以用 `ps` 命令查看

PRI(new) = PRI(old) + nice

**【注意】** ： NI只是影响PRI,最终PRI还是内核自己判断，不是一定按照上面的公式严格计算。NI的范围是(-20, 19),系统还一般只允许你往大了调整。

##(四)SELinux初探

啊啊啊这个，不看了不看了，谁管这玩意啊。


