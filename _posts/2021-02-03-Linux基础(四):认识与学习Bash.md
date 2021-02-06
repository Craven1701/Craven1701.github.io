---
title: 【正经教程】Linux基础(四)：认识与学习Bash
published: true
tags: linux
categories: Linux
---

## 1. Shell

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-03 at 20.41.55.png)

简而言之，shell是用户与Kernel交互的界面。shell是用户使用操作系统来操作硬件的一个接口。shell可以调用其他的软件，比如各种各样的命令，来调用内核运行所需的工作。

狭义上是仅仅只命令行，广义上的shell也包括图形操作界面。

我在mac和linux上用的shell都是bash.（Bourne Again SHell）

目录`/etc/shells`可以用来查看当前系统中有多少可以使用的shell

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-03 at 20.50.51.png)

`/home/username/.bash_history` 目录中存放有上一次登录所使用的命令

## 2. 环境变量

- 习惯性的，环境变量要求全部大写

- `env`命令可以查看当前所有的【环境变量】

- `set`命令用于查看当前所有【变量】

- 环境变量与自定义变量的区别在于，后者不可以被子进程所继续引用，而环境变量可以。

- `export`命令可以将自定义变量转化为环境变量

  

**`PS1`** 提示符设置，这玩意可以改变命令提示符的样式

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-03 at 22.11.13.png)

## 3. Bash Shell的操作环境

### a.路径与命令查找顺序

1. 以相对/绝对路径执行命令，如"/bin/ls"或者"./ls"
2. 由alias找到该命令来执行
3. 由bash内置的(builtin)命令来执行
4. 通过$PATH这个变量的顺序找到的第一个命令来执行
![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-04 at 12.51.43.png)

### b.Bash的欢迎信息文件

`/etc/issue` 文件内写了登录终端机接口tty时的欢迎信息，你可以在里面画图之类的。

`/etc/issue.net` 文件内写了远程登录终端机时的欢迎信息

`/etc/motd` 是让所有人都知道的信息

#### **【如何登录tty】**

- 按下Ctrl + Alt + **Fn1**进入*图形化用户登录界面*
- 按下Ctrl + Alt + **Fn2**进入*当前图形化界面*
- 按下Ctrl + Alt + **Fn3-Fn6**进入*命令行虚拟终端*
- 按下Ctrl + Alt + **Fn7-Fn12**进入*另外的虚拟终端，这些虚拟终端没有任何程序执行，所以只能看到一个闪烁的光标*

|      | issue内的各代码意义                |
| ---- | ---------------------------------- |
| \d   | 本地端时间的日期                   |
| \l   | 显示第几个终端机接口               |
| \m   | 显示硬件的等级                     |
| \n   | 显示主机网络名称                   |
| \o   | 显示domain name                    |
| \r   | 显示操作系统版本（相当于uname -r） |
| \t   | 显示本地端时间的时间               |
| \s   | 操作系统的名称                     |
| \v   | 操作系统的版本                     |



### c.Bash的环境配置文件

#### ①. login shell 与 non-login shell

**login shell** 取得bash时需要完整的登录流程，称为login shell, 如tty2~tty6

**non-login shell** 取得bash接口时不需要完整的登录流程，如tty1是从图形界面进入terminal，不需要再次登录，属于non-login shell

#### ②. Login shell的配置文件读取


| 文件                                             |                                                              |
| ------------------------------------------------ | ------------------------------------------------------------ |
| /etc/profile                                     | 内写有系统的整体设置，最好不要修改。                         |
| ~/.bash_profile 或者~/.bash_login或者 ~/.profile | 用户个人设置 想改动的话改动这里吧。左边三个文件只会读取其中一个，按照优先级顺序为.bash_profile→.bash_login→.profile |

**/etc/profile**文件内容

```bash
# /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
# and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).

if [ "${PS1-}" ]; then #判断命令提示符的设定
  if [ "${BASH-}" ] && [ "$BASH" != "/bin/sh" ]; then
    # The file bash.bashrc already sets the default PS1.
    # PS1='\h:\w\$ '
    if [ -f /etc/bash.bashrc ]; then
      . /etc/bash.bashrc
    fi
  else
    if [ "`id -u`" -eq 0 ]; then # `id -u`拿到当前用户的uid,判断是否为root uid=0(root) 设定命令提示符
      PS1='# '
    else
      PS1='$ '
    fi
  fi
fi

if [ -d /etc/profile.d ]; then
  for i in /etc/profile.d/*.sh; do #对用户有r权限的.sh文件进行调用
    if [ -r $i ]; then
      . $i
    fi
  done
  unset i
fi
# /etc/profile.d/*.sh中设置了颜色，语系，别名等等
```
**~/.profile**文件内容

```bash
# ~/.profile: executed by the command interpreter for login shells.
# This file is not read by bash(1), if ~/.bash_profile or ~/.bash_login
# exists.
# see /usr/share/doc/bash/examples/startup-files for examples.
# the files are located in the bash-doc package.

# the default umask is set in /etc/profile; for setting the umask
# for ssh logins, install and configure the libpam-umask package.
#umask 022

# if running bash
if [ -n "$BASH_VERSION" ]; then
    # include .bashrc if it exists
    if [ -f "$HOME/.bashrc" ]; then
	. "$HOME/.bashrc" # 这个文件里写了非常多自己bash的配置 .是source命令的简写，如果对这个文件进行了修改 可以直接source ~/.bashrc读取新环境
    fi
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH" # 修改环境变量PATH
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH" #修改环境变量PATH
fi
```

**login shell的启动流程图**

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-04 at 14.55.55.png)

#### ③.non-login shell的配置文件读取

仅仅读取**`~/.bashrc`**文件而已

#### ④. 其他相关文件

1. `~/.bash_history` 记录历史命令
2. `~/.bash_logout`注销bash系统后帮我做完什么操作再离开

## 3.Bash的通配符与特殊符号以及组合按键

- **bash默认组合按键**

| 组合按键 | 执行结果                   |
| -------- | -------------------------- |
| Ctrl + C | 终止目前的命令             |
| Ctrl + D | 输入结束(EOF)              |
| Ctrl + M | 就是Enter                  |
| Ctrl + S | 暂停屏幕输出               |
| Ctrl + Q | 恢复屏幕输出               |
| Ctrl + U | 在提示符下，将整行命令删除 |
| Ctrl + Z | 暂停目前的命令             |



- 通配符

| 符号 | 意义                                                         |
| ---- | ------------------------------------------------------------ |
| *    | 代表0到无穷多个任意字符                                      |
| ?    | 代表一定有一个任意字符                                       |
| []   | 同样代表一定有一个在中括号内的字符（非任意字符） 如[abcd]表示有abcd其中某一个 |
| [-]  | 表示在编码顺序内的所有字符 [0-9]                             |
| [^]  | 中括号中的第一个字符为^ 表示原向选择 [ ^abc] 表示非a,b,c的任意字符 |



- 特殊符号

| 符号 | 内容                                        |
| ---- | ------------------------------------------- |
| #    | 脚本注释                                    |
| \    | 转义符                                      |
| \|   | 管道 分隔两个管道命令的界定                 |
| ;    | 连续执行命令分隔符 连续命令的界定           |
| ~    | 用户主文件夹                                |
| $    | 变量前导符  ` $?` 命令回传码                |
| &    | 作业控制（job control）将命令变成背景下工作 |
| !    | 逻辑运算 非                                 |
| /    | 目录符号 路径分隔                           |
| >,>> | 输出重定向 "替换" "累积"                    |
| <,<< | 输入重定向 ""    <<表示输入结束             |
| ''   | 单引号 不具有变量置换功能                   |
| ""   | 双引号 具有变量置换功能                     |
| ``   | 被反引号包住的命令可以先执行                |
| ()   | 子shell的起始与结束                         |
| {}   | 命令块的组合                                |

  

- 命令执行的判断

| 语句          | 说明                                 |
| ------------- | ------------------------------------ |
| cmd;cmd       | 不考虑命令系相关性的连续执行         |
| cmd1 && cmd2  | 若cmd1执行完毕且$?=0, 则开始执行cmd2 |
|               | 若cmd1执行完毕且$?≠0,则cmd2不执行    |
| cm1 \|\| cmd2 | 若cmd1执行完毕且$?=0,则不执行cmd2    |
|               | 若cmd1执行完毕且$?≠0,则执行cmd2      |

