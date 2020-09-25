# 使用rem

1. 引入 [flexible.js](vscode中使用rem\flexible.js) 
2. 浏览器f12查看html元素 font-size是多少px
3. vscode引入插件cssrem
4. 在扩展中找到cssrem -- 管理 -- 扩展设置-- 修改用户Cssrem:Root Font Size的值为上面font-size的值。
5. <span style="color: red">重启vscode生效</span>

> ​	以上修改只是以当前屏幕尺寸为基准计算的1rem = *px，设置好布局后，修改屏幕尺寸rem也会随之变化，以达到自适应屏幕。



# 构建webpack

```shell
npm init	//初始化包描述文件，输入包名称，以及其他信息，最终会生成package.json文件

npm i webpack webpack-cli -g	//全局安装，以便于使用webpack命令

npm i webpack webpack-cli -dev	//本地安装开发依赖

webpack ./src/index.js -o ./build/built.js --mode=development	//webpack会以./src/index.js作为入口文件打包，打包后输出到./build/built.js，整体打包环境是开发环境
```

```javascript
//src/index.js

/**
 * index.js：webpack入口起点文件
 * 1.运行指令，测试webpack能否使用：
 *  开发命令：webpack ./src/index.js -o ./build/built.js --mode=development
 *  webpack会以./src/index.js作为入口文件打包，打包后输出到./build/built.js，整体打包环境是开发环境
 * 2.结论：
 *  2.1 webpack可以打包js/json，不能处理css/img等其他资源。
 *  2.2 生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化。
 *  2.3 生产环境比开发环境多一个压缩js代码。
 */

 function add(a, b) {
     return a+b;
 }
 console.log(add(1,2));
```

<a href="问题/webpack命令不能使用.md">webpack命令不能使用原因</a>

