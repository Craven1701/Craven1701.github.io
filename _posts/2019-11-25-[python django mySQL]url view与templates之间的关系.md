---
title: 【python+django+mySQL】url view 与 templates之间的关系
published: true
---

urls.py负责通过匹配对应的url，给views.py里面的各种定义的view发送一个request请求，相应的view接收到request请求后，进行一系列的操作，返回一个render形式的数据包，包含了需要传回前端的数据，以及应该使用进行渲染的html文件名。