# 步骤

1. 引入 [flexible.js](vscode中使用rem\flexible.js) 
2. 浏览器f12查看html元素 font-size是多少px
3. vscode引入插件cssrem
4. 在扩展中找到cssrem -- 管理 -- 扩展设置-- 修改用户Cssrem:Root Font Size的值为上面font-size的值。
5. <span style="color: red">重启vscode生效</span>

> ​	以上修改只是以当前屏幕尺寸为基准计算的1rem = *px，设置好布局后，修改屏幕尺寸rem也会随之变化，以达到自适应屏幕。