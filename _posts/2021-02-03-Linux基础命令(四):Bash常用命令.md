---
title: 【Linux命令】Linux基础命令(四)：Bash常用命令
published: true
tags: linux
categories: Linux

---

### 1.`type`

`type`只找出执行文件

```bash
type [-tpa] name
# 输出含义
file # 外部命令
alias # 该命令为命令的别名
builtin # 为bash内置的命令

# 参数
-t
-p #显示完整路径
-a #显示在PATH变量定义的路径中，显示所有的命令
```

### 2.`echo` 打印变量

```bash
echo $variable # 显示变量 '$'表示后面所接的是变量名
#应用实例
echo $PATH
echo $HOME
echo $MAIL
```

【有关变量的其他几点提示】

1. 使用**"$变量名称"** 或者 **${变量}**累加内容 如 `PATH="$PATH":/home/bin`或者`PATH=S{PATH}:/home/bin`
2. 若需要该变量在其他子进程执行，需要**export**来使变量变成环境变量 如`export PATH`
3. **`** 反单引号，被两个反单引号所包括的命令会优先执行

### 3.`unset` 取消变量

```bash
unset variable
```

### 4.`set` 显示所有变量

### 5.`export` 将自定义变量转化为环境变量

```bash
export variable
```

### 6.`locale` 查询系统支持的语言

```bash
locale -a
```

相关信息存放于`/usr/lib/locale`

### 7.`read` 读取来自键盘输入的变量

```bash
read [-pt] variable
-p # 后接提示符
-t # 后接等待的秒数
#你的键盘输入会被存储到变量里 可以用echo $variable查看
```

### 8.`array`

```bash
declare -a var
var[index]=content
echo ${var[index]}
```

### 9.`declare` &`typeset` 声明变量类型

```bash
declare [-aixr] variable
#默认变量类型是字符串 
#参数说明
-a # 将后面名为variable的变量定义为 array类型
-i # integer ps:bash不支持浮点数运算的
-x # 声明为环境变量 功能同export
-r # readonly
```

### 10.`ulimit` 设置文件系统与程序的限制关系

```bash
xxxxxxxxxx ulimit [-SHacdfltu] [配额]
```

感觉我目前还用不上。

### 11.`alias` 设置命令别名

```bash
alias lm='ls -l | more'
alias rm='rm -i'
alias # 不加任何东西会显示目前已有的alias
```

### 12.`unalias` 取消别名设置

```bash
unalias 别名
```

### 13.`history`

```bash
history [n] #后接数字 显示最近n条命令
history [-c]
history [-raw] histfiles
-w  # 当前记录立刻写入~/.bash_history中

#配合一下操作使用
!number #执行第number条
!command #模糊的command也可以
!! #执行上一条
```

### 14.`stty` 终端机的环境设置

```bash
stty - change and print terminal line settings
stty [-a]

#一般挺少要配置这玩意的
```

### 15.`set` Bash自己的终端机设置值 

```bash
set [OPTIONS]
-u
-v
-x
-h
-H
-m
-B
-C

echo $- #查看当前set的设置情况
```

## 数据重定向

1. 标准输入(stdin) 0 <;<< `0<`与`0<<`     数字与符号中间无空格
2. 标准输出(stdout) 1 >;>> `1>`与`1>>`    数字与符号中间无空格
3. 标准错误输出(stderr) 2>; 2>>

### 16.`>`数据重定向

### 17.`|` 管道界定符与管道命令

**管道命令**，指可以接受 standard input 数据的命令。

- 管道命令仅处理standard output, 对于standard error output会给予忽略
- 管道命令必须要能够接受来自前一个命令的数据作为 standard input 继续处理

```bash
ls -al /etc | less
```

## 管道命令之选取命令

### 18.`cut`

主要用途在于将同一行里面的数据进行分解，最常使用在分析一些数据或者文字数据的时候。比如分析log文件

```bash
cut -d '分隔字符' -f fields # 用于分隔字符
cut -c 字符范围 #用于排列整齐信息
-d # 后接分隔字符 与-f一起使用；
-f # 依据-d的分隔字符将一段信息切割成为数段 
-c # 以字符的单位取出固定字符区间

# 举例说明 
echo $PATH | cut -d ':' -f 5
echo $PATH | cut -d ':' -f 3,5
export | cut -c 12-
```

### 19.`grep`

```bash
grep [-acinv] [--color=auto] '查找字符串' filename
#参数说明
-a # 将binary文件以text文件的方式查找数据
-c # 计算找到'查找字符串'的次数
-i # 忽略大小写差异
-n # 顺便输出行号
-v # 反向选择 即输出不符合查找模式的行
--colort=auto #查找的关键字突出颜色显示
# 显然 查找字符串的模式当然是支持正则表达式的
```

我感觉sed也可以做这个啊。grep有什么好处吗。虽然grep写起来写法更复杂？

以及，说明书上说`GREP_COLORS`的优先级比`GREP_COLOR`更高，结果我写进~/.bashrc 的时候，居然只有后者能被识别？？？？

## 排序命令

### 20.`sort`

```bash
sort [OPTION] ... [FILE] ...
#参数说明
-f # 忽略大小写差异
-b # 忽略最前面的空格符号部分
-M # 以月份的名字来排序
-n # 使用以 纯数字 来排序
-r # 反向排序
-u # 就是uniq 相同的数据中 仅仅出现一行代表
-t # 分隔符 默认是使用[Tab]来分隔
-k # 以field 来进行排序的意思
```

### 21.`wc` 单词计数器 估算信息量

```bash
wc [-lwm]
#参数
-l # 仅列出行
-w # 仅列出多少字
-m # 多少字符

cat /etc/passwd | wc -l # 统计系统里有多少账户
```

### 22. `uniq` 重复数据仅仅显示一次

```bash
uniq [-ic]
# 参数
-i : 忽略大小写字符的不同
-c : 进行计数

last | cut -d ' ' -f1 | sort | uniq -c
```

### 23.`tee` 双重重定向

该命令会同时将数据流送往文件与屏幕（stdout）,stdout可以让下一个命令继续处理

```bash
tee [-a] file
#参数
-a # 以累加的方式将数据加入file文件中
```

## 字符转换命令

### 24.`tr` 信息删除与替换

```bash
tr [-ds] SET1
# 参数
-d # 删除信息中的SET1这个字符串
-s # 替换掉重复的字符

该命令可以直接写入正则表达式
```

### 25.`col`

```bash
col [-xb]
#参数
-x # 将tab键转换成对等的空格键
-b # 在文字内有反斜杠/时，仅保留/最后接的那个字符
```

### 26.`join`

```bash

```

### 27.`paste` 两行贴在一起  以[tab]键隔开

### 28.`expand` [tab]转空格

```bash
expand [-t] file
```

### 29.`split` 	切割命令 将大文件切割成小文件

嗯，一般会`split`切完再用数据重定向整合起来吧。

```bash
split [-bl] file PREFIX
# 参数说明
-b # 后面可以接欲切割成的文件大小 支持单位 b k m
-l # 以行数来进行切割
PREFIX # 前导符号 可作为切割文件的前导文字 
# 生成的小文件都是以 PREFIX*命名

cd /tmp; split -b 300k /etc/termcap termcap
cat termcap* >> termback #数据流重定向整合文件
```

### 30.`xargs`  参数替换

用于产生某个命令的参数。xarg可以读入stdin的数据，并且以空格符或者断行字符进行分辨，将stdin的数据分隔成为arguments

在很多命令不支持管道的情况下（即不接受stdin作为输入），可以使用xargs转换为该命令的参数， 很好用的东西

```bash
xargs [-0epn] command
#参数说明
-0 # 如果输入的stdin含有特殊字符 如` \ 等，这个参数可以将其还原成一般字符。
-e # End Of File xargs收到指定字符串时停止工作 
-p # 在执行每个命令的参数时，询问用户的意思
-n # 后面接次数 每次command命令执行时，需要使用几个参数

# 当xargs后面没有接到任何命令时 默认是使用echo来进行输出

#实际应用
cut -d ':' -f 1  /etc/passwd | head -n 3 | xargs finger
#xargs命令将 head -n 3 所产生的，通过|传来的stdin 转化为了finger命令能识别的参数

cut -d ':' -f 1 /etc/passwd | xargs -p -n 5 finger
#对所有的用户执行finger命令 但是一次只执行5个 每次都询问
```

### 31.`-` 减号的用途

管道命令中 经常会使用到前一个命令的stdout作为这次的stdin, 某些命令需要用到文件名（如tar）来进行处理，该stdin和stdout可以利用`-` 来代替

```bash
tar -vcf - /home | tar -xvf -
```

……很好 虽然我没看懂。hhhh

