---
title: 【爬虫基础·零】:VScode+Anaconda+MacOS环境搭建
published: true
tags: python爬虫
categories: 
---

【声明】本系列教程主要参考《Python3网络爬虫开发实战·崔庆才》

出于想迅速收集同人文的目的开始学爬虫.

出于大家都说好的原因开始用VScode.

顺便复习一下之前虽然会用但是完全不知道每个包被装在哪的Anaconda.

顺便复习一下Anaconda的虚拟环境到底里面有些啥.

那么开始吧。

### 1. Anaconda虚拟环境创建

```bash
# 新建一个叫crawler的虚拟环境
conda create --name crawler python=3.6
```

![](/Users/ncc-1701-enterprise/Desktop/Screen Shot 2021-02-14 at 22.58.08.png)

根据上图信息，可以了解到，新的环境安装在`/Library/anaconda3/envs/crawler`这个目录下。

Anaconda的虚拟环境目录结构如下

```bash
cd /Library/anaconda3/envs/crawler
tree -L 1
.
├── bin #存放当前环境支持的二进制命令 启动环境后，python与pip都使用这个目录下的
├── conda-meta
├── include
├── lib # /lib/python3.6/site-packages 存放pip安装的包
├── share
└── ssl
```

### 2.VScode的环境配置

1. 先下载vscode的python插件
2. 打开工程目录下的任意一个.py文件都能自动启动该插件
3. 点击下方提示栏左端的`python 3.6`字样，可以选择自己的python解释器，在这里就选择之前新建的conda 虚拟环境。

###3.爬虫相关包的配置
主要使用pip安装了`request` `selenium`,`aiohttp`几个包等等,全部是`pip install`的安装方式

`request` 用来向浏览器发出请求，阻塞式。只有服务器响应了才会进行下一步处理

`aiohttp`是个异步Web服务库，和`request`相比不同的地方在于这个可以在等待的过程中做别的事。

`selenium`是一个自动化测试工具，我们用这个实现通过代码实现特定的浏览器动作，如点击，下拉等等。

`ctrl+shift+反点号`可以呼出VScode自带的Terminal，自动启动conda虚拟环境并切换到当前打开文件的目录下。在这使用`pip`就可以实现包安装了

###4.下载ChromeDrive工具

说实话我还没没搞清楚为啥要下载个浏览器驱动。总之先下载。

[ChromeDrive官网](https://chromedriver.chromium.org/)

官网提供给mac的安装包有两种，`chromedriver_mac64_m1.zip` 和 `chromedriver_mac64.zip`,我下载带有m1的执行报错了`Bad CPU type in executable`,不带有m1的那个可以正常用。

解压之后直接是可执行文件，将可执行文件移入文件夹`/Library/anaconda3/envs/crawler/bin` ， 在启动虚拟环境的状况下，这个路径是$PATH的第一路径。因为这个chromedrive估计我也只有用爬虫的时候才会用到，放到`crawler`这个环境下一起比较好管理吧。

```bash
#查看当前的环境变量$PATH
echo $PATH | sed -n 's/:/\n/go'
/Library/anaconda3/envs/crawler/bin #如果虚拟环境已经启动，这个路径会在首位
/Library/anaconda3/condabin
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin

mv /Downloads/chromedriver /Library/anaconda3/envs/crawler/bin

#如果mv命令执行成功，那么使用type可以找到chromedrive的执行路径已经进入了/crawler/bin
type chromedriver
chromedriver is /Library/anaconda3/envs/crawler/bin/chromedriver
```

跑一下这个测试一下，如果能弹出空白的网页，那就配置OK，可以准备去网上爬东西啦~

```python
from selenium import webdriver
browser = webdriver.Chrome()
```

（**PS**: 火狐的浏览器驱动是叫做geckodriver。我懒得下了）

### 5.PhantomJS的安装

“PhantomJS是一个无界面的、可脚本编程的WebKit浏览器引擎,它原生支持多种Web标准:DOM操作、css选择器、JSON、Canvas以及SVG。Selenium支持PhantomJS,这样在运行的时候就不会再弹出一个浏览器了。”

嗯，所以再装一次PhantomJS,同样把解压出来的可执行文件放到/crawler/bin目录下面

**【相关链接】**
官方网站:http://phantomjs.org
官方文梢:http://phantomjs.org/quick-start.html
下载地址:http://phantomjs.org/download.html
API接门说明:http://phantomjs.org/api/command-line.html

### 6.汇总安装的工具

看到后面发现真的安装了一堆东西，干脆写一起好了

（相关依赖pip自己管理着，不列出来了）

| 来自pip的相关包           | 功能                                                         |
| ------------------------- | ------------------------------------------------------------ |
| requests          2.25.1  | 向服务器发起请求                                             |
| selenium          3.141.0 | 自动规划测试工具                                             |
| aiohttp           3.7.3   | 异步请求                                                     |
| aiodns            2.0.0   | 加速DNS解析                                                  |
| cchardet          2.1.7   | 字符编码检测库                                               |
|lxml-4.6.2|Python的一个解析库,支持HTML和XML的解析,支持XPath解析方式,|
|beautifulsoup4-4.9.3|BeautifulSoup是Python的一个H T M L或XML的解析库,我们可以用它来方便地从网页中提取数据。|
|pyquery-1.4.3|pyquery同样是一个强大的网页解析工具,它提供了和jQuery类似的语法来解析HTML文梢,支持css选择器,|
|||



| 官网安装     | 功能                                                  |
| ------------ | ----------------------------------------------------- |
| ChromeDriver | Chrome的浏览器驱动 我还不知道有啥用                   |
| PhantomJS    | PhantomJS是一个无界面的、可脚本编程的WebKit浏览器引擎 |
|              |                                                       |
|              |                                                       |
|              |                                                       |


我的Homebrew默认包管理路径在`/usr/local/Cellar`
| Homebrew管理 | 功能                                                         |
| ------------ | ------------------------------------------------------------ |
| imagemagick  |                                                              |
| tesseract    | OCR，是tesserocr的核心，tesserocr只是这玩意做了python API封装 |
|              |                                                              |
|              |                                                              |
|              |                                                              |

