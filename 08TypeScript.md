TypeScript是由微软开发开源的编程语言，属于JavaScript的扩展，支持ES6标准，可以编译成纯JavaScript。相较于js拥有了类型检查，，尽早的发现问题。

1. [安装使用](https://www.runoob.com/typescript/ts-install.html)

```shell
npm install typescript -g

tsc -v

tsc --init	//生成tsconfig.json文件

```

tsconfig.json

```json
{
  "compilerOptions": {
   "target": "es5",
   "noImplicitAny": false,
   "module": "amd",
   "removeComments": false,
   "sourceMap": false,
   "outDir": "src/js"//你要生成js的目录
  }
}
```

```
tsc -w	//监控ts文件变化
```

2. 使用：TypeScript会忽略程序中出现的空格、制表符和换行符。常用于缩进代码，便于阅读。<font color="red">面向对象</font>.
   - 模块
   - 函数
   - 变量
   - 语句和表达式
   - 注释

循环：

```javascript
// 以下均为typescript写法

// 1.简单for遍历
var test:string = "1 2 3 4";
for(let i = 0; i < test.length; i++) {
    console.log(test[i])
}

// 2.for...in循环：val只能string或any类型
for (var val in list) { 
    //语句 
}

// 3.for...of：可遍历多种数据结构
let someArray = [1, "string", false];
 
for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}

// 4.forEach：在执行完前不能返回
let list = [4, 5, 6];
list.forEach((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
});

// 5.every 和 some 循环
let list = [4, 5, 6];
list.every((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
    return true; // Continues
    // Return false will quit the iteration
});
```

接口、类、访问控制修饰符、对象