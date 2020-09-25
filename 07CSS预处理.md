# [Sass](https://www.jianshu.com/p/a99764ff3c41)

- 1. 变量 $name

  ```css
  $myfont-color: red;
  //嵌套语法
  div {
  	a {
          color:$myfont-color;
          font-size: 100px / 1920px * 100%;	//可以使用操作符：+ - * / %
  	}
  }
  ```

- 2. 引入@import：会直接将引入的片段合并至当前CSS文件，并且不会产生新的HTTP请求。

  ```css
  // _reset.scss	使用下划线作为前缀，SASS会识别后不会直接生成CSS文件
  html, body, ul, ol {
    margin:  0;
    padding: 0;
  }
  ```

  ```css
  // base.scss
  @import 'reset';	//无需填写后缀
  body {
    font: 100% Helvetica, sans-serif;
    background-color: #efefef;
  }
  ```

  输出：

  ```css
  html, body, ul, ol {
    margin: 0;
    padding: 0; 
  }
  body {
    font: 100% Helvetica, sans-serif;
    background-color: #efefef; 
  }
  ```

- 3. 混合操作@mixin name：添加浏览器兼容性前缀较为适合

  ```css
  @mixin border-radius($radius) {
            border-radius: $radius;
        -ms-border-radius: $radius;
       -moz-border-radius: $radius;
    -webkit-border-radius: $radius;
  }
  
  .box {
    @include border-radius(10px);
  }
  ```

- 4. 继承@extend

  ```css
  // 这段代码不会被输出到最终生成的CSS文件，因为它没有被任何代码所继承。
  %other-styles {
    display: flex;
    flex-wrap: wrap;
  }
  
  // 下面代码会正常输出到生成的CSS文件，因为它被其接下来的代码所继承。
  %message-common {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333;
  }
  
  .message {
    @extend %message-common;
  }
  
  .success {
    @extend %message-common;
    border-color: green;
  }
  
  .error {
    @extend %message-common;
    border-color: red;
  }
  
  .warning {
    @extend %message-common;
    border-color: yellow;
  }
  ```

  输出：

  ```css
  .message, .success, .error, .warning {
    border: 1px solid #ccc;
    padding: 10px;
    color: #333; }
  
  .success {
    border-color: green; }
  
  .error {
    border-color: red; }
  
  .warning {
    border-color: yellow; }
  ```

- 5. 引用父级选择器 &

  ```css
  /*===== SCSS =====*/
  a {
    font-weight: bold;
    text-decoration: none;
    &:hover { text-decoration: underline; }
    body.firefox & { font-weight: normal; }
  }
  
  /*===== CSS =====*/
  a {
    font-weight: bold;
    text-decoration: none; 
  }
    a:hover {
      text-decoration: underline; 
  }
    body.firefox a {
      font-weight: normal; 
  }
  ```

  