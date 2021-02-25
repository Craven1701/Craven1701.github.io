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

（**PS**: 火狐的浏览器驱动是叫做**geckodriver**。我懒得下了）

### 5.PhantomJS的安装

“PhantomJS是一个无界面的、可脚本编程的WebKit浏览器引擎,它原生支持多种Web标准:DOM操作、css选择器、JSON、Canvas以及SVG。Selenium支持PhantomJS,这样在运行的时候就不会再弹出一个浏览器了。”

嗯，所以再装一次PhantomJS,同样把解压出来的可执行文件放到/crawler/bin目录下面

**【相关链接】**
官方网站:http://phantomjs.org
官方文梢:http://phantomjs.org/quick-start.html
下载地址:http://phantomjs.org/download.html
API接门说明:http://phantomjs.org/api/command-line.html

# 汇总安装的工具

## 网站的请求与解析

看到后面发现真的安装了一堆东西，干脆写一起好了

（相关依赖pip自己管理着，不列出来了）

| 请求库(pip3管理)           | 功能                                                         |
| ------------------------- | ------------------------------------------------------------ |
| requests-2.25.1  | 向服务器发起请求                                             |
| selenium-3.141.0 | 自动规划测试工具                                             |
| aiohttp -3.7.3   | 异步请求                                                     |
| aiodns-2.0.0   | 加速DNS解析                                                  |
| cchardet- 2.1.7   | 字符编码检测库                                               |



| 解析库(pip3管理)           | 功能                                                         |
| ------------------------- | ------------------------------------------------------------ |
|lxml-4.6.2|Python的一个解析库,支持HTML和XML的解析,支持XPath解析方式,|
|beautifulsoup4-4.9.3|BeautifulSoup是Python的一个HTML或XML的解析库,我们可以用它来方便地从网页中提取数据。|
|pyquery-1.4.3|pyquery同样是一个强大的网页解析工具,它提供了和jQuery类似的语法来解析HTML文梢,支持css选择器,|
|tesserocr-2.5.1|文字识别呗|



| 官网安装     | 功能                                                  |
| ------------ | ----------------------------------------------------- |
| ChromeDriver | Chrome的浏览器驱动 我还不知道有啥用                   |
| PhantomJS    | PhantomJS是一个无界面的、可脚本编程的WebKit浏览器引擎 |



我的Homebrew默认包管理路径在`/usr/local/Cellar`,里面的二进制文件会自动复制一份到`/usr/local/bin/` ，保证Terminal能在`$PATH`下找到命令 
| Homebrew管理 | 功能                                                         |
| ------------ | ------------------------------------------------------------ |
| imagemagick  |                                                              |
| tesseract    | OCR，是tesserocr的核心，tesserocr只是这玩意做了python API封装 (这玩意要扩增语言包的，我还没装，不过先这样) |



## 数据库与存储库

| 数据库名称                          | 安装以及服务启动                                             |
| ----------------------------------- | ------------------------------------------------------------ |
| MySQL                               | `/usr/local/mysql-5.7.27-macos10.14-x86_64` 数据目录         |
| MongoDB v4.0.3 (可视化工具 Robo 3T) | `brew services start mongodb`<br />`/usr/local/var/log/mongodb` 日志文件<br />`/usr/local/Cellar/mongodb/4.0.3_1/.bottle/etc/mongod.conf`  配置文件<br /> `mongod --config /usr/local/Cellar/mongodb/4.0.3_1/.bottle/etc/mongod.conf`启动服务 |
| Redis(brew install )                | `/usr/local/etc/redis.conf` 配置文件地址 <br />`brew services start/stop/restart redis` 服务启动/停止/重启 |



| 数据库的存储库(pip3管理)    | 功能                                                         |
| --------------------------- | ------------------------------------------------------------ |
| pymysql-1.0.2               | 在python中提供MySQL数据库的交互接口                          |
| pymongo-3.11.3 （这个好大） | 在python中提供MongoDB数据库的交互接口                        |
| redis-3.5.3                 | 在python中提供Redis数据库的交互接口                          |
| RedisDump（gem install）    | R e d i s D u m p 是一个用于R e d i s 数据导人/ 导出的工具,是基于R u b y 实现的,所以要安装R e d i s D u m p,需要先安装R u b y(我电脑上已经有Ruby了) |
因为gem是第一次用 记一下bash的输出
```bash
sudo gem install redis-dump #因为要写/Library/Ruby/Gems/2.6.0 目录，所以要添加sudo权限
Fetching redis-dump-0.4.0.gem
Fetching drydock-0.6.9.gem
Fetching redis-4.2.5.gem
Fetching uri-redis-0.4.2.gem
Fetching yajl-ruby-1.4.1.gem
Successfully installed drydock-0.6.9
Successfully installed uri-redis-0.4.2
Successfully installed redis-4.2.5
Building native extensions. This could take a while...
Successfully installed yajl-ruby-1.4.1
Successfully installed redis-dump-0.4.0
Parsing documentation for drydock-0.6.9
Installing ri documentation for drydock-0.6.9
Parsing documentation for uri-redis-0.4.2
Installing ri documentation for uri-redis-0.4.2
Parsing documentation for redis-4.2.5
Installing ri documentation for redis-4.2.5
Parsing documentation for yajl-ruby-1.4.1
Installing ri documentation for yajl-ruby-1.4.1
Parsing documentation for redis-dump-0.4.0
Installing ri documentation for redis-dump-0.4.0
Done installing documentation for drydock, uri-redis, redis, yajl-ruby, redis-dump after 1 seconds
5 gems installed
```
.....我咋没看出来往哪个目录装了。

## Web库与APP爬取库的安装

| Web服务程序库(框架?)(pip3管理) |      |
| ------------------------------ | ---- |
| flask-1.1.2                    |      |
| tornado-6.1                    |      |


看来我以前用的django也是web服务框架之一。这东西运行起来就是在本地端口开一个web服务。



| App爬取库(pip3)管理 |                                                              |
| ------------------- | ------------------------------------------------------------ |
|**暂未安装**|Charles是一个网络抓包工具,相比Fiddler,其功能更为强大,而且跨平台支持得更好,所以这里选用它来作为主要的移动端抓包工具。|
|mitmproxy         5.3.0(这个玩意真的好大啊。艹 超级大。一堆依赖安装好久)|mitmproxy是一个支持HTTP和HTTPS的抓包程序,类似Fiddler、Charles的功能,只不过它通过控制台的形式操作。此外,mitmproxy还有两个关联组件,一个是mitmdump,它是mitmproxy的命令行接口,利用它可以对接Python脚本,实现监听后的处理;另一个是mitmweb,它是一个Web程序,通过它以沾楚地观察到mitmproxy捕获的请求。|
|**暂未安装**|Appium是移动端的自动化测试工具,类似于前面所说的Selenium,利用它可以驱动Android、iOS等设备完成向动化测试,比如模拟点击、滑动、输入等操作,其官方网站为:http://appium.io/。本节我们就来了解一下Appium的安装方式。|

## 爬虫框架的安装

| 爬虫框架管理(pip3)       |                                                              |
| ------------------------ | ------------------------------------------------------------ |
|pyspider0.3.10|pyspider是国人binux编写的强大的网络爬虫框架,它带有强大的WebUI、脚本编辑器、任务监控器、项目管理器以及结果处理器,同时支持多种数据库后端、多种消息队列,另外还支持JavaScript渲染页面的爬取,使用起来非常方便|
|Scrapy2.4.1|Scrapy是一个十分强大的爬虫框架,依赖的库比较多,至少需要依赖的库有Twisted14.0、lxml3.4和pyOpenSSL0.14。|
|scrapy-splash-0.7.2|Scrapy-Splash是一个Scrapy中支持JavaScript渲染的工具,Scrapy-Splash的安装分为两部分。一个是Splash服务的安装,具体是通过Docker,安装之后,会启动一个Splash服务,我们可以通过它的接口来实现JavaScript页面的加载。另外一个是Scrapy-Splash的Python库的安装,安装之后即可在Scrapy中使用Splash服务|
| scrapy-redis-0.6.8 | Scrapy-Redis是Scrapy的分布式扩展模块,有了它,我们就可以方便地实现Scrapy分布式爬虫的搭建。 |

###【有关Docker的说明与安装】

**PS** : Splash服务的安装所需要的[Docker](https://www.docker.com/resources/what-container)是一个软件容器引擎。引用官网的说明如下：

```markdown
A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Container images become containers at runtime and in the case of Docker containers - images become containers when they run on Docker Engine. Available for both Linux and Windows-based applications, containerized software will always run the same, regardless of the infrastructure. 

Containers isolate software from its environment and ensure that it works uniformly despite differences for instance between development and staging
```

而教材上的说明是中文看的更通俗易懂一点

```
Docker是一种容器技术,可以将应用和环境等进行打包,形成一个独立的、类似于iOS的App形式的“应用”。这个应用可以直接被分发到任意一个支持Docker的环境中,通过简单的命令即可启动运行。Docker是一种最流行的容器化实现方案,和虚拟化技术类似,它极大地方便了应用服务的部署;又与虚拟化技术不同,它以一种更轻量的方式实现了应用服务的打包。使用Docker,可以让每个应用彼此相互隔离,在同一台机器上同时运行多个应用,不过它们彼此之间共享同一个操作系统。Docker的优势在于,它可以在更细的粒度上进行资源管理,也比虚拟化技术更加节约资源。
```

粗略的看了一下是个可以让你免去环境配置，下载即用的方便东西？我没用过这个，所以要先安装`docker` ，查了一下可以使用homebrew来安装，但是与普通的`brew install`有点差异,要使用`homebrew cask`。

二者的区别在与前者只能装命令行工具，后者能装图形化工具

详细的区别请查看: [Docker on Mac with Homebrew: A Step-by-Step Tutorial](https://www.cprime.com/resources/blog/docker-on-mac-with-homebrew-a-step-by-step-tutorial/)

具体命令如下:

```bash
brew cask install docker
```

因为第一次用这个，所以输出也记录一下好了。

## 部署相关库的安装

| 部署相关库                   | 功能                                                         |
| ---------------------------- | ------------------------------------------------------------ |
|Docker(由homebrewcask安装)|Docker是一种容器技术,可以将应用和环境等进行打包,形成一个独立的、类似于iOS的App形式的“应用”。这个应用可以直接被分发到任意一个支持Docker的环境中,通过简单的命令即可启动运行。|
|scrapyd-1.2.1(pip3install)|Scrapyd是一个用于部署和l运行Scrapy项目的工具,有了它,你可以将写好的Scrapy项目上传到云主机并通过API来控制它的运行。|
| scrapyd-client-1.1.0(pip3install) | 在将S c r a p y 代码部署到远程S c r a p y d的时候,第一步就是要将代码打包为E GG文件,其次需要将E G G文件上传到远程主机。这个过程如果用程序来实现,也是完全可以的,但是我们并不需要做这些t作,因为S c r a p y d - C l i e n t 已经为我们实现了这些功能。 |
| python-scrapyd-api-2.1.2(pip3install) | 是scrapyd对python的接口，之后可以通过python来获取主机状态。 |
| scrapyrt-0.11.0 | S c r a p y r t 为S c r a p y 提供了一个调度的H T T P接口,有了它,我们就不需要再执行S c r a p y 命令而是通过请求一个H T T P接口来调度S c r a p y 任务了。S c r a p y r t 比S c r a p y d 更轻量,如果不需要分布式多任务的话,可以简单使用S c r a p y r t 实现远程S c r a p y 任务的调度。 |
| gerapy-0.9.6 | G e r a p y 是一个S c r a p y 分布式管理模块, |

**【有关scrapyd】**安装完毕之后,需要新建一个配置文件/etc/scrapyd/scrapyd.conf,Scrapyd在运行的时候会读取此配置文件。内容我都是抄书。具体配置了啥再说吧。

**【有关scrapyrt的报错】**安装完之后运行报错了，因为我没Scrapy项目中运行的原因，具体参考[这里](https://blog.csdn.net/zimojiang/article/details/103882571)

```
RuntimeError: Cannot find scrapy.cfg file
```



#### **吐槽一下**：

我这一下午，代码一个字没写上，装各种不知道啥用的包装了这么多。说实话有点劝退啊。好累。Docker还没装(网不好)，数据库也还没配置好，但是差不多该装的都装完了。边做边写博客做笔记有点慢，但是感觉清晰挺多，继续吧。

啥时候才能实现爬取同人文的伟大事业哟。