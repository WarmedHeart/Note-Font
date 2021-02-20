# 包管理

### （一）npm

​	由node官方维护，下载较慢，npm是随同node.js一同安装的包管理工具。<a href="npm全局  局部安装.md">安装教程</a>

```
	npm -v	//查看版本号
	npm init //文件生成package.json
```

### （二）cnpm

阿里巴巴维护，解决下载慢。

```
npm install -g cnpm --registry=https://registry.npm.taobao.org	//安装

cnpm -v	//查看版本号
```

### （二）yarn

facebook维护。

### （三）npx

是npm5.2之后发布的一个命令。**不用全局安装，使用项目中安装的直接在命令行执行命令。**

# 常用包

### （一）fs读写文件

```javascript
let fs = require("fs");	//引入fs模块

//读取文件
function readFile() {
    // 同步读取
    var data = fs.readFileSync('read.txt', 'utf-8');
    console.log("同步读取: " + data.toString());
    // 异步读取
    fs.readFile('read.txt', 'utf-8', function (err, data) {
        if (err) {
            return console.error(err);
        }
        console.log("异步读取: " + data.toString());
    });
}
//写入文件
function writeFile() {
    // 同步读取
    var data = fs.writeFileSync('write1.txt', '我是被写入的内容1！');
    var writeData1 = fs.readFileSync('write1.txt', 'utf-8');
    console.log("同步读取写入的内容1: " + writeData1.toString());
    // 异步读取
    fs.writeFile('write2.txt', '我是被写入的内容2！', function (err) {
        if (err) {
            return console.error(err);
        }
        var writeData2 = fs.readFileSync('write2.txt', 'utf-8');
        console.log("同步读取写入的内容2: " + writeData2.toString());
    });
}
```

### （二）路径查找path

```javascript
var path = require("path")	//引入path模块

// 格式化路径
console.log('normalization : ' + path.normalize('/test/test1//2slashes/1slash/tab/..'));

// 连接路径
console.log('joint path : ' + path.join('/test', 'test1', '2slashes/1slash', 'tab', '..'));

// 转换为绝对路径
console.log('resolve : ' + path.resolve('main.js'));

// 路径中文件的后缀名
console.log('ext name : ' + path.extname('main.js'));
```

```
输出：
normalization : /test/test1/2slashes/1slash
joint path : /test/test1/2slashes/1slash
resolve : /web/com/1427176256_27423/main.js
ext name : .js
```

### （三）[网络http](https://www.jianshu.com/p/b5973ef87f73)

```
var http = require("http");	//引入http模块
```

1. http

   ```javascript
   // http_demo.js
   var http=require("http");
   
   http.createServer(function(req,res){
       res.writeHead(200,{
           "content-type":"text/plain"
       });
       res.write("hello world");
       res.end();
   }).listen(3000);
   
   $~ node http_demo.js
   ```

2. http server

   ```javascript
   var http=require("http");
   var server=new http.Server();
   
   server.on("request",function(req,res){
       res.writeHead(200,{
           "content-type":"text/plain"
       });
       res.write("hello nodejs");
       res.end();
   });
   server.listen(3000);
   ```

   以上req是`http.IncomingMessage`的实例，res是`http.ServerResponse`的实例。

   - req 为http请求信息。
   - res 为返回给客户端的响应信息。

3. http client

   `http.request`和 `http.get`，功能是作为客户端向http服务器发起请求

<a href="Charles抓包.md">抓包工具</a>

