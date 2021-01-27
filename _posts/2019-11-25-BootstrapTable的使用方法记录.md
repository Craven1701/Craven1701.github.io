---
title: Bootstrap Table的使用方法记录
published: true
tag: Bootstrap
---

实现了从后端服务器中查询数据，并返回给前端，通过BootStrap Table渲染到表格中。

 1. 引入的js文件与css文件如下：

```css
<link rel="stylesheet" href="https://unpkg.com/bootstrap-table@1.15.5/dist/bootstrap-table.min.css">
<script src="https://unpkg.com/bootstrap-table@1.15.5/dist/bootstrap-table.min.js"></script>
```
 2. 表格的初始化与一些基本参数的配置：


```javascript
function loadData(tableId, url) {
    $(tableId).bootstrapTable('destroy'); // 清除缓存表格数据
    $(tableId).bootstrapTable({
        striped: true, // 隔行变色
        url: url,
        method: "get",
        dataType: "json",
        pagination: true, //分页
        sidePagination: "client", //客户端处理分页
        pageNumber: 1, // 默认显示第几页
        pageSize: 10, // 每页显示条数
//	    sortable: false,
//	    cache: false, // 默认true 设置为 false 禁用 AJAX 数据缓存
        contentType: "application/x-www-form-urlencoded", // 参数提交类型，默认以application/json提交
        queryParams: function (params) {
            return {
                keyWord: $('#keyWord').val(),
                dataSet: $('#dataSet').find("option:selected").attr('value'),
                fieldName: $('#fieldName').find("option:selected").attr('value'),
            };
        },// 请求参数，发往后台，通过自定义参数将查询时需要的数据传给后台
	    // responseHandler:function(res){ // res为从服务器请求到的数据
	    // 	return res.data;
	    // },
        columns: [ // 渲染列
            {
                title: 'B',
                field: 'B',
                align: 'center',
                valign: 'middle',
            },
            {
                title: 'C',
                field: 'C',
                align: 'center',
                valign: 'middle',
            },
            {
                title: 'names',
                field: 'names',
                align: 'center'
            },
            {
                title: 'L',
                field: 'l',
                align: 'center',
            },
            {
                title: 'G_Name',
                field: 'G_Name',
                align: 'center',
            },
            {
                title: 'annotation',
                field: 'annotation',
                align: 'center',
            },
        ]
    });
}
```
基本的设置大致如此，网上这一类的资料也很多。需要注意点有一下几个：

 - 最开始需要用`
   $(tableId).bootstrapTable('destroy');`来清除缓存的数据表格，否则可能查询的结果展示不出来；
 - 不同的分页方式对后台返回的数据格式有不同的要求，在返回数据时需要注意一下，这里用到是客户端处理分页`sidePagination:
   "client", `网上大多数资料用到都是服务器端处理分页，即`sidePagination: "server",
   `但是我不想调整返回的数据格式了所以就用`client`了，这个返回格式比较简单。
   （返回格式的差异参看这个链接：[https://www.jb51.net/article/117388.htm](https://www.jb51.net/article/117388.htm)）
 - **最为关键的一个参数，`queryParams`的设置：**
   
   ```javascript  queryParams: function (params) {
               return {
                   keyWord: $('#keyWord').val(),
                   dataSet: $('#dataSet').find("option:selected").attr('value'),
                   fieldName: $('#fieldName').find("option:selected").attr('value'),
               };
           }, ```
   ```

 **`queryParams`就相当于`$.ajax()`里的的参数`data`,通过自定义参数的形式，将前端取出来的数据传到后端去，通常就是传一些查询条件之类的。** 

我一开始确实没理解到这里是可以自定义参数的，在网上看到的例子也都是用的limit什么之类的奇怪的参数，耽误了我很多时间来尝试在哪给后台发查询条件……Orz。  我使用的是`get`的方法向后台传递数据，数据发到后台的格式如下图所示 ：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019112508514742.png)
最后处理一下返回数据的格式，就完成了。后端的代码如下：

```python
def search(request):
    request.encoding='utf-8'
    keyWord = request.GET['keyWord']
    dataSet = request.GET['dataSet']
    fieldName = request.GET['fieldName']

    if keyWord:
        sql = 'SELECT * from ' + dataSet + ' WHERE ' + fieldName + ' = "' + keyWord+'"'
    else:
        sql = 'SELECT * from ' + dataSet
    data = pd.read_sql_query(sql, engine).to_json(orient='records')
    if data:
        return HttpResponse(data)
    else:
        return render(request,{
            'Message': 'No result found.'
        })
```

> 参考资料：
> Bootstrap Table官方文档：[https://bootstrap-table.com/](https://bootstrap-table.com/)
> Table的初始化与有关参数详解：
> [https://www.jb51.net/article/144386.htm](https://www.jb51.net/article/144386.htm)
> [https://zhuanlan.zhihu.com/p/46609913](https://zhuanlan.zhihu.com/p/46609913)
> 后端返回数据时的格式控制问题：[https://www.jb51.net/article/117388.htm](https://www.jb51.net/article/117388.htm)
> （其实还看了好多别的- - 但是都大同小异 这几个比较有代表性一点所以就引用这几个吧）