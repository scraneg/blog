---
title: Node.js期中爬虫实验项目
tags:
  - 树莓派
  - web
categories:
  - node.js
  - 爬虫
abbrlink: 5526
date: 2021-07-16 00:05:18
---
# 期中作业要求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416162003770.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70#pic_center)
<!--more-->
# 基础概念引入
>前后端
>前台：呈现给用户的视觉和基本的操作。
后台：用户浏览网页时，我们看不见的后台数据跑动。后台包括前端、后端。
前端：对应html、css、javascript 等网页语言作用在前端网页。 
后端：对应jsp、javaBean、dao层、action层和service层的业务逻辑代码。（包括数据库）


>Web
>定义：Web 是一种基于超文本和HTTP协议的、全球性的、动态交互的、跨平台的分布式图形信息系统，是建立在Internet上的一种网络服务。
>组成部分：
>结构标准语言（html）：存放网页元素。决定网页结构和内容（决定看到什么），相当于人的身体（描述类）
>表现标准语言（css）：对元素的样式进行设置，例如宽度、高度、位置、字体。决定网页呈现给用户的模样（决定好不好看），相当于化妆（描述类）
>行为标准语言（javascript）：对元素取值，动态的改变。实现业务逻辑和页面控制（决定功能），相当于人的各种动作（编程类）

>Node.js
>Node.js 就是运行在服务端的 JavaScript。
Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google Chrome的V8引擎，V8引擎执行JavaScript的速度非常快，性能非常好。
node.js属于异步编程：发送初始请求，没有得到响应的时候可以继续发送请求，如果第一个发送的请求得到响应了，就返回。即同时执行多个。
而如php，java是同步，必须得到响应之后才能继续发送请求

>检查和源码的区别
1.查看源代码：他人服务器发送到浏览器的原封不动的代码，也就是最原始的代码。在源代码中找不到的代码，是在浏览器执行js动态生成的。
2.检查（F12）：最终的html代码。即：源代码 + 网页js渲染 。
# 前期准备工作
## 安装node.js
[node.js下载地址](https://nodejs.org/en/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417074824106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)


## 安装数据库
以使用最广泛的开源数据库MySQL为例
[mysql下载地址](https://dev.mysql.com/downloads/mysql/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416211812136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
老师给的ppt上有详细的初始配置流程

[mysql配置也可参考网站](https://www.cnblogs.com/winton-nfs/p/11524007.html)
## 安装Navicat Premium 15
学姐推荐下载这个软件，可以使爬取的数据显示更为清晰明了

Navicat是一套快速、可靠的数据库管理工具。
[Navicat下载地址](http://www.navicat.com.cn/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416212851962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 正则表达式学习网站
[regular expressions 101](https://regex101.com/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417222449923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
[菜鸟教程正则表达式在线测试](https://c.runoob.com/front-end/854)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417223801892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)

# 课堂示例演示
## 示例一（显示在终端中
### 代码
```js
var myRequest=require('request')
var myCheerio=require('cheerio')
var myURL='http://www.ecnu.edu.cn/e5/bc/c1950a255420/page.htm'

function request(url,callback){
    var options={
        url:url,
        encoding:null,
        headers:null
    }
    myRequest(options,callback)
}

request(myURL,function(err,res,body){
    var html=body
    var $=myCheerio.load(html,{decodeEntities:false})
    console.log($.html())
    // html页面title的信息
    console.log('title:'+$('title').text())
    // html页面的摘要的信息
    // console.log('description:'+$('meta[name="description"]').eq(0).attr("content"))  
})
```
### 运行结果
实际现在这个网站不可访问，无法输出下列结果，但可以正常输出网页结构，即运行成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416195341705.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 示例二（保存在本地文件
>流程
>1、读取种子页面
2、分析出种子页面里的所有新闻链接
3、爬取所有新闻链接的内容
4、分析新闻页面内容，解析出结构化数据
5、将结构化数据保存到本地文件

### 代码
require引入模块。保存在本地文件的关键模块是fs。故在后续存入数据库的代码中，fs模块是可以省略的。
```js
var fs = require('fs');//通过require引入各种包，以方便功能地实现request，fs功能是使爬取内容保存在本地，对系统文件和目录读取
var myRequest = require('request')//发送http请求
var myCheerio = require('cheerio')//获取html内容，使用jquery的方式
var myIconv = require('iconv-lite')//用于各字符集间高效的转码，编码转码gb2312到utf-8
require('date-utils');//为了解析日期的方便
```

获取URL（即获取要访问的网址
>Uniform Resource Locator,统一资源定位器。它是万维网（www）的统一资源定位标志，即指网络地址。
URL格式一般如下：
协议类型://服务器地址[:端口号]/路径/文件名[参数=值]
```js
var source_name = "中国新闻网";//获取url 其格式一般为“协议名+主机名/IP地址及其端口+文件路径”
var myEncoding = "utf-8";//字符编码
var seedURL = 'http://www.chinanews.com/';//目标网址中国新闻网
```

定义新闻元素的读取方式，即规定哪些url可有作为新闻页面，最后两行使用的是正则表达式，用于检查网页是否符合规范
>正则表达式（regular expression)描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。
>[正则表达式语法教程](https://www.runoob.com/regexp/regexp-syntax.html)
```js
var seedURL_format = "$('a')";
var keywords_format = " $('meta[name=\"keywords\"]').eq(0).attr(\"content\")";
var title_format = "$('title').text()";
var date_format = "$('#pubtime_baidu').text()";
var author_format = "$('#editor_baidu').text()";
var content_format = "$('.left_zw').text()";
var desc_format = " $('meta[name=\"description\"]').eq(0).attr(\"content\")";
var source_format = "$('#source_baidu').text()";
var url_reg = /\/(\d{4})\/(\d{2})-(\d{2})\/(\d{7}).shtml/;
var regExp = /((\d{4}|\d{2})(\-|\/|\.)\d{1,2}\3\d{1,2})|(\d{4}年\d{1,2}月\d{1,2}日)/
```

构造一个模仿浏览器的request
```js
//防止网站屏蔽爬虫
//如果爬取过于频繁，可能会被网站屏蔽，这段代码的作用是伪装成浏览器，防止被屏蔽
var headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36'
}

//request模块异步fetch url
function request(url, callback) {
    var options = {
        url: url,
        encoding: null,
        headers: headers,
        timeout: 10000 
    }
    myRequest(options, callback)
}
```
读取种子页面，解析出种子页面里所有的a herf链接
```js
request(seedURL, function(err, res, body) {
    // try {
    //用iconv转换编码
    var html = myIconv.decode(body, myEncoding);
    //console.log(html);
    //准备用cheerio解析html
    var $ = myCheerio.load(html, { decodeEntities: true });
    // } catch (e) { console.log('读种子页面并转码出错：' + e) };
    var seedurl_news;
    try {
        seedurl_news = eval(seedURL_format);
        //console.log(seedurl_news);
    } catch (e) { console.log('url列表所处的html块识别出错：' + e) };
```
遍历种子页面里所有的a herf链接
规整化所有链接，如果满足新闻url的正则表达式就爬取
```js
    seedurl_news.each(function(i, e) { 
        var myURL = "";
        try {
            //得到具体新闻url
            var href = "";
            href = $(e).attr("href");
            if (typeof(href) == "undefined") {  // 有些网页地址undefined
                return true;
            }
            if (href.toLowerCase().indexOf('http://') >= 0 || href.toLowerCase().indexOf('https://') >= 0) myURL = href; //http://开头的或者https://开头
            else if (href.startsWith('//')) myURL = 'http:' + href; ////开头的
            else myURL = seedURL.substr(0, seedURL.lastIndexOf('/') + 1) + href; //其他

        } catch (e) { console.log('识别种子页面中的新闻链接出错：' + e) }

        if (!url_reg.test(myURL)) return; //检验是否符合新闻url的正则表达式
        //console.log(myURL);
        newsGet(myURL); //读取新闻页面
    });
});
```
读取具体的新闻页面，构造一个空的fetch对象用于存储数据
```js
function newsGet(myURL) { //读取新闻页面
    request(myURL, function(err, res, body) { //读取具体的新闻页面，构造一个空的fetch对象用于存储数据
        //try {
        var html_news = myIconv.decode(body, myEncoding); //用iconv转换编码
        //console.log(html_news);
        //准备用cheerio解析html_news
        var $ = myCheerio.load(html_news, { decodeEntities: true });
        myhtml = html_news;
        //} catch (e) {    console.log('读新闻页面并转码出错：' + e);};

        console.log("转码读取成功:" + myURL);
        //动态执行format字符串，构建json对象准备写入文件或数据库
        var fetch = {};
        fetch.title = "";
        fetch.content = "";
        fetch.publish_date = (new Date()).toFormat("YYYY-MM-DD");
        //fetch.html = myhtml;
        fetch.url = myURL;
        fetch.source_name = source_name;
        fetch.source_encoding = myEncoding; //编码
        fetch.crawltime = new Date();
```
读取新闻页面中的元素并保存到fetch对象里
```js
        if (keywords_format == "") fetch.keywords = source_name; // eval(keywords_format);  //没有关键词就用sourcename
        else fetch.keywords = eval(keywords_format);

        if (title_format == "") fetch.title = ""
        else fetch.title = eval(title_format); //标题

        if (date_format != "") fetch.publish_date = eval(date_format); //刊登日期   
        console.log('date: ' + fetch.publish_date);
        console.log(myURL);
        fetch.publish_date = regExp.exec(fetch.publish_date)[0];
        fetch.publish_date = fetch.publish_date.replace('年', '-')
        fetch.publish_date = fetch.publish_date.replace('月', '-')
        fetch.publish_date = fetch.publish_date.replace('日', '')
        fetch.publish_date = new Date(fetch.publish_date).toFormat("YYYY-MM-DD");

        if (author_format == "") fetch.author = source_name; //eval(author_format);  //作者
        else fetch.author = eval(author_format);

        if (content_format == "") fetch.content = "";
        else fetch.content = eval(content_format).replace("\r\n" + fetch.author, ""); //内容,是否要去掉作者信息自行决定
```
将fetch对象（获取新闻页面中的信息）存储为json文件
```js
        if (source_format == "") fetch.source = fetch.source_name;
        else fetch.source = eval(source_format).replace("\r\n", ""); //来源

        if (desc_format == "") fetch.desc = fetch.title;
        else fetch.desc = eval(desc_format).replace("\r\n", ""); //摘要    

        var filename = source_name + "_" + (new Date()).toFormat("YYYY-MM-DD") +
            "_" + myURL.substr(myURL.lastIndexOf('/') + 1) + ".json";
        ////存储json
        fs.writeFileSync(filename, JSON.stringify(fetch));
    });
}
```
### 运行结果
在终端中显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416201523158.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
在爬虫文件所在路径下显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416201833339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
打开其中一个json文件，可以看到爬取下来的数据（右键格式化文档使显示美观
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416202019239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 示例三（保存在数据库中
>流程
>1、数据库存储爬取的数据
2、爬取新闻页面之前先查询数据库，是否该url已经爬取过了
3、设置爬虫定时工作
### 代码
示例三建立在示例二的基础上，变动在于储存爬取数据的位置由本地json文件变为数据库。则在其基础上修改即可。准备工作为下载mysql，并进行安装配置。

建表
```sql
CREATE TABLE `fetches` (
  `id_fetches` int(11)  NOT NULL AUTO_INCREMENT,--Auto-increment 会在新记录插入表中时生成一个唯一的数字，所以在navicat中查看时每一行前面会有数字标记
  `url` varchar(200) DEFAULT NULL,--varchar()针对变长数据，如果一个字段值是不固定长度的，只知道它不可能超过10个字符，则它定义为varchar(10)
  `source_name` varchar(200) DEFAULT NULL,--默认为空，如果这个字段不为空就会在表格里显示null，如果默认不为空default not null，如果默认不为空，这个字段又没有值，是这一条链接中的所有数据都不能插入，会报错
  `source_encoding` varchar(45) DEFAULT NULL,
  `title` varchar(200) DEFAULT NULL,
  `keywords` varchar(200) DEFAULT NULL,
  `author` varchar(200) DEFAULT NULL,
  `publish_date` date DEFAULT NULL,
  `crawltime` datetime DEFAULT NULL,
  `content` longtext,--比text字节数长
  `createtime` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id_fetches`),
  UNIQUE KEY `id_fetches_UNIQUE` (`id_fetches`),
  UNIQUE KEY `url_UNIQUE` (`url`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;--规定编码为utf-8
```
创建mysql.js

```js
var mysql = require("mysql");
var pool = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: 'root',
    database: 'crawl'
});
var query = function(sql, sqlparam, callback) {
    pool.getConnection(function(err, conn) {
        if (err) {
            callback(err, null, null);
        } else {
            conn.query(sql, sqlparam, function(qerr, vals, fields) {
                conn.release(); //释放连接 
                callback(qerr, vals, fields); //事件驱动回调 
            });
        }
    });
};
var query_noparam = function(sql, callback) {
    pool.getConnection(function(err, conn) {
        if (err) {
            callback(err, null, null);
        } else {
            conn.query(sql, function(qerr, vals, fields) {
                conn.release(); //释放连接 
                callback(qerr, vals, fields); //事件驱动回调 
            });
        }
    });
};
exports.query = query;
exports.query_noparam = query_noparam;
```

require再引入./mysql.js模块
```js
var mysql = require('./mysql.js');
```
newsGet函数变动为
```js
// var filename = source_name + "_" + (new Date()).toFormat("YYYY-MM-DD") +
        //     "_" + myURL.substr(myURL.lastIndexOf('/') + 1) + ".json";
        // ////存储json
        // fs.writeFileSync(filename, JSON.stringify(fetch));

        var fetchAddSql = 'INSERT INTO fetches(url,source_name,source_encoding,title,' +
            'keywords,author,publish_date,crawltime,content) VALUES(?,?,?,?,?,?,?,?,?)';
        var fetchAddSql_Params = [fetch.url, fetch.source_name, fetch.source_encoding,
            fetch.title, fetch.keywords, fetch.author, fetch.publish_date,
            fetch.crawltime.toFormat("YYYY-MM-DD HH24:MI:SS"), fetch.content
        ];

        //执行sql，数据库中fetch表里的url属性是unique的，不会把重复的url内容写入数据库
        mysql.query(fetchAddSql, fetchAddSql_Params, function(qerr, vals, fields) {
            if (qerr) {
                console.log(qerr);
            }
        }); //mysql写入
```
查询数据库判断是否已读取（防止重复存入）
```js
var fetch_url_Sql = 'select url from fetches where url=?';
            var fetch_url_Sql_Params = [myURL];
            mysql.query(fetch_url_Sql, fetch_url_Sql_Params, function(qerr, vals, fields) {
                if (vals.length > 0) {
                    console.log('URL duplicate!')
                } else newsGet(myURL); //读取新闻页面
```
### 运行结果
在终端显示
在每次爬取新闻页面之前先查询数据库，是否该url已经爬取过了，故在爬取到相同的内容时，会输出URL duplicate!
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417075934756.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70#pic_center)


在命令行中显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210416214616732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
在navicat中显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041707542935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
### 定时爬取
在命令行中下载node-schedule模块
npm install node-schedule

引入模块
```js
var schedule = require('node-schedule');
```

增加定时执行代码。即当爬虫程序运行时，没到规定时间，会自动爬取想要获得的数据。
```js
var rule = new schedule.RecurrenceRule();
var times = [0, 14]; //每天2次自动执行
var times2 = 50; //定义在第几分钟执行
rule.hour = times;
rule.minute = times2;

//定时执行httpGet()函数
schedule.scheduleJob(rule, function() {
    seedget();
});
```

## 示例四（在浏览器显示爬取数据
>流程
>1、用mysql查询已爬取的数据
2、用网页发送请求到后端查询
3、用express构建网站访问mysql
4、用表格显示查询结果
5、可以尝试的其他扩展

### 用mysql查询已爬取的数据
```js
var mysql = require('./mysql.js');
var title = '新冠';
var select_Sql = "select title,author,publish_date from fetches where title like '%" + title + "%'";

mysql.query(select_Sql, function(qerr, vals, fields) {
    console.log(vals);
});
```
运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417094948556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
### 用网页发送请求到后端查询
在浏览器中运行html文件作为前端
```html
<!DOCTYPE html>
<html>

<body>
    <form action="http://127.0.0.1:8080/7.02.html" method="GET">
        <br> 标题：<input type="text" name="title">
        <input type="submit" value="Submit">
    </form>
    <script>
    </script>
</body>

</html>
```
浏览器运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417095234124.png)
运行js文件作为后端
```js
var http = require('http');
var fs = require('fs');
var url = require('url');
var mysql = require('./mysql.js');
http.createServer(function(request, response) {
    var pathname = url.parse(request.url).pathname;
    var params = url.parse(request.url, true).query;
    fs.readFile(pathname.substr(1), function(err, data) {
        response.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
        if ((params.title === undefined) && (data !== undefined))
            response.write(data.toString());
        else {
            response.write(JSON.stringify(params));
            var select_Sql = "select title,author,publish_date from fetches where title like '%" +
                params.title + "%'";
            mysql.query(select_Sql, function(qerr, vals, fields) {
                console.log(vals);
            });
        }
        response.end();
    });
}).listen(8080);
console.log('Server running at http://127.0.0.1:8080/');
```
终端运行结果
![](https://img-blog.csdnimg.cn/2021041709573843.png)
浏览器运行结果（运行js文件后在浏览器中访问http://127.0.0.1:8080/7.02.html（7.02是上面html文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417100315341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417100325173.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
### 用express构建网站访问mysql
前端
```html
<!DOCTYPE html>
<html>

<body>
    <form action="http://127.0.0.1:8080/process_get" method="GET">
        <br> 标题：<input type="text" name="title">
        <input type="submit" value="Submit">
    </form>
    <script>
    </script>
</body>

</html>
```
运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417100537167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
后端
```js
var express = require('express');
var mysql = require('./mysql.js')
var app = express();
//app.use(express.static('public'));
app.get('/7.03.html', function(req, res) {
    res.sendFile(__dirname + "/" + "7.03.html");
})
app.get('/7.04.html', function(req, res) {
    res.sendFile(__dirname + "/" + "7.04.html");
})
app.get('/process_get', function(req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html;charset=utf-8' }); //设置res编码为utf-8
    //sql字符串和参数
    var fetchSql = "select url,source_name,title,author,publish_date from fetches where title like '%" +
        req.query.title + "%'";
    mysql.query(fetchSql, function(err, result, fields) {
        console.log(result);
        res.end(JSON.stringify(result));
    });
})
var server = app.listen(8080, function() {
    console.log("访问地址为 http://127.0.0.1:8080/7.03.html")
})
```
终端运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417101052911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)

浏览器运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417100947235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417100938493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
### 用表格显示查询结果
用express脚手架来创建一个网站框架
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417143736577.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417151412897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)


# 爬虫项目展示
## 雪球网
### 分析
抓取数据有两种方式：
1.使用正则表达，适用于日期、网页链接这种有固定格式的
如https://xueqiu.com/1514349083/177532365
用于匹配的正则表达式为
```js
var url_reg = /\/(\d{10})\/(\d{9})/;
```
2.使用jquery语法，适用于在网页源码/检查页面中标签下的数据，可见下
ps.如果网页链接不用正则，就是把页面上所有的a href链接都抓回来，有的可能不是我们想要的页面。

获取网页来源名和字符编码（初始声明为“雪球网”和“utf-8”，直接引用
```js
fetch.source_name = source_name;
fetch.source_encoding = myEncoding;
```

获取帖名
源码显示如下，则是文本内容，用.text()
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418161803951.png)
获取内容和关键字
源码如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418162314491.png)
```js
fetch.content=$('meta[name="description"]').attr("content");
fetch.keywords= $('meta[name="keywords"]').attr("content");```js
fetch.title=$("title").text();
```
获取发帖人昵称
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418161019801.png)
以下写法无法获取
```js
fetch.author=$('a').attr("data-screenname");
```
获取div，然后再去获取属性。修改为以下，依旧无法获取
```js
fetch.author=$("div").children("a").attr("data-screenname");
```
再指定这个div的class，修改成功
```js
fetch.author=$("div.article__author").children("a").attr("data-screenname");
```
完整的信息筛选代码
```js
var fetch = {};
    fetch.url = myURL;
    fetch.title=$("title").text();
    //fetch.title = $('h1[class="article__bd__title"]').text();*/
    fetch.content=$('meta[name="description"]').attr("content");
    //fetch.content = $('div[class="article__bd__detail"]').text();
    fetch.publish_date = $('div[class="avatar__subtitle"]').children("a").text();
    fetch.source_name = source_name;
    fetch.source_encoding = myEncoding;
    fetch.crawltime = new Date();
    fetch.author=$("div.article__author").children("a").attr("data-screenname");
    //fetch.author = $('div[class="avatar__name"]').children("a").text().split("(")[0];也可以，只不过获取的位置不同
    fetch.keywords= $('meta[name="keywords"]').attr("content");
```
进入数据库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420145949493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
建表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420150004316.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420150125857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
如果建错想要删除（别忘了分号
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042015014842.png)
在数据库中显示已有表格
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420151824299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)

在js文件中编写存入数据库语句
```js
var fetchadd ='INSERT INTO xueqiu(url,source_name,source_encoding,title,keywords,author,publish_date,crawltime,content )VALUES(?,?,?,?,?,?,?,?,?)';
    
    var fetchadd_params=[fetch.url,fetch.source_name,fetch.source_encoding,fetch.title,fetch.keywords,fetch.author,fetch.publish_date,fetch.crawltime,fetch.content];
    mysql.query(fetchadd,fetchadd_params,function(qerr,vals,fields){});
```
数据库显示
直接在数据库中显示数据会非常的凌乱，可以采用可视化的界面去操作，比如MySQL workbench。也可以使用学姐推荐的navicat。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420151849368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
navicat展示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420150736115.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
浏览器中显示
参照课堂测试案例四，在浏览器中显示需要前端html文件>后端js文件>express构建网站访问mysql>express脚手架构建网z站框架>用表格显示数据结果
这里需要在老师给的代码的基础上做出小小的改动
后端修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210420223834557.png)
前端修改一下展示的名称，中文更鲜明
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210423131548709.png#pic_center)

增加一个动态背景
[随鼠标变化的蜘蛛网线条](https://github.com/hustcc/canvas-nest.js)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210423131522148.png)

加上表格（这里还更改了表格默认的颜色和透明度
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042313171651.png#pic_center)
浏览器运行结果显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210423132331819.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70#pic_center)

### 代码
```js
var myRequest = require('request');
var myIconv = require('iconv-lite');
var myCheerio = require('cheerio');
var mysql = require('./mysql.js');

var myEncoding = "utf-8";
var source_name = "雪球网";
var seedURL = 'https://xueqiu.com/';

function request(url, callback) {
    var options = {
        url: url,
        encoding: null,
        //proxy: 'http://x.x.x.x:8080',
        headers: headers,
        timeout: 10000 //
    }
    myRequest(options, callback)
};

var headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36'
}
seedget();

function seedget() {
    request('https://xueqiu.com/', function(err, res, body) { 
        
        var html = myIconv.decode(body, myEncoding);
       
        var $ = myCheerio.load(html, { decodeEntities: true });
        var seedurl_news;
        try {
            seedurl_news = eval($(".AnonymousHome_a__placeholder_3RZ"));
        } catch (e) { console.log('url列表所处的html块识别出错：' + e) };
    
        seedurl_news.each(function(i, e) { 
            var myURL = "";
            var href="";
            try {
               href=$(e).attr("href");
               console.log(href);
               myURL='https://xueqiu.com'+href
            } catch (e) { console.log('识别种子页面中的新闻链接出错：' + e) }

            newsget(myURL)

            


        });
    });
}
function newsget(myURL){


 request(myURL, function(err, res, body){
    var html_news = myIconv.decode(body, myEncoding); 
    var $ = myCheerio.load(html_news, { decodeEntities: true });
    myhtml = html_news; 
    
    console.log("转码读取成功:" + myURL);

    var fetch = {};
    fetch.url = myURL;
    fetch.title=$("title").text();
    //fetch.title = $('h1[class="article__bd__title"]').text();*/
    fetch.content=$('meta[name="description"]').attr("content");
    //fetch.content = $('div[class="article__bd__detail"]').text();
    fetch.publish_date = $('div[class="avatar__subtitle"]').children("a").text();
    fetch.source_name = source_name;
    fetch.source_encoding = myEncoding;
    fetch.crawltime = new Date();
    fetch.author=$("div.article__author").children("a").attr("data-screenname");
    //fetch.author = $('div[class="avatar__name"]').children("a").text().split("(")[0];也可以，只不过获取的位置不同
    fetch.keywords= $('meta[name="keywords"]').attr("content");

    var fetchadd ='INSERT INTO xueqiu(url,source_name,source_encoding,title,keywords,author,publish_date,crawltime,content )VALUES(?,?,?,?,?,?,?,?,?)';
    
    var fetchadd_params=[fetch.url,fetch.source_name,fetch.source_encoding,fetch.title,fetch.keywords,fetch.author,fetch.publish_date,fetch.crawltime,fetch.content];
    mysql.query(fetchadd,fetchadd_params,function(qerr,vals,fields){});
  
 });

}
```
### 热度分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427173727180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 人民网
### 分析
修改正则表达式
```js
var seedURL_format = "$('a')";
var keywords_format = " $('meta[name=\"keywords\"]').eq(0).attr(\"content\")";
var title_format = "$('title').text()";
var date_format = "$('meta[name=\"publishdate\"]').eq(0).attr(\"content\")";
var author_format = "$('meta[name=\"author\"]').eq(0).attr(\"content\")";
var content_format = "$('.fl text_con_left').text()";
var desc_format = " $('meta[name=\"description\"]').eq(0).attr(\"content\")";
var source_format = "$('meta[name=\"source\"]').eq(0).attr(\"content\")";
var url_reg = /\/(\w{2})\/(\d{4})\/(\d{4})\/(\w{5,10}-(\w{8,15})).html/;
var regExp = /((\d{4}|\d{2})(\-|\/|\.)\d{1,2}\3\d{1,2})|(\d{4}年\d{1,2}月\d{1,2}日)/
```
可以运行，但爬取下来的数据在navicat里显示为乱码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418114502627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
咨询学长，给出的建议是把myEncoding改为GBK。因为人民网比较特殊，可能请求后响应就是用的GBK 。则一般情况下使用utf-8，如果有乱码问题再尝试GBK。

修改后运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041811515774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
在浏览器中显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210423143126566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)

### 代码
```js
var fs = require('fs');
var myRequest = require('request');
var myCheerio = require('cheerio');
var myIconv = require('iconv-lite');
require('date-utils');
var mysql = require('./mysql.js');

var source_name = "人民网";
var domain = 'http://www.people.com.cn/';
var myEncoding = "gbk";
var seedURL = 'http://www.people.com.cn/';

var seedURL_format = "$('a')";
var keywords_format = " $('meta[name=\"keywords\"]').eq(0).attr(\"content\")";
var title_format = "$('title').text()";
var date_format = "$('meta[name=\"publishdate\"]').eq(0).attr(\"content\")";
var author_format = "$('meta[name=\"author\"]').eq(0).attr(\"content\")";
var content_format = "$('.fl text_con_left').text()";
var desc_format = " $('meta[name=\"description\"]').eq(0).attr(\"content\")";
var source_format = "$('meta[name=\"source\"]').eq(0).attr(\"content\")";
var url_reg = /\/(\w{2})\/(\d{4})\/(\d{4})\/(\w{5,10}-(\w{8,15})).html/;
var regExp = /((\d{4}|\d{2})(\-|\/|\.)\d{1,2}\3\d{1,2})|(\d{4}年\d{1,2}月\d{1,2}日)/

//防止网站屏蔽我们的爬虫
var headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36'
}

//request模块异步fetch url
function request(url, callback) {
    var options = {
        url: url,
        encoding: null,
        //proxy: 'http://x.x.x.x:8080',
        headers: headers,
        timeout: 10000 //
    }
    myRequest(options, callback)
};

seedget();

function seedget() {
    request(seedURL, function(err, res, body) { //读取种子页面
        // try {
        //用iconv转换编码
        var html = myIconv.decode(body, myEncoding);
        //console.log(html);
        //准备用cheerio解析html
        var $ = myCheerio.load(html, { decodeEntities: true });
        // } catch (e) { console.log('读种子页面并转码出错：' + e) };
        var seedurl_news;
        try {
            seedurl_news = eval(seedURL_format);
        } catch (e) { console.log('url列表所处的html块识别出错：' + e) };
        seedurl_news.each(function(i, e) { //遍历种子页面里所有的a链接
            var myURL = "";
            try {
                //得到具体新闻url
                var href = "";
                href = $(e).attr("href");
                if (href == undefined) return;
                if (href.toLowerCase().indexOf('http://') >= 0||href.toLowerCase().indexOf('https；://') >= 0) myURL = href;
                //http://开头的，之前这个文件虽然也能爬，但是会报错，原因是只爬了http开头的网址，现在加了https开头的网址，就好了
                else if (href.startsWith('//')) myURL = 'http:' + href; ////开头的
                else myURL = seedURL.substr(0, seedURL.lastIndexOf('/') + 1) + href; //其他

            } catch (e) { console.log('识别种子页面中的新闻链接出错：' + e) }

            if (!url_reg.test(myURL)) return; //检验是否符合新闻url的正则表达式
            //console.log(myURL);

            var fetch_url_Sql = 'select url from fetches where url=?';
            var fetch_url_Sql_Params = [myURL];
            mysql.query(fetch_url_Sql, fetch_url_Sql_Params, function(qerr, vals, fields) {
                if (vals.length > 0) {
                    console.log('URL duplicate!')
                } else newsGet(myURL); //读取新闻页面
            });
        });
    });
};

function newsGet(myURL) { //读取新闻页面
    request(myURL, function(err, res, body) { //读取新闻页面
        //try {
        var html_news = myIconv.decode(body, myEncoding); //用iconv转换编码
        //console.log(html_news);
        //准备用cheerio解析html_news
        var $ = myCheerio.load(html_news, { decodeEntities: true });
        myhtml = html_news;
        //} catch (e) {    console.log('读新闻页面并转码出错：' + e);};

        console.log("转码读取成功:" + myURL);
        //动态执行format字符串，构建json对象准备写入文件或数据库
        var fetch = {};
        fetch.title = "";
        fetch.content = "";
        fetch.publish_date = eval(date_format);
        //fetch.html = myhtml;
        fetch.url = myURL;
        fetch.source_name = source_name;
        fetch.source_encoding = myEncoding; //编码
        fetch.crawltime = new Date();

        if (keywords_format == "") fetch.keywords = source_name; // eval(keywords_format);  //没有关键词就用sourcename
        else fetch.keywords = eval(keywords_format);

        if (title_format == "") fetch.title = ""
        else fetch.title = eval(title_format); //标题

        
        if (author_format == "") fetch.author = source_name; //eval(author_format);  //作者
        else fetch.author = eval(author_format);

        if (content_format == "") fetch.content = "";
        else fetch.content = eval(content_format).replace("\r\n" + fetch.author, ""); //内容,是否要去掉作者信息自行决定

        if (source_format == "") fetch.source = fetch.source_name;
        else fetch.source = eval(source_format).replace("\r\n", ""); //来源

        if (desc_format == "") fetch.desc = fetch.title;
        else fetch.desc = eval(desc_format).replace("\r\n", ""); //摘要    

        // var filename = source_name + "_" + (new Date()).toFormat("YYYY-MM-DD") +
        //     "_" + myURL.substr(myURL.lastIndexOf('/') + 1) + ".json";
        // ////存储json
        // fs.writeFileSync(filename, JSON.stringify(fetch));

        var fetchAddSql = 'INSERT INTO fetches(url,source_name,source_encoding,title,' +
            'keywords,author,publish_date,crawltime,content) VALUES(?,?,?,?,?,?,?,?,?)';
        var fetchAddSql_Params = [fetch.url, fetch.source_name, fetch.source_encoding,
            fetch.title, fetch.keywords, fetch.author, fetch.publish_date,
            fetch.crawltime.toFormat("YYYY-MM-DD HH24:MI:SS"), fetch.content
        ];

        //执行sql，数据库中fetch表里的url属性是unique的，不会把重复的url内容写入数据库
        mysql.query(fetchAddSql, fetchAddSql_Params, function(qerr, vals, fields) {
            if (qerr) {
                console.log(qerr);
            }
        }); //mysql写入
    });
}
```
### 热度分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427173800795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 网易新闻网
### 分析
参考老师给出的对中国新闻网页面的爬虫，在此基础上修改对应爬取数据的正则表达式即可

正则表达式修改结果
```js
var seedURL_format = "$('a')";
var keywords_format = " $('meta[name=\"keywords\"]').eq(0).attr(\"content\")";
var title_format = " $('meta[property=\"og:title\"]').eq(0).attr(\"content\")";
var date_format = "$('.post_time_source').text()"|"$(#ptime).text()";
var author_format = "$('meta[name=\"author\"]').eq(0).attr(\"content\")";
var content_format = "$('#endText').text()";
var desc_format = " $('meta[name=\"description\"]').eq(0).attr(\"content\")";
var source_format = "$('#ne_article_source').text()";
var url_reg = /\/(\d{2})\/(\d{4})\/(\d{2})\/(\w{10,30}).html/;
var regExp = /((\d{4}|\d{2})(\-|\/|\.)\d{1,2}\3\d{1,2})|(\d{4}年\d{1,2}月\d{1,2}日)|(\d{4}\-\d{2}\-\d{2})/
```

以及一处微小改动
老师最初代码里得到具体新闻作用语句如下，只能爬取http协议的链接，修改后可以爬取http和https开头的链接
```js
 if (href.toLowerCase().indexOf('http://') >= 0) myURL = href;
```
修改为
```js
if (href.toLowerCase().indexOf('http://') >= 0||href.toLowerCase().indexOf('https；://') >= 0) myURL = href;
```
终端运行结果显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210417232844731.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
Navicat显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041800151819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
存在问题：终端有报错，Navicat数据表显示不出老师所给中国新闻网案例完整
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041800133099.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
>当mysql出现”ERROR 1062”错误时：查看字段的属性是否合理，不合理，则修改该字段的属性；合理，则进行表的恢复。

则根据报错内容推测可能是字符编码问题
果然经过对网易新闻网检查，利用ctrl+f搜索功能发现含有gbk和utf-8两种编码形式（而中国新闻网只有utf-8一种编码形式
[查看网页编码方式的指路博客](https://blog.csdn.net/qq_43557623/article/details/111939073)

尝试解决方法
1.将utf-8替换为GBK
```js
var myEncoding = "GBK";
```
终端运行结果相同，但会出现1062 'GBK' 'NULL'报错，且在Navicat上显示出的表格数据为乱码（因为还有utf-8字符编码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418005413448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70#pic_center)
2.myEncoding设为uft-8，且增加一段代码(来自网络，关键字是解决GBK乱码问题
```js
function getUrl(url, handle) {
    http.get(url, (res) => {
        console.log(`status code: ${res.statusCode}`)
        var html = ''
        res.on('data', (chunk) => {
            html += iconv.decode(chunk, 'utf-8')
        })
        res.on('end', () => {
            handle(html)
        })
    }).on('error', (e) => {
        console.log(`http error: ${e.message}`)
    })
}
```
终端仍报错（ html += iconv.decode(chunk, 'GKB')效果近似），但navicat开始显示content
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418010611430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70#pic_center)
利用express在网页中显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210418013545908.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210423143415941.png)

ps.针对乱码问题咨询了学长，给出的建议是在同时存在GBK编码和utf-8编码的情况下，统一采用utf-8编码。


### 代码
```js
var fs = require('fs');
var http = require('http')
var myRequest = require('request');
var myCheerio = require('cheerio');
var myIconv = require('iconv-lite');
require('date-utils');
var mysql = require('./mysql.js');

var source_name = "网易新闻";
var myEncoding = "utf-8";
var seedURL = 'https://news.163.com/';


function getUrl(url, handle) {
    http.get(url, (res) => {
        console.log(`status code: ${res.statusCode}`)
        var html = ''
        res.on('data', (chunk) => {
            html += iconv.decode(chunk, 'utf-8')
        })
        res.on('end', () => {
            handle(html)
        })
    }).on('error', (e) => {
        console.log(`http error: ${e.message}`)
    })
}

var seedURL_format = "$('a')";
var keywords_format = " $('meta[name=\"keywords\"]').eq(0).attr(\"content\")";
var title_format = " $('meta[property=\"og:title\"]').eq(0).attr(\"content\")";
var date_format = "$('.post_time_source').text()"|"$(#ptime).text()";
var author_format = "$('meta[name=\"author\"]').eq(0).attr(\"content\")";
var content_format = "$('#endText').text()";
var desc_format = " $('meta[name=\"description\"]').eq(0).attr(\"content\")";
var source_format = "$('#ne_article_source').text()";
var url_reg = /\/(\d{2})\/(\d{4})\/(\d{2})\/(\w{10,30}).html/;
var regExp = /((\d{4}|\d{2})(\-|\/|\.)\d{1,2}\3\d{1,2})|(\d{4}年\d{1,2}月\d{1,2}日)|(\d{4}\-\d{2}\-\d{2})/

//防止网站屏蔽我们的爬虫
var headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36'
}

//request模块异步fetch url
function request(url, callback) {
    var options = {
        url: url,
        encoding: null,
        //proxy: 'http://x.x.x.x:8080',
        headers: headers,
        timeout: 10000 //
    }
    myRequest(options, callback)
};

seedget();

function seedget() {
    request(seedURL, function(err, res, body) { //读取种子页面
        // try {
        //用iconv转换编码
        var html = myIconv.decode(body, myEncoding);
        //console.log(html);
        //准备用cheerio解析html
        var $ = myCheerio.load(html, { decodeEntities: true });
        // } catch (e) { console.log('读种子页面并转码出错：' + e) };
        var seedurl_news;
        try {
            seedurl_news = eval(seedURL_format);
        } catch (e) { console.log('url列表所处的html块识别出错：' + e) };
        seedurl_news.each(function(i, e) { //遍历种子页面里所有的a链接
            var myURL = "";
            try {
                //得到具体新闻url
                var href = "";
                href = $(e).attr("href");
                if (href == undefined) return;
                if (href.toLowerCase().indexOf('http://') >= 0||href.toLowerCase().indexOf('https；://') >= 0) myURL = href;
                else if (href.startsWith('//')) myURL = 'http:' + href; 
                else myURL = seedURL.substr(0, seedURL.lastIndexOf('/') + 1) + href; //其他

            } catch (e) { console.log('识别种子页面中的新闻链接出错：' + e) }

            if (!url_reg.test(myURL)) return; //检验是否符合新闻url的正则表达式
            //console.log(myURL);

            var fetch_url_Sql = 'select url from fetches where url=?';
            var fetch_url_Sql_Params = [myURL];
            mysql.query(fetch_url_Sql, fetch_url_Sql_Params, function(qerr, vals, fields) {
                newsGet(myURL); //读取新闻页面
            });
        });
    });
};

function newsGet(myURL) { //读取新闻页面
    request(myURL, function(err, res, body) { //读取新闻页面
        //try {
        var html_news = myIconv.decode(body, myEncoding); //用iconv转换编码
        //console.log(html_news);
        //准备用cheerio解析html_news
        var $ = myCheerio.load(html_news, { decodeEntities: true });
        myhtml = html_news;
        //} catch (e) {    console.log('读新闻页面并转码出错：' + e);};

        console.log("转码读取成功:" + myURL);
        //动态执行format字符串，构建json对象准备写入文件或数据库
        var fetch = {};
        fetch.title = "";
        fetch.content = "";
        fetch.publish_date = (new Date()).toFormat("YYYY-MM-DD");
        //fetch.html = myhtml;
        fetch.url = myURL;
        fetch.source_name = source_name;
        fetch.source_encoding = myEncoding; //编码
        fetch.crawltime = new Date();

        if (keywords_format == "") fetch.keywords = source_name; // eval(keywords_format);  //没有关键词就用sourcename
        else fetch.keywords = eval(keywords_format);

        if (title_format == "") fetch.title = ""
        else fetch.title = eval(title_format); //标题

        if (date_format != "") fetch.publish_date = eval(date_format); //刊登日期   
        console.log('date: ' + fetch.publish_date);
        fetch.publish_date = regExp.exec(fetch.publish_date)[0];
        fetch.publish_date = fetch.publish_date.replace('年', '-')
        fetch.publish_date = fetch.publish_date.replace('月', '-')
        fetch.publish_date = fetch.publish_date.replace('日', '')
        fetch.publish_date = new Date(fetch.publish_date).toFormat("YYYY-MM-DD");

        if (author_format == "") fetch.author = source_name; //eval(author_format);  //作者
        else fetch.author = eval(author_format);

        if (content_format == "") fetch.content = "";
        else fetch.content = eval(content_format).replace("\r\n" + fetch.author, ""); //内容,是否要去掉作者信息自行决定

        if (source_format == "") fetch.source = fetch.source_name;
        else fetch.source = eval(source_format).replace("\r\n", ""); //来源

        if (desc_format == "") fetch.desc = fetch.title;
        else fetch.desc = eval(desc_format).replace("\r\n", ""); //摘要    

        // var filename = source_name + "_" + (new Date()).toFormat("YYYY-MM-DD") +
        //     "_" + myURL.substr(myURL.lastIndexOf('/') + 1) + ".json";
        // 存储json
        // fs.writeFileSync(filename, JSON.stringify(fetch));

        var fetchAddSql = 'INSERT INTO fetches(url,source_name,source_encoding,title,' +
            'keywords,author,publish_date,crawltime,content) VALUES(?,?,?,?,?,?,?,?,?)';
        var fetchAddSql_Params = [fetch.url, fetch.source_name, fetch.source_encoding,
            fetch.title, fetch.keywords, fetch.author, fetch.publish_date,
            fetch.crawltime.toFormat("YYYY-MM-DD HH24:MI:SS"), fetch.content
        ];

        //执行sql，数据库中fetch表里的url属性是unique的，不会把重复的url内容写入数据库
        mysql.query(fetchAddSql, fetchAddSql_Params, function(qerr, vals, fields) {
            if (qerr) {
                console.log(qerr);
            }
        }); //mysql写入
    });
}
```
### 热度分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427173840365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 小白财经
### 分析
在我的数据库里新建表格,注意这里没有设置唯一约束，所以可以重复爬取。（设置后因未知原因报错故出此下策
```sql
CREATE TABLE if not exists `xiaobai` (
  `id_xiaobai` int(11)  NOT NULL AUTO_INCREMENT,
  `url` varchar(200) DEFAULT NULL,
  `source_name` text DEFAULT NULL,
  `source_encoding` text DEFAULT NULL,
  `title` text DEFAULT NULL,
  `publish_date` text DEFAULT NULL,
  `crawltime` text DEFAULT NULL,
  `content` text DEFAULT NULL,
  `createtime` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id_xiaobai`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
navicat显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427160051710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
这里我们修改一下查询方式，不再使用查询标题，改为查询来源，并且修改在网页显示的内容（因为小白财经没有作者
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427162223526.png)
前端显示修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427162346732.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427162406776.png)
浏览器显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042716282936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
### 代码
```javascript
var fs = require('fs');
var myRequest = require('request');
var myCheerio = require('cheerio');
var myIconv = require('iconv-lite');
require('date-utils');
var mysql = require('./mysql.js');

var source_name = "小白财经";
var domain = 'http://www.xiaobaicj.cn/';
var myEncoding = "utf-8";
var seedURL = 'http://www.xiaobaicj.cn/';

var seedURL_format = "$('a')";
var url_reg = /\/(\w{1})(\d{5}).html/;

//防止网站屏蔽我们的爬虫
var headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36'
}

//request模块异步fetch url
function request(url, callback) {
    var options = {
        url: url,
        encoding: null,
        //proxy: 'http://x.x.x.x:8080',
        headers: headers,
        timeout: 10000 //
    }
    myRequest(options, callback)
};

seedget();

function seedget() {
    request(seedURL, function(err, res, body) { //读取种子页面
        // try {
        //用iconv转换编码
        var html = myIconv.decode(body, myEncoding);
        //console.log(html);
        //准备用cheerio解析html
        var $ = myCheerio.load(html, { decodeEntities: true });
        // } catch (e) { console.log('读种子页面并转码出错：' + e) };
        var seedurl_news;
        try {
            seedurl_news = eval(seedURL_format);
        } catch (e) { console.log('url列表所处的html块识别出错：' + e) };
        seedurl_news.each(function(i, e) { //遍历种子页面里所有的a链接
            var myURL = "";
            try {
                //得到具体新闻url
                var href = "";
                href = $(e).attr("href");
                if (href == undefined) return;
                if (href.toLowerCase().indexOf('http://') >= 0||href.toLowerCase().indexOf('https；://') >= 0) myURL = href;
                //http://开头的，之前这个文件虽然也能爬，但是会报错，原因是只爬了http开头的网址，现在加了https开头的网址，就好了
                else if (href.startsWith('//')) myURL = 'http:' + href; ////开头的
                else myURL = seedURL.substr(0, seedURL.lastIndexOf('/') + 1) + href; //其他

            } catch (e) { console.log('识别种子页面中的新闻链接出错：' + e) }

            if (!url_reg.test(myURL)) return; //检验是否符合新闻url的正则表达式
            //console.log(myURL);

            var fetch_url_Sql = 'select url from fetches where url=?';
            var fetch_url_Sql_Params = [myURL];
            mysql.query(fetch_url_Sql, fetch_url_Sql_Params, function(qerr, vals, fields) {
                if (vals.length > 0) {
                    console.log('URL duplicate!')
                } else newsGet(myURL); //读取新闻页面
            });
        });
    });
};

function newsGet(myURL) { //读取新闻页面
    request(myURL, function(err, res, body) { //读取新闻页面
        //try {
        var html_news = myIconv.decode(body, myEncoding); //用iconv转换编码
        //console.log(html_news);
        //准备用cheerio解析html_news
        var $ = myCheerio.load(html_news, { decodeEntities: true });
        myhtml = html_news;
        //} catch (e) {    console.log('读新闻页面并转码出错：' + e);};

        console.log("转码读取成功:" + myURL);
        //动态执行format字符串，构建json对象准备写入文件或数据库
        var fetch = {};
        fetch.title = $('h1[class="entry-title"]').text();
        if (fetch.title == "") fetch.title = "无标题";
        fetch.content = $('div[class="entry-content"]').text();
        fetch.publish_date = $('span[class="entry-date"]').text();
        fetch.url = myURL;
        fetch.source_name = source_name;
        fetch.source_encoding = myEncoding; //编码
        fetch.crawltime = new Date();

 

        var fetchAddSql = 'INSERT INTO xiaobai(url,source_name,source_encoding,title,publish_date,crawltime,content) VALUES(?,?,?,?,?,?,?)';
        var fetchAddSql_Params = [fetch.url, fetch.source_name, fetch.source_encoding,
            fetch.title, fetch.publish_date,fetch.crawltime.toFormat("YYYY-MM-DD HH24:MI:SS"), fetch.content
        ];

        //执行sql，数据库中fetch表里的url属性是unique的，不会把重复的url内容写入数据库
        mysql.query(fetchAddSql, fetchAddSql_Params, function(qerr, vals, fields) {
            if (qerr) {
                console.log(qerr);
            }
        }); //mysql写入
    });
}
```
### 热度分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427173925991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 东方财富网
### 分析
navicat显示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427190332574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)

浏览器显示结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427190221948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
### 代码
```js
var fs = require('fs');
var myRequest = require('request');
var myCheerio = require('cheerio');
var myIconv = require('iconv-lite');
require('date-utils');
var mysql = require('./mysql.js');

var source_name = "东方财富网";
var myEncoding = "utf-8";
var seedURL = 'https://www.eastmoney.com/';
var seedURL_format = "$('a')";
var url_reg = /\/(\w{1})\/(\d{18}).html/;

//防止网站屏蔽我们的爬虫
var headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.65 Safari/537.36'
}

//request模块异步fetch url
function request(url, callback) {
    var options = {
        url: url,
        encoding: null,
        //proxy: 'http://x.x.x.x:8080',
        headers: headers,
        timeout: 10000 //
    }
    myRequest(options, callback)
};

seedget();

function seedget() {
    request(seedURL, function(err, res, body) { //读取种子页面
        // try {
        //用iconv转换编码
        var html = myIconv.decode(body, myEncoding);
        //console.log(html);
        //准备用cheerio解析html
        var $ = myCheerio.load(html, { decodeEntities: true });
        // } catch (e) { console.log('读种子页面并转码出错：' + e) };
        var seedurl_news;
        try {
            seedurl_news = eval(seedURL_format);
        } catch (e) { console.log('url列表所处的html块识别出错：' + e) };
        seedurl_news.each(function(i, e) { //遍历种子页面里所有的a链接
            var myURL = "";
            try {
                //得到具体新闻url
                var href = "";
                href = $(e).attr("href");
                if (href == undefined) return;
                if (href.toLowerCase().indexOf('http://') >= 0||href.toLowerCase().indexOf('https；://') >= 0) myURL = href;
                //http://开头的，之前这个文件虽然也能爬，但是会报错，原因是只爬了http开头的网址，现在加了https开头的网址，就好了
                else if (href.startsWith('//')) myURL = 'http:' + href; ////开头的
                else myURL = seedURL.substr(0, seedURL.lastIndexOf('/') + 1) + href; //其他

            } catch (e) { console.log('识别种子页面中的新闻链接出错：' + e) }

            if (!url_reg.test(myURL)) return; //检验是否符合新闻url的正则表达式
            //console.log(myURL);

            var fetch_url_Sql = 'select url from fetches where url=?';
            var fetch_url_Sql_Params = [myURL];
            mysql.query(fetch_url_Sql, fetch_url_Sql_Params, function(qerr, vals, fields) {
                if (vals.length > 0) {
                    console.log('URL duplicate!')
                } else newsGet(myURL); //读取新闻页面
            });
        });
    });
};

function newsGet(myURL) { //读取新闻页面
    request(myURL, function(err, res, body) { //读取新闻页面
        //try {
        var html_news = myIconv.decode(body, myEncoding); //用iconv转换编码
        //console.log(html_news);
        //准备用cheerio解析html_news
        var $ = myCheerio.load(html_news, { decodeEntities: true });
        myhtml = html_news;
        //} catch (e) {    console.log('读新闻页面并转码出错：' + e);};

        console.log("转码读取成功:" + myURL);
        //动态执行format字符串，构建json对象准备写入文件或数据库
        var fetch = {};
        fetch.url = myURL;
        fetch.source_encoding = myEncoding; //编码
        fetch.crawltime = new Date();
        fetch.title = $('div[class="newsContent"]').children("h1").text();
        if (fetch.title == "") fetch.title = "无标题";
        fetch.content = $('div[id="ContentBody"]').children("p").text();

        /*fetch.source_name=$("div.source data-source").attr("data-source");
        fetch.source_name = $('span').text();*/
        fetch.source_name =source_name;
        fetch.publish_date = $("div.time").text();
        fetch.keywords = $('meta[name="keywords"]').attr("content");
        fetch.author=$("div.author").text();
      
        var fetchAddSql = 'INSERT INTO fetches(url,source_name,source_encoding,title,' +
            'keywords,author,publish_date,crawltime,content) VALUES(?,?,?,?,?,?,?,?,?)';
        var fetchAddSql_Params = [fetch.url, fetch.source_name, fetch.source_encoding,
            fetch.title, fetch.keywords, fetch.author,fetch.publish_date,fetch.crawltime.toFormat("YYYY-MM-DD HH24:MI:SS"), fetch.content
        ];

        //执行sql，数据库中fetch表里的url属性是unique的，不会把重复的url内容写入数据库
        mysql.query(fetchAddSql, fetchAddSql_Params, function(qerr, vals, fields) {
            if (qerr) {
                console.log(qerr);
            }
        }); //mysql写入
    });
}
```
### 热度分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427190429440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
# 前端进一步美化
## 效果展示
之前在网页中插入了蜘蛛网的动态背景，依旧感觉有所欠缺，故以雪球网为例进行了进一步美化
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430121445571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430121535990.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
也可以再增加这样的表格
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210430121651219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
## 代码
后端也要做对应修改，这里因篇幅限制不多做展示
```html
<!DOCTYPE html>
<html>
<header>
    
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
</header>

<body>
    <body background=".\心水2.jpg" style=" background-repeat:no-repeat ;background-size:100% 100%;
background-attachment: fixed;">

    <form>
        <br> 请输入查询内容：<input type="text"  name="title_text" style="width: 100px; height: 30px" > 
        <input
          class="form-submit"
          type="button"
          value="查询"
          style="
            width: 100px;
            height: 34px;
          "
        />
        <input
          type="reset"
          value="清空"
          style="
            width: 100px;
            height: 34px;
          "
        />
        搜索范围：
        <input type="checkbox" class="option1">标题
        <input type="checkbox" class="option2">作者
        <input type="checkbox" class="option3">内容
    </form>
    <div class="cardLayout" style="margin: 10px 0px">
        <table border="1" style="border-color: rgba(1, 1, 0, 0.2);" width="100%" id="record2"></table>
    </div>
    <script>
        $(document).ready(function() {
            $("input:button").click(function() {
                $.get('/process_get?title=' + $("input:text").val(), function(data) {
                    $("#record2").empty();
                    /*$("#record2").append('<tr class="cardLayout"><td>url</td><td>source_name</td>' +
                        '<td>title</td><td>author</td><td>publish_date</td></tr>');*/
                        $("#record2").append('<tr class="cardLayout"><td>网址</td><td>来源</td>' +
                        '<td>标题</td><td>作者</td><td>发表时间</td></tr>');
                    for (let list of data) {
                        let table = '<tr class="cardLayout"><td>';
                        Object.values(list).forEach(element => {
                            table += (element + '</td><td>');
                        });
                        $("#record2").append(table + '</td></tr>');
                    }
                });
            });

        });
    </script>
</body>

</html>
```



# 词云实现热度分析
作业附加要求：完成对搜索内容的时间热度分析，比如搜索“新冠”，可以展示爬取数据内容中每一天包含“新冠”的条数，具体展示形式不限，可以用文字或表格展示，也可以用图表展示。

这里采用词云的方式实现，源码来自，需要使用python
[在vscode上使用python的配置](https://zhuanlan.zhihu.com/p/66157046)
[词云使用](https://godweiyang.com/2019/07/27/wordcloud/)
[词云源码](https://github.com/godweiyang/wordcloud)
初步效果展示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427124940133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427125010193.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
在前端html文件里增加如下语句，即可实现点击按钮看词云图片的功能
```html
<br />
    <a href="/文件所在完整路径\新闻.png"><button>词云</button></a>
<br />
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210427131025863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MTA3Njg3OQ==,size_16,color_FFFFFF,t_70)
点击词云按钮即可在浏览器显示这张图片

# 心得总结
本次期中爬虫作业到这里就暂时告一段落。

对于一个只有c语言基础的菜鸟来说，入门web编程的确存在着艰巨的挑战。一开始想从零开始一步步学懂js和html语法，再完成期中作业，但随着计划的进行发现语法数量繁杂，且只进行机械学习而不去应用很难记住，化为自用。

且由于其语法具有繁多而又较为简单的特点，比起一味的学习不如在事件中现学现用。所以采取了“下楼梯学习法”，先研究老师给出的爬虫示例，尽最大努力完成这项艰巨的挑战，再进行更低阶内容的巩固。

在后续的过程中了解到了模块引入、正则表达式、网站响应等基础用法，并且经历了爬虫数据“显示在终端中”->"保存在本地文件中“->”保存在数据库中“->”在浏览器中显示“->“终端美化，增加动态背景图和边框”这五个阶段，其中尤为困难的是正则表达式、jquery语法的学习和爬虫语句整体的理解。

虽然实现的爬虫展示依旧简单，但经过一段时间的学习我逐渐走出了一头雾水的状况，对如何学习web有了一定的体悟。感谢老师的指导讲解，学长学姐的帮助，相信假以时日会做出更为优化的页面。期待下一次项目的完成。

