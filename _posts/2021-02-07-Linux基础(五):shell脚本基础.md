---
title: 【正经教程】Linux基础(五)：shell脚本基础
published: true
tags: linux
categories: Linux
---

## Script脚本基本要点

###  Script脚本的执行方式

1. 直接命令执行: 直接路径，相对路径，放入$PATH变量所在路径
2. 以bash进程来执行 `bash shell.sh` 或者 `sh shell.sh`
3. 使用`source`命令执行

### 不同执行方式下对bash环境的影响

### Script脚本的语法格式要求与一些书写规范

1. 第一行必须为`#!/bin/bash`声明

2. 使用 `$variable` 或者 `${variable}`使用已声明的变量

3. 变量赋值 `var1=10` **【注意等号左右不允许空格！！】**  

4. 命令输出回传给变量 `var=$(command1)` 或者var=\`command1\`

5. 循环与条件判断格式

   ```bash
   # if的写法1  
     if command
     then
       commands
     fi
     
   # if的写法2
     if command; then
       commands
     fi
     
   # if的写法3 
     if command; then
       break
     fi
   ```
   

   ```bash
   #for的写法1
     for var in list
     do
       commands
     done
     
   #for的写法2
     for var in list; do
   
   #for的写法3 C语言风格
     for (( variable assignment ; condition ; iteration process )) #注意空格
     for (( a = 1; a < 10; a++ )) #注意这里的变量赋值可以有空格
   ```
   ```bash
   #while的格式 test-command状态码$?≠0终止循环
     while test-command
     do
       other commands
     done
   
   #until的格式 test-command状态码$?≠0执行循环
     until test-commands
     do
       other commands
     done
   ```

   

6. 养成 `exit 0` 正常中断的好习惯


## Script脚本的常见命令

### 1.读取用户输入 `read`
```bash
read -p "something say to user" var1
#之后等待用户输入 用户从键盘的输入存入变量var1
```

### 2.利用`test`进行各种测试

```bash
test [OPTION] [EXPRESSION]
参数巨多。总而言之你翻翻书吧。就这样啦。
```

## 3.利用判断符`[]`来判断

```bash
[ -z "$HOME"] ; echo $? # 查看变量是否为空 注意[]与其中内容的空格
```

