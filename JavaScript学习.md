## 一、ECMAScript 基本语法

ECMAScript  是 JavaScript 标准的规范。

### （一）变量的命名

| 类型                   | 前缀 | 示例      |
| ---------------------- | ---- | --------- |
| 数组                   | a    | aValues   |
| 布尔型                 | b    | bFound    |
| 浮点型（数字）         | f    | fValue    |
| 函数                   | fn   | fnMethod  |
| 整型（数字）           | i    | iValue    |
| 对象                   | o    | oType     |
| 正则表达式             | re   | rePattern |
| 字符串                 | s    | sValue    |
| 变型（可以是任何类型） | v    | vValue    |

### （二）变量类型

**值类型(基本类型)**：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）

**引用数据类型**：对象(Object)、数组(Array)、函数(Function)



1. 检查变量或值的类型：

   ```javascript
   typeof obj;	//引用类型全部为object。特例：JS中null被认为是对象的占位符：object - 如果变量是一种引用类型或Null类型
   
   obj instanceof People;	//引用类型可以明确知道是哪种特定对象。
   ```

2. Function

   ```javascript
   function testFunc() {
   }
   //当函数没明确返回值，返回的也是值"undefined"
   alert(testFunc() == undefined);  //输出 "true"
   ```

3. Number

   **对于浮点字面量的有趣之处在于，用它进行计算前，真正存储的是字符串。**

   ```javascript
   isFinite();	// 数值类型判断是否是无穷大
   
   isNaN();	// 判断非数（is Not a Number），更正常思维相反
   
   toFixed();	//方法返回的是具有指定位数小数的数字的字符串表示
   ```

4. String

   **它是唯一没有固定大小的原始类型。**

   ```javascript
   slice()/substring();	//用来截取字符串，在对负值处理上不同，substring将其作为0处理。
   ```

   **String拼接：**

   ```javascript
   var str = "hello ";
   str += "world";
   ```

   执行步骤如下：

   1. 创建存储 "hello " 的字符串。
   2. 创建存储 "world" 的字符串。
   3. 创建存储连接结果的字符串。
   4. 把 str 的当前内容复制到结果中。
   5. 把 "world" 复制到结果中。
   6. 更新 str，使它指向结果。

   每次完成字符串连接都会执行步骤 2 到 6；重复多次会非常消耗资源，造成性能问题。

   **优化1：**用 Array 对象存储字符串，然后用 join() 方法（参数是空字符串）创建最后的字符串

   ```javascript
   var arr = new Array();
   arr[0] = "hello ";
   arr[1] = "world";
   var str = arr.join("");
   ```

   数组中无论引入多少字符串都没问题，只有在执行join()方法才会发生连接操作。执行步骤如下：

   1. 创建存储结果的字符串
   2. 把每个字符串复制到结果中的合适位置

   **优化2：**对优化1进行封装

   ```javascript
   // 自定义函数
   function StringBuffer () {
     this._strings_ = new Array();
   }
   
   StringBuffer.prototype.append = function(str) {
     this._strings_.push(str);
   };
   
   StringBuffer.prototype.toString = function() {
     return this._strings_.join("");
   };
   ```

   ```javascript
   // 使用
   var buffer = new StringBuffer ();
   buffer.append("hello ");
   buffer.append("world");
   var result = buffer.toString();
   ```

   测试：当循环10000000次，直接+ 耗时1152ms，append耗时348ms

5. Object

   ECMAScript中的**所有对象都由Object对象继承而来**,Object对象中的所有属性和方法都会出现在其他对象中。

   Object属性：

   ```
   constructor：对创建对象的函数的引用（指针）。对于Object对象，该指针指向原始的Object()函数。
   
   Prototype：对该对象的对象原型的引用。对于所有的对象，它默认返回Object对象的一个实例。
   ```

   Object方法：

   ```
   hasOwnProperty(property);	判断对象是否有某个特定的属性。必须用字符串指定该属性。（例如，o.hasOwnProperty("name")）
   
   IsPrototypeOf(object);	判断该对象是否为另一个对象的原型。
   
   PropertyIsEnumerable;	判断给定的属性是否可以用for...in语句进行枚举。
   
   ToString();	返回对象的原始字符串表示。
   
   ValueOf();	返回最适合该对象的原始值。对于许多对象，该方法返回的值都与ToString()的返回值相同。
   ```

### （三）运算符

#### 位运算符

 1. 有符号整数（用户默认使用）：正数储存为原码，负数储存为补码。最高位32位为符号标识位，ECMAScript规定开发者不能访问符号位，所以在显示负整数的二进制字符串时，不以补码形式，而是数字绝对值的标准二进制前面加负号形式输出。

    ```javascript
    var iNum = -18;
    iNum.toString(2);	//输出 "-10010"
    ```

 2. 无符号整数：只有ECMAScript的位运算符才能创建无符号整数

### （四）语句

​	if/while/for/for-in/switch

### （五）函数

​	[参考文档](https://www.jianshu.com/p/80a2fb877168)

1. 函数内部特殊对象：arguments数组代表传入参数值，可不写参数。

2. **ECMAScript 的函数实际上是功能完整的对象。**Function 对象（类），其中的**原型property**属性使用较多。

3. 对象方法、类方法、原型方法

   ```javascript
   //声明
   // 1. 函数
   function test() {}	//普通函数
   // 2. 方法：当一个函数被保存为一个对象的属性时，我们称之为方法
   var test = function fn(){}	// 函数表达式：匿名函数（拉姆达表达式）,fn()将作为内部变量fn调用
   							// 相当于：var test ={ fn:  function()}
   //函数调用
   test();
   
   ```

   

### （六）对象

​	1. 用完一个对象后，就将其废除o，来释放内存，这是个好习惯。object = null;

2. ECMAScript只支持晚绑定（late binding）指的是编译器或解释程序在运行前，不知道对象的类型。使用晚绑定，无需检查对象的类型，只需检查对象是否支持属性和方法即可。ECMAScript 中的所有变量都采用晚绑定方法。这样就允许执行大量的对象操作，而无任何惩罚。

3. ECMAScript只有公用作用域（没有私有），开发者统一规约在属性前后加下划线：

   ```javascript
   obj._color_ = "blue";
   ```

   注意，下划线并不改变属性是公用属性的事实，它只是告诉其他开发者，应该把该属性看作私有的。

4. **this关键字**：总是指向调用该方法的对象。

5. 定义类的方式演变至今较为成熟的方式。

   ```javascript
   // 1. 随用随定义
   var obj = new Object();
   obj.name = "hang";
   obj.age = '24';
   obj.printName = function() {
   	console.log(this.name);
   }
   // 问题：可能需要创建多个obj对象
   // 2. 工厂方式
   function createObj(mName, mAge) {
   	var obj = new Object();
       obj.name = mName;
       obj.age = mAge;
       obj.printName = function() {
           console.log(this.name);
       };
       return obj;
   }
   // 问题：每次调用createObi()方法都要创建新函数printName(),造成内存浪费。而事实上，应该是每个对象共	享同一个函数。
   // 3. 外部定义方法
   function printName() {
       console.log(this.name);
   }
   function createObj(mName, mAge) {
   	var obj = new Object();
       obj.name = mName;
       obj.age = mAge;
       obj.printName = printName();
       return obj;
   }
   // 问题：功能上：解决了重复创建函数对象的问题。语义上：该函数不太像是对象中的方法。
   // 4. 构造函数
   function Obj(mName, mAge) {
       this.name = mName;	//对象属性
       this.age = mAge;
       this.like = new Array['play', 'learn'];
       this.printName = function() {	//对象方法
           console.log(this.name);
       };
   }
   // 问题：还是存在2中重复创建函数对象的问题。也可用外部定义方法解决
   // 5.原型方式: 包含函数实例共享的方法和属性。函数运行时会先去本体的函数中去，没有会到prototype中	去找。
   function Obj() {  
   }
   Obj.prototype.name = 'hang';
   Obj.prototype.age = '24';
   Obj.prototype.like = new Array['play', 'learn'];
   Obj.prototype.printName = function() {	//原型方法
       console.log(this.name);
   };
   // 问题：当对象在原型中创建引用对象（此例中的like数组），创建的全部Obj对象的like指针不同，可是指向的	堆内存是同一块区域，即全部对象共享like属性。
   // 6.混合的构造函数/原型方法
   function Obj(mName, mAge) {
       this.name = mName;
       this.age = mAge;
       this.like = new Array['play', 'learn'];
   }
   Obj.prototype.printName = function() {
       console.log(this.name);
   };
   // 感觉还不是很完美
   // 7.动态原型方法	！！！！！！ (较为完善) ！！！！！！
   function Obj(mName, mAge) {
       this.name = mName;
       this.age = mAge;
       this.like = new Array['play', 'learn'];
       if(typeOf Obj.initialized == 'undefined') {
           Obj.prototype.printName = function() {
               console.log(this.name);
           };
       }
       Obj._initialized_ = true;
   }
   ```

   

### （七）继承机制



### （八）ES6（未看）



## 二、DOM编程

document object model文档对象模型，通过JS操作document进而动态修改html元素。

```javascript
// 获取html节点
document.getElementById("intro");

document.getElementsByTagName("p");

document.getElementsByClassName("intro");

//修改HTML内容、样式、元素
document.getElementById("p1").innerHTML="新文本!";

document.getElementById("p2").style.color="blue";

var para=document.createElement("p");
var node=document.createTextNode("这是一个新段落。");
para.appendChild(node);
var element=document.getElementById("div1");
element.appendChild(para);

parent.removeChild(child);

element.insertBefore(para,child);

parent.replaceChild(para,child);
//事件
onclick/onload/onunload/onchange/onmouseover/onmouseout/onmousedown/onmouseup

//获取根节点
document.documentElement - 全部文档
document.body - 文档的主体

```

## 三、BOM对象

Browser Object Model 浏览器对象模型，核心对象window。一个 window 对象实际上就是一个独立的窗口。

（一）全局作用域

​	window 是全局对象，因此所有的全局变量都被解析为该对象的属性。

```javascript
var a = "window.a";  //全局变量
function f () {  //全局函数
    console.log(a);
}
console.log(window.a);  //返回字符串“window.a”
window.f();  //返回字符串“window.a”
```

（二）客户端对象

- window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
- navigator：包含客户端有关浏览器信息。
- screen：包含客户端屏幕的信息。
- history：包含浏览器窗口访问过的 URL 信息。
- location：包含当前网页文档的 URL 信息。
- document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

**window**

1. 系统对话框

- alert()
- confirm()
- prompt()

2. 打开和关闭窗口

-  open() 
- 新创建的 window 对象拥有一个 opener 属性，引用打开它的原始对象
- close()

3. 定时器

- setInterval()：按照执行的**周期**（单位为毫秒）调用函数或计算表达式
- setTimeout()： 在指定的毫秒数后调用函数或计算表达式
- clearInterval()
- clearTimeout()

**navigator**：对象存储了与浏览器相关的基本信息，如名称、版本和系统等.window.navigator

**location**：对象存储了当前文档位置（URL）相关的信息，简单地说就是网页地址字符串。window.location

| 属性     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| href     | 声明了当前显示文档的完整 URL，与其他 location 属性只声明部分 URL 不同，把该属性设置为新的 URL 会使浏览器读取并显示新 URL 的内容 |
| protocol | 声明了 URL 的协议部分，包括后缀的冒号。例如：“http:”         |
| host     | 声明了当前 URL 中的主机名和端口部分。例如：“www.123.cn:80”   |
| hostname | 声明了当前 URL 中的主机名。例如：“www.123.cn”                |
| port     | 声明了当前 URL 的端口部分。例如：“80”                        |
| pathname | 声明了当前 URL的路径部分。例如：“news/index.asp”             |
| search   | 声明了当前 URL 的查询部分，包括前导问号。例如：“?id=123&name=location” |
| hash     | 声明了当前 URL 中锚部分，包括前导符（#）。例如：“#top”,指定在文档中锚记的名称 |

**history**：存储最近访问的、有限条目的 URL 信息。

```javascript
window.history.back(); / window.history.go(-1);
window.history.forward(); / window.history.go(1); 

window.history.length;

```

**screen**：客户端屏幕信息

**document**：

```javascript
document.documentElement.clientWidth;
document.documentElement.clientHeight;
```

### （四）作用域、闭包、原型链

作用域：全局作用域、局部作用域、块级作用域: (function(){})()

作用域链：变量会顺着父级一级一级向上搜索，直到找到为止，找不到就报错。

闭包：js为了实现数据和方法私有化的方式。原理：内层函数可以调用外层函数的变量和方法。

比如：实现每进入函数一次变量temp 加1

```javascript
// 1.定义全局变量
var temp = 0;
function incTemp() {
	return ++temp;
}
incTemp();
// 弊端：定义的全局变量，所有函数都可以看到使用

// 2.闭包
function addWarp() {
    var temp = 0;
    return function() {
    	return ++temp;
    }
}
var incTemp = addWarp();
incTemp();
```

