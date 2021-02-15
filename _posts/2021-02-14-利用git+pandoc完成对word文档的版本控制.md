---
title: 利用git+pandoc完成对word文档的版本控制
published: true
tags: git
---



搞这个事情的初衷是因为尝到了git甜头想要搞点事情而已。

因为.doc是个二进制文件，git只能显示纯文本文档的修改变化，所以带上`pandoc`作一下格式转换。

**参考资料：**
[https://blog.csdn.net/GAI159/article/details/105025312/](https://blog.csdn.net/GAI159/article/details/105025312/)
[https://www.cnblogs.com/yezuhui/p/6853271.html](https://www.cnblogs.com/yezuhui/p/6853271.html)
[https://github.com/vigente/gerardus/wiki/Integrate-git-diffs-with-word-docx-files](https://github.com/vigente/gerardus/wiki/Integrate-git-diffs-with-word-docx-files)
[Pandoc的安装与使用](https://www.jianshu.com/p/6ba04f669d0b)

## Pandoc简介
本部分内容参考
 [Pandoc的安装与使用](https://www.jianshu.com/p/6ba04f669d0b)
作者：grug350   			来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处
						
官网：[Pandoc](https://www.pandoc.org/)

Pandoc是个好用的文本格式转化器。它可以将文档在 Markdown、LaTeX、reStructuredText、HTML、Word docx 等多种标记格式之间相互转换，并支持输出 PDF、EPUB、HTML 幻灯片等多种格式。该程序被称为格式转换界的 “瑞士军刀”。这里用来把word转成markdown，好让我们能在git的管理下看见详细的版本控制信息。

###1. 安装
安装参考官网就好，支持Windows,macOS,Linux。Linux蛮多发行版自带这个，比如Ubuntu，或则说如果安装过Anacoda,那么pandoc也会同时装入电脑中。

我的电脑里装了Anacoda,所以直接拿来用就好了。

###2.pandoc基本使用
```bash
pandoc [OPTIONS] [FILES]

#基本参数说明
其中 <files> 为输入的内容，其输入即可以来自文件，也可以来自标准输入甚至网页链接。而 <options> 为参数选项。主要的参数选项有：

-f <format>、-r <format> # 指定输入文件格式，默认为 Markdown；
-t <format>、-w <format> # 指定输出文件格式，默认为 HTML；
注意：上述两项pandoc可以根据文件拓展名自己识别，不输入也行
-o <file> # 指定输出文件，该项缺省时，将输出到标准输出；
--highlight-style <style> # 设置代码高亮主题，默认为 pygments；
-s # 生成有头尾的独立文件（HTML，LaTeX，TEI 或 RTF）；
-S # 聪明模式，根据文件判断其格式；
--self-contained # 生成自包含的文件，仅在输出 HTML 文档时有效；
--verbose # 开启 Verbose 模式，用于 Debug；
--list-input-formats # 列出支持的输入格式；
 # 列出支持的输出格式；
--list-extensions # 列出支持的 Markdown 扩展方案；
--list-highlight-languages # 列出支持代码高亮的编程语言；
--list-highlight-styles # 列出支持的代码高亮主题；
-v、--version # 显示程序的版本号；
-h、--help # 显示程序的帮助信息。

#使用示范
pandoc input.docx -o output.md #把我的word转成markdown
pandoc input.md -o output.docx
```

##实现对word文档的版本控制
参考教程地址：[https://github.com/vigente/gerardus/wiki/Integrate-git-diffs-with-word-docx-files](https://github.com/vigente/gerardus/wiki/Integrate-git-diffs-with-word-docx-files)

This section was inspired by Martin Fenner's "Using Microsoft Word with git".

To configure git diff:

Install pandoc.

Tell git how to handle diffs of .docx files.

Create or edit file ~/.gitconfig (linux, Mac) or "c:\Documents and Settings\user.gitconfig" (Windows) to add
```bash
 [diff "pandoc"]
   textconv=pandoc --to=markdown
   prompt = false
 [alias]
   wdiff = diff --word-diff=color --unified=1
```
In your paper directory, create or edit file` .gitattributes` (linux, Windows and Mac) to add

```bash
*.docx diff=pandoc
```
You can commit .gitattributes so that it stays with your paper for use in other computers, but you'll need to edit` ~/.gitconfig` in every new computer you want to use.

========================================================================================================================================

### 【**.gitattributes**】

稍微说明一下`.gitatrributes`这个文件。官网链接：[gitattributes documents](https://git-scm.com/docs/gitattributes) 
这个文件是一个简单的文本文件，作用是为`attributes`提供一个路径名

文件的基本格式
```bash
pattern attr1 attr2 ...
```
是一个pattern跟随一串的attrs, 这个pattern我理解的是某种匹配模式，然后会找到符合这种匹配模式的文件，用后面的attrs对这个文件的操作进行规范。
上文中的 `*.docx diff=pandoc` 表示对于所有.docx的文件，在使用diff时，默认是使用[diff "pandoc"] 这个subsection里面的配置。

========================================================================================================================================

Now you can see a pretty coloured diff with the changes you have made to your .docx file since the last commit
`git wdiff file.docx`

To see all changes over time
`git log -p --word-diff=color file.docx`

Track changes in Word (.docx) documents getting a diff with the commit.
Automatically when running git commit.
This is only going to work from linux/Mac or Windows running git from a bash shell.

Install pandoc. Pandoc is a program to convert between different file formats. It's going to allow us to convert Word files (.docx) to Markdown (.md).

Set up git hooks to enable automatic generation and tracking of Markdown copies of .docx files.

Copy these hook files to your git project's .git/hooks directory and rename them, or soft-link to them with ln -s, and make them executable` (chmod u+x *.sh)`:
```bash
pre-commit-git-diff-docx.sh -> .git/hooks/pre-commit
post-commit-git-diff-docx.sh -> .git/hooks/post-commit
```
Now every time you run git commit, the pre-commit hook will automatically run before you see the window to enter the log message. The hook is a script that makes a copy in Markdown format (.md) of every .docx file you are committing. The post-commit hook then amends the commit adding the .md files.















