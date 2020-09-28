# Vue

1. MVVM：model + model view + view实现的双向数据绑定；主要依靠View model中Dom Listeners对view的监听，Directives对model中数据改变进而修改dom。

2. 模块化



vue指令：v-bind（属性）、v-on（事件）、v-model（绑定用户输入）、v-if、v-show、v-for、v-once、v-html

命名规则：最省事始终使用 kebab-case 命名

- 组件：

  ```
  // 模板命名和使用时命名都是用 kebab-case （短横线分隔命名）
  Vue.component('my-component-name', { /* ... */ })
  
  <my-component-name></my-component-name>
  ```

- prop参数

  ```
  Vue.component('blog-post', {
    props: ['postTitle'],	//模板中camelCase（驼峰命名）
  })
  
  <blog-post post-title="hello!"></blog-post>	//使用 kebab-case（短横线分隔命名）
  ```

- 自定义事件

  ```
  始终使用 kebab-case 的事件名
  Vue.component('blog-post', {
      <button v-on:click="$emit('enlarge-text')">
        Enlarge text
      </button>
  })
  
  <blog-post v-on:enlarge-text="postFontSize += 0.1"></blog-post>
  ```



## 与子模块间通讯：

```javascript
//组件B引入组件A，title由
var componentB = {
    //组件内data声明成函数，防止多个地方使用该组件，变量互相影响
    data: {
        function() {
            return {name: 'hang'}
        },
    },
    //props中变量接受由父级组件传入的值，v-bind默认为单向数据流：父级的变化会改变子组件
    props: {
        //带有约束条件
        "title":{
            type:[Number,String],
            required:true,
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
     template: `
        <div id="componentB" >   
            <h2>{{title}}</h2> 
            <component-a></component-a>
            <button v-on:click="$emit('qie-title','切换')">切换杂志标题</button>
			<input 
                v-bind:value = "myValue" 
                v-on:input="$emit('input', $event.target.value)"/>
        </div>`,
}
```

```html
/**父容器v-bind=""同时传入多个参数；v-on传入事件；v-model实现父子容器双向绑定**/
<div id="app">
    <component-b v-bind= "post" v-on:qie-title = "swtitle" v-model="inputValue">			</component-b>
</div>

<script>
	var app = new Vue({
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
            }
        }
    })
</script>
```

替代v-model的.sync

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
	var app = new Vue({
		el: "#app",
		data: {
			ftitle: ""
		}
	})
</script>
```

## solt插槽