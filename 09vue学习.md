	前端规范：缩进2个空格

MVVM：model + model view + view实现的双向数据绑定；主要依靠View model中Dom Listeners对view的监听，Directives对model中数据改变进而修改dom；模块化

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
            return {name: 'hang', dtitle=this.title, dmyValue=this.myValue}	//将接受到的props转成data中数据
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

```shell
//vuecli3版本 @1(只是用脚手架3)
npm install -g @vue/cli
vue create my-project

//在脚手架3的基础上使用vuecli2版本 @2(先安装脚手架3，在安装脚手架2，两个都可以使用)
npm install -g @vue/cli-init
vue init webpack my-project
```

### 路由

#### 一、web开发历程

1. 后端渲染jsp、php
2. 前后端分离：不同链接对应不同html+css+js
3. SPA：单页面富应用，整个网页只有一个html页面  。

静态资源服务器：最终将全部文件打包成一个html、一个js、一个css。

客户端请求资源，服务器将html + css + js（全部资源）作为响应，客户端不会全部加载显示，当更改请求链接时浏览器只是根据前端路由规则显示不同页面。



#### 二、SPA：单页面富应用

不请求服务器的方式：

1.  锚（hash值）：/#/home

    ```
    location.hash = 'home'
    ```

2.  BOM修改 history.pushState()：/home

    ```
    //类似栈形式
    history.pushState({},'','home')	//localhost:8080/home
    history.pushState({},'','about')//localhost:8080/about
    
    history.back()	//localhost:8080/home#
    history.forward()
    history.go(-1)	//等同于back()
    history.go(1)	//等同于forward()
    ```

3.  BOM修改 history.replaceState()：/home

    ```
    //该方式没有历史纪录，不能使用history.back()
    history.replaceState({},'','home')
    ```

#### 三、vue-router的单页面应用，页面的路径改变就是组件的切换。

1.  引入vue-router

```
npm install vue-router --save
```

2.  配置路由相关信息

    router/index.js

```vue
// 
import VueRouter from 'vue-router'
import Vue from 'vue'

// import Home from '../componetns/Home'
//懒加载组件，最终打包成多个js,客户端路由到哪个就访问哪个js，不向之前全部组件打包成一个js，大型项目最终会很大。
const Home = () => import('../components/Home')
const About = () => import('../components/About')
const HomeNews = () => import('../components/HomeNews')
//1 通过Vue.use(插件)，安装插件
Vue.use(VueRouter)

//2 创建VueRouter对象
const routes = [
	//访根目录默认重定向
	{
		path: '',
		redirect: '/home'
	}，
	{
		path: '/home',
		component: Home,
		//子路由
		children: [
			{
				path: 'news',
				component: HomeNews,
			}
		]
	},
    {	
		//动态路由
		path: 'about/:userId',
		component: 'About'
    }
]
const router = new VueRouter({
	//配置路由和组件之间的映射关系
	routes,
	mode: 'history'	//防止浏览器显示的hash，即带有#
})

//3 将router对象传入到vue实例
export default router
```

​			main.js

```
new Vue({
	el: '#app',
	router: router,
	render: h >= h(App)
})
```

​		App.vue

```vue
<template>
	<div id="app">
        <router-link to="/home">首页</router-link>	//最终渲染成a标签
         <router-link :to="'/about/' + userId">首页</router-link>	//通过this.$route.params.userId获取
       <router-view></router-view>
    </div>    
</tempalte>
<script>
	export default{
        data() {
            return {
                userId: 'zhangsan'
            }
        }
    }    
</script>
```

1. `view-link路由跳转属性:`

```vue
<router-link to="/home" tag="button" replace active-class="active"/>
```

```javascript

```

2. 参数传递

```javascript
//动态参数params：字符串
{
	path: 'about/:userId',
	component: 'About'
},
{
	path: 'home',
	component: 'Home'
}
```

```javascript
//params字符串参数   
//about/zhangsan   通过this.$route.params.userId获取
<router-link :to="'/about/' + userId">首页</router-link>

//query对象参数
//home?name=hang 通过this.$route.query.name获取
<router-link :to="{path: '/home', query: {name: 'hang'}}">首页</router-link>
```

3. router路由跳转

```javascript
jumpRoute() {
	this.$router.push('/about' + this.userId);	//push => pushState	可返回
	this.$router.replace({	//replace => replaceState
        path: '/home',
        query: {
            name: 'hang'
        }
    }); 
}
```



#### 四、vue-router内部属性

​	所有组件都继承vue原型：vue.prototype.$router以及vue.prototype.$route都是在原型类中。

1. this.$router：即index.js中创建的router属性，在所有组件data()内部默认都存在，**push()、replace()**效果相当于~~hsitory.pushState()、history.replaceState()~~，history的方法会绕过vue-router。beforeEach()进行跳转拦截
2. this.$route：处于活跃路由的组件。.params、.query获取访问该组件时传入的参数。

#### 五、项目遇到的问题

（一）、**生命周期中包含有异步请求（阻塞、不阻塞）**

1. 钩子函数（8大生命周期）
     	1. beforeCreate
      2. created：**$data、data中的数据、methods中的方法已创建完成**
      3. beforeMount
      4. mounted：**$el创建完成并替换了DOM元素**
      5. beforeUpdate
      6. updated：**data中数据发生改变，会触发对应组件的重新渲染（beforeUpdate和updated没看出区别）**
      7. beforeDestroy：**销毁前、实例可用**
      8. destroyed：**vue实例销毁**

2. 问题描述：

   当钩子函数中有异步请求（即使阻塞），不会妨碍下一阶段钩子函数的执行

   ```js
   async created() {
   	console.log("created-----into");
   	await res = await this.$axios.get("./getData/country.json");
   	console.log(res);
   	console.log("created-----out");
   },
   mounted() {
   	console.log("mounted-----into");
   }
   // 正常逻辑：created执行完，再执行mounted，可是，执行方式有差异！！！
   // created-----into
   // mounted-----into
   // data
   // created-----out
   ```

3. 问题解决：

   再使用该模块**添加v-if = “flag”标识**。初始为否，异步数据获取到再设置为真。

   <span style="color: red">父子组件的 created 是顺次执行的：先父后子。</span>

   ```html
   <template>
   	<div>
           <!-- 当leftDataFlag为false时该template不会被加载 -->
   		<left-table v-if="leftDataFlag" :data="data"/>
   	</div>
   </template>
   
   <script>
   export default {
   	data() {
           return {
               leftDataFlag: false,
               data: {}
           }
       }
   	created() {
           getRestData(param).then(resp => {
               this.data = JSON.parse(resp);
               this.leftDataFlag = true;
           })
       }
   }
   </script>
   ```

4. 参考文章

   [Vue created 中的异步请求的影响分析](https://blog.csdn.net/wojiushiwo945you/article/details/107539344)

（二）、**子组件v-if=“false”，父组件无法调用子组件的方法**

 1. 尽可能避免的写法

    ```html
    // 父组件 (这样写模态框会被记载到父组件中)
    <a-modal title="用户信息完善" v-model="showModal" width="35%">
       <user-info-complete ref="infoModal" v-if="showUserInfo"></user-info-complete>
    </a-modal>
    
    // 请求该方法：this,$refs.infoModal.selectData(); 
    // 当showUserInfo为false时： 调用子组件的方法，是获取不到的，父组件中不存在该dom。
    // 当showUserInfo为true时: 才能调用子组件的方法。
    ```

    ```html
    // 子组件
    <template>
      // 内容
    </template>
    
    <script>
    export default {
    	methods:{
    		selectData() {
    			this.showModal = true;
                // 获取弹出框所需数据
                // ...
    		}
    	}
    }
    </script>
    ```

 2. 推荐的写法（将弹出框写在组件中）

    ```html
    // 父组件 (这样写模态框会被记载到父组件中)
    <template>
       <user-info-complete ref="infoModal"></user-info-complete>
    </template>
    
    // this,$refs.infoModal.selectData(); 调用子组件的方法，使模态框显示
    ```

    ```html
    // 子组件
    <template>
      <a-modal title="用户信息完善" v-model="showModal" width="35%">
      	//...
      </a-modal>
    </template>
    
    <script>
    export default {
    	data() {
            return {
                showModal: false
            }
        }
    	methods:{
    		selectData() {
    			this.showModal = true;
                // 获取弹出框所需数据
                // ...
    		}
    	}
    }
    </script>
    ```

 3. 总结：

    **v-show使用display:none实现，会存在于dom树中。**

    ```html
    <h3 v-show="showModal">专项类型</h3>
    ```

    ![image-20210329200731760](C:\Users\25681\AppData\Roaming\Typora\typora-user-images\image-20210329200731760.png)

    **v-if 为false时直接从dom中去除**。

    ```
    <h3 v-if="showModal">专项类型</h3>
    ```

    <img src="C:\Users\25681\AppData\Roaming\Typora\typora-user-images\image-20210329195721465.png" alt="image-20210329195721465 " style="float: left;margin-top:20px" /> 

<img src="C:\Users\25681\AppData\Roaming\Typora\typora-user-images\image-20210329195852873.png" alt="image-20210329195852873" title=" style=&quot; " style="float: left; margin-left: 20px" /> 

