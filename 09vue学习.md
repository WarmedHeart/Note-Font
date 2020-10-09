前端规范：缩进2个空格

### 指令

- **v-once**	不会随着变量值变化而变化

- **v-html = “ ”**

- **v-pre**     不会解析内部{{message}}

- **v-cloak**   该指令可以隐藏未编译的双大括号{{}} ，直到实例准备完成被移除。

- **v-bind:src=" "  | :src=""**   为属性、class、style绑定值，不能使用双大括号{{}}，双大括号是为元素内容修改值。不支持驼峰名称：myName

  ```
  :class='{类名:boolean}'
  ```

- **v-on:click=" "**  **|  @click=""**    监听事件：参数event

  ```html
  //当用户点击或者移动鼠标等操作，浏览器会自动生成对象event：$event
  <button @click="clickBtn(name, $event)"></button>
  ```

  ```javascript
  new Vue({
      el: "#app",
      methods: {
          clickBtn(name, event){
              concole.log(name + " " + event)
          }
      }
  })
  ```

  - 修饰符：
    - event.stopPropagation()阻止事件冒泡：.stop
    - event.preventDefault()阻止默认行为:    .prevent
    - 监听键盘某个键位点击：.enter回车等
    - 监听**组件**根组件的原生事件：.native
    - 只触发一次：.once

- **v-show=" false"**：相当于display: none，会在页面中渲染上该元素。**v-if="false"**元素dom不会渲染到页面上。

  - 当显示和隐藏之间频繁切换时，使用v-show

  - 当只有一次切换时，使用v-if

- **v-model=" "**:   双向绑定处理用户输入。相当于v-bind绑定变量 + v-on:input=" "绑定监听事件修改输入修改变量值。

  ```html
  <--单选radio-->
  <div id="app">
      <lable for="male">
          <input type="radio" id="male" value="男" v-model="sex"> 男
      </lable>
      <lable for="female">
          <input type="radio" id="female" value="女" v-model="sex"> 女
      </lable>   
  </div>
      
  <--多选checkbox-->
  <--@1-->    
  <input type="checkbox" value="玩游戏" v-model="hobbies">玩游戏
  <input type="checkbox" value="跑步" v-model="hobbies">跑步
  <--@2-->        
  <label v-for="item in hobbiesOptions" :for="item">
  	<input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
  </label>
      
  <--单选select单选-->    
  <select name="eat" v-model="fruit1">
      <option value="苹果">苹果</option>
      <option value="芒果">芒果</option>
  </select>
      
  <--单选select多选-->    
  <select name="eat" v-model="fruit2" multiple>
      <option value="苹果">苹果</option>
      <option value="芒果">芒果</option>
  </select>
  ```

  ```javascript
  new Vue({
  	el: '#app',
  	data: {
  		sex: '男',
          hobbies: [],
          hobbiesOptions['篮球’,'足球','乒乓球'],
          fruit1:'芒果',
          fruit2:[]
  	}
  })
  ```

  - ​	修饰符
    - 懒加载：.lazy
    - 数字类型赋值给变量(v-model默认都会被当做字符串类型进行变量绑定)：.number
    - 去除首尾空格后赋值给变量：.trim

> 注意：vue中对象{key: value}
>
> key：不需要加引号；value：需要加引号（否则当做变量去使用）



### Vue(object)中object的key

- el: "#app"

- data: { }

- methods: {fullName() {}}，可以在元素内使用双大括号使用{{fullName()}}

- computed: {totlePrice: function(){}}，带有缓存,方法内部变量没有发生变化，调用时直接返回上次缓存的值。

  ```javascript
  computed: {
  	// @1
  	fullName: {
  		//计算属性一般不会设有set方法
  		set:function() {},
  		get: function() {}
  	}
  	//@2:默认为get方法，{{fullName}}调用的get方法
  	fullName: function(){}
  }
  ```

- filters: {}

  ```html
  <td>{{item.price | showPrice}}</td>	<--vue会自动将前面作为参数传入-->
  ```

  ```javascript
  filters: {
  	showPrice(price) {
  		return '$' + price.toFixed(2);
  	}
  }
  ```

- 生命周期函数

- watch

### 条件判断

​	**v-if	v-else-if	v-else	较为复杂的逻辑使用computed计算属性。**

```html
<h2 v-if="score>=90">优秀</h2>
<h2 v-else-if="score>=60">良好</h2>
<h2 v-else>不合格</h2>

<-- 推荐使用 -->
<h2>{{result}}</h2>
```

```javascript
new Vue({
	el: "#app",
	data: {
		score: 90
	},
	computed: {
		result() {
			let resultMesg = "";
			if(this.score >= 90) {
				resultMesg="优秀";
			} else if(this.score >= 60) {
				resultMesg="良好";
			} else {
				resultMesg="不合格";
			}
			return resultMesg;
		}
	}
})
```

### 遍历

v-for遍历数组，可添加唯一key提高性能，不要使用index作为key

数组响应式方法：push()、po()、shift()、 unshift()、 splice()、 sort()、 reverse()

当args[0] = "你好",数组内部元素会修改，页面不会响应式变化。

### 组件化

（一）命名规则：最省事始终使用 kebab-case 命名

- 组件：

  ```javascript
  // 模板命名和使用时命名都是用 kebab-case （短横线分隔命名）
  Vue.component('my-component-name', { /* ... */ })
  
  <my-component-name></my-component-name>
  ```

- prop参数

  ```javascript
  Vue.component('blog-post', {
    props: ['postTitle'],	//模板中camelCase（驼峰命名）
  })
  
  <blog-post :post-title="hello!"></blog-post>	//使用 kebab-case（短横线分隔命名）
  ```

- 自定义事件

  ```javascript
  始终使用 kebab-case 的事件名
  Vue.component('blog-post', {
      <button v-on:click="$emit('enlarge-text')">
        Enlarge text
      </button>
  })
  
  <blog-post v-on:enlarge-text="postFontSize += 0.1"></blog-post>
  ```



（二）步骤：1. 调用Vue.extend()创建组件构造器。2. 调用Vue.component()注册组件。3. 在Vue实例的作用范围内使用组件。

```html
<div id="app">
	<my-component></my-component>	<-- 3.使用组件-->
</app>

<script src="../js/vue.js"></script>
<script>
    Vue.component('my-component', {	//合并1 2的语法糖
    	template: `
			<div>
    			<h1>标题</h1>
    		</div>
		`});	 
    
    new Vue({
        el:'#app',
        components: {
            my-component: myComponent	<-- 2.局部注册组件-->
        }
    })
</script>
```

（三）组件模板分离：

1.  <script type="type/x-template" id="cpn"></script>

2.  <template id="cpn"></template>

（四）父子组件通讯：父通过props传给子；子通过事件传给父

```html
 <template id="componentB">
  <div>  
      <h2>{{name}}</h2> 		
      <h2>{{title}}</h2> 
      <-- 向父组件发送事件,第一个参数发射事件类型，其他参数为传入参数-->
      <button v-on:click="$emit('qie-title','切换')">切换杂志标题</button>
      <input 
          v-bind:value = "myValue" 
          v-on:input="changeValue($event)"/>
      <component-a></component-a>
  </div>
 </template>
```

```javascript
//组件B引入组件A
var componentB = {
    //组件内data声明成函数，防止多个地方使用该组件，变量互相影响。es6增强函数：data(){return {name:"hang"}}
    data: function() {
            return {name: 'hang', dtitle=this.title, dmyValue=this.myValue}	//将接受到的peops转成data中数据
        },
    
    //props中变量接受由父级组件传入的值，v-bind默认为单向数据流：父级的变化会改变子组件
    //props:["title","myValue"]
    props: {
        //带有约束条件
        "title":{
            type:[Number,String],
            required:true,
            defalut:'标题',
            //default(){return []},	//数组/对象形式默认值
            validator: function(value) {
                //匹配以下某个字符串
                return ['success','warning','danger'].indexOf(value) !== -1;
            }
        },
        //为父级容器使用v-model实现父子组件中变量同时变化
        'myValue':{}
    },
    
    //引入其他组件
    components: {
        'component-a':componentA,
    },
    
    //定义子模板
     template: '#componentB',
     methods: {
        changeValue: function(event) {
            this.dmyValue = event.target.value;
            $emit('change-val', event.target.value);
        }
     }
}
```

```html
/**父容器v-bind=""同时传入多个参数；v-on传入事件；v-model实现父子容器双向绑定**/
<div id="app">
     <-- v-on监听子组件发射出来的事件类型：qie-title-->
    <component-b v-bind= "post" v-on:qie-title = "swtitle" v-on:change-val = "changeVal">			</component-b>
</div>

<script>
	new Vue({
        el: '#app',
        data: {
            post: {
                'title': '中文杂志'   
            },
            inputValue: '请输入：'
        },
        components: {
            'component-b':componentB
        },
        methods: {
           'swtitle': function(ars) {
                alert(ars);
            },
            changeVal: function(val) {
                this.inputValue = val;
            }
        }
    })
</script>
```

（五）模板：替代v-model的.sync

```javascript
var com = {
	props:["title"],
	template:`
		<input 
            v-bind:value = "title" 
            v-on:input="$emit('update:title', newTitle)"/>
	`
}
```

```html
<div id="app">
	<com v-bind:title.sync="ftitle"></com>
</div>

<script>
	new Vue({
		el: "#app",
		data: {
			ftitle: ""
		}
	})
</script>
```

### 父子组件访问

- this.$children[0] | **this.$refs.refName**：访问子组件

- this.$parent ：访问父组件

- this.$root：访问根组件

### 组件扩展性：具名插槽slot

```html
<div id="app">
    <cpn><span slot="center">标题</span></cpn>
     <cpn><span slot="right">右边</span></cpn>
</div>
```

```html

<template id="cpn">
	<div>
        <slot name="left">默认值：左边</slot>
        <slot name="center">默认值：中间</slot>
        <slot name="right">默认值：右边</slot>
    </div>
</template>
```

父组件使用子组件插的数据： 

```html
<div id="app">
    <cpn>
    	<template slot-scope="slotProps">
        	<span v-for="item in slotProps.myData"></span>
        </template>
    </cpn>
</div>
```

```html
<template id="cpn">
	<div>
        <slot :myData="planList"></slot>
    </div>
</template>
```

模块化：导出、导入

- 基础

  ```javascript
  //aaa.js
  var modelA = (function() {	//全局modelA变量
  	var flag = true;
  	function sum(num1,num2) {};
  	var obj = {};
  	obj.flag = flag;
  	obj.sum = sum;
  	return obj;
  })()
  ```

  ```javascript
  //bbb.js
  <script src="./aaa.js" />
      
  if(modelA.flag){
      
  }
  ```

- nodejs使用的commonJS：需要node做支撑

  ```javascript
  //aaa.js
  var flag = true;
  function sum(num1,num2) {};
  //node环境才能解析该语法
  model.exports = {
  	flag: flag,
  	sum: sum
  }
  ```

  ```javascript
  //bbb.js
  var {flag, sum} = require(./aaa.js);
  ```

- ES6：export、import：浏览器作为支撑

  ```html
  <--index.html:模块化加type="module"-->
  <script src="aaa.js" type="module"></script>
  <script src="bbb.js" type="module"></script>    
  ```

  ```javascript
  //aaa.js
  var flag = true;
  function sum(num1,num2) {};
  //导出方式一
  export{
  	flag, sum
  }
  //导出方式二
  export var num1 = 100;
  export function mul(){};
  export class Person{};
  //导出方式三(default只能导出一个)
  export defalut flag
  ```

  ```javascript
  //bbb.js：通过方式一、二导出的导入必须名称对应；
  import{flag,sum} from "./aaa.js"
  
  //方式三default导出这块可以随意命名
  import f from "./aaa.js"
  
  //全部导入
  import * as obj from "./aaa.js"
  ```


### 内部性能优化

1. Vue在进行DOM渲染时，会在真实dom和浏览器显示中间增加virtual dom在内存中，出于性能考虑，会尽可能的复用已经存在的元素，而不是重新创建新的元素。

   [官网](https://cn.vuejs.org/v2/guide/conditional.html)：中input复用实例。通过key来避免复用。

2. v-for遍历时，建议添加上key属性。当遍历数组在页面渲染成功后，向数组中间插入元素。
   1. 没有添加key属性：Vue的虚拟DOM的Diff算法，会依次修改插入元素所处位置后面的值，相当于向后移动一位。
   2. 添加key属性：为每个渲染的节点做唯一标识，找到正确的位置插入新的节点。key 对应的dom没有发生变化就不会修改虚拟dom，所以**不要使用index作为key**，当向中间插入后，插入位置后面所有元素对应的key都会变动，导致虚拟dom会重新渲染，与没有添加key属性新能一样。

### webpack环境集成Vue.js

```shell
npm install --save vue	//安装vue
```

```javascript
import Vue from ‘vue’	//导入vue使用new Vue()挂载模块
```

```javascript
//webpack.config.js 指定版本vue为runtime-complier进行编译template
resolve: {
	alias: {
		'vue$': 'vue/dist/vue.esm.js'
	}
}
```

### el和template同时存在

template内容会替换父模板<div id="app"></div>

```javascript
import Vue from 'vue'

new Vue({
    el: '#app',
    template: `
        <div>
            nihao~
        </div>
    `
})
```

### Vue开发时使用模板

```javascript
//App.vue
<template>
	<div> </div>
</template>

<script>
	export default {
		name: "App",
		data() {
			return　{
			
			}
		}，
		methods() {}
	}
</script>

<style scoped>
</style>
```

```javascript
import Vue from 'vue'
import App from './vue/App.vue'
new Vue({
	el: '#app',
	template: '<APP/>',
	components: {
		APP
	}
})
```

### Vue cli脚手架的使用

快速搭建vue开发环境以及对应的webpack配置

```
//vuecli3版本 @1(只是用脚手架3)
npm install -g @vue/cli
vue create my-project

//在脚手架3的基础上使用vuecli2版本 @2(先安装脚手架3，在安装脚手架2，两个都可以使用)
npm install -g @vue/cli-init
vue init webpack my-project
```

