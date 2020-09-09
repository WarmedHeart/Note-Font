# display属性

1. inline：内联元素，没有宽高属性，不能独占一行。

2. block：块级元素，具有宽高属性，并且独占一行。

3. inline-block：内联块级元素，可以设置宽高、内联元素。（并列排列时可选择，不过用浮动居多）

4. none：不显示该元素，不占据原空间。

   `visibility: hidden;` 会占据原空间；`opactity:0;`透明度为0，会占据原空间。

5. flex：弹性盒

# float浮动

> 浮动后会脱离文本框，会盖住后面非浮动元素。**使用后记得清除浮动。**
>
> float:left/right/none;（只有 横向浮动，没有纵向浮动）
>
> clear:left/right/both;

1. 任何元素（块级、行级）设置浮动 ` float`  后，**` display` 属性将完全失效**均可设置宽高，并且不会独占一行.
2. 可以设置z-index元素的堆叠顺序，大的在上边。
3. 清除浮动：是在使用了浮动必不可少的。
   1. 浮动元素的父元素定宽高。
   2. 结尾处加空div设置 `clear:both;`
   3. 浮动元素的父div设置：`overflow:hidden`;

建议：

1. 并列排列时使用。想要设置左右距离，使用盒子模型的margin来设置。
2. 水平垂直居中（原位置会占用）：`float: left; position: relative; left:50%; top:50%; transform: translate(-50%, -50%); `  <span><font color="red">弊端：使用的相对定位，原位置会一直被占用。</font></span>
3. 外层套带有宽度的div，设置好定位（使用盒子居中或者position定位），**浮动元素外层嵌套的div宽度应该设置为浮动元素的宽度之和**。



# position定位

static：默认值。元素会出现在正常流中。（会忽略top、bottom、left、right、z-index声明）

absolute：相对父级（不是static默认值即可）定位，父级没有设置定位（static默认值）会向上层继续查找。（可使用top、bottom、left、right、z-index声明）

relative：绝对定位（以原位置定位），使用top等属性，原位置会被占用。**一般配合子级		`position:absolute;`，父级设置`position:relative;`**（可使用top、bottom、left、right、z-index声明）

fixed：固定定位。（可使用top、bottom、left、right、z-index声明）



建议：

1. 只有设置了position才能使用top、bottom、left、right属性，会脱离文本框。
2. 水平垂直居中：`position:absolute; top:50%; left:50%; transform: translate(-50%,-50%);`（兼容性不好）
3. 水平垂直居中：`position:absolute; top:0; right:0; bottom:0; left:0; margin:auto;`（兼容性好）

# 盒子模型

1. 内容（content）：设置的宽高属于内容。
2. 内边距（padding）
3. 边框（border）
4. 外边距（margin）:
   
   1. 同级外边距上下方向合并以最大的作为两者边距；左右边距不会合并
   
   

建议（以下均为普通文本流，浮动后不起作用）：

	1. 块级元素水平居中：`margin:0 auto;`
 	2. 内联元素水平垂直居中：`line-height:(父级高度); text-align:center;`



# flex布局 +  rem自适应

属性参考：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html



# Grid网格布局

属性参考：http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html

### 容器属性

```
display: grid | inline-grid;
// 3 * 3 网格
grid-template-columns:repeat(3, 1fr);
grid-template-rows:repeat(3, 1fr);
// 间距
grid-gap: 20px 20px;
// 区域名称
grid-template-areas: 'header header header'
					 'main main sidebar'
					 'footer footer footer'
// 容器子元素排列顺序row | column | row dense | column dense
grid-auto-flow: row
// 单元格中内容(内容没有撑满单元格)排列顺序 start | end | center | stretch
place-items: start end ;
// 网格所处位置
place-content: space-around space-evenly;

```

### 项目属性

```
// 单元格所占位置
//grid-column-start: span 2;所占单元格个数
grid-column: 1 / 3; 网格线起始位置
//grid-row-start: span 2;
grid-row-start:1 / 2;
```

### 布局实例

[css布局练习总结]: https://www.jianshu.com/p/6c41d9068b73

