[TOC]

## 初识 VUE

核心：数据的双向绑定、虚拟 DOM

设计模式：MVVM模式

响应性：

- 响应性是一种允许我们以声明式的方式去适应变化的编程范例。如 excel，
- “响应式”，是指当数据改变后，Vue 会通知到使用该数据的代码。例如，视图渲染中使用了数据，数据改变后，视图也会自动更新。

渐进式 JavaScript 框架

- 渐进式：用你想用或者能用的功能特性，你不想用的部分功能可以先不用。VUE不强求你一次性接受并使用它的全部功能特性，分为核心库+插件。

- 作用：动态构建用户界面，将后台的数据在前台动态显示

- 遵循模式：MVVM响应模式（Model View View Model）

## 使用方法

- vue-cli

1. 进入要安装的目录
2. `vue create <文件名>`
3. 运行服务`npm run serve`

主要在 vue.vue 文件写

- Vue2：直接 在 script 中引用

```
<script src="js/vue.js" type="text/javascript" charset="utf-8"></script>
```

- 原理

```html
<body>
	<!--view-->
		<div id="app">
			<input type="text" v-model="title"></input>
            <h1>{{title}}</h1>
		</div>
        <script type="text/javascript">
            const vm = new Vue({
                el:"#app",
                data:{
                    title:"hello vue",
                }
            });
        </script>
	</body>
```

vue 属于声明式开发：按照规则声明，不需要管流程

> 命令式开发：
>
> Jquery

MVVM 对应：

model：数据对象 data

view：视图

view model：视图模型vue 实例（vm



## 基础语法

### 条件渲染

```
v-if、v-else、 v-else-if
```

v-if 和 v-else 连用的时候中间不能有其他元素 
#### 条件语句

```vue
<body>
  <div id="app">
    <h1> 用户名:{{ username }}</h1>
    <h3 v-if="isVip"> 用户类型：VIP</h3>
    <h3 v-else> 用户类型：普通用户</h3>
    <hr>
    <h3 v-if="age>18"> 允许 24 小时登录</h3>
    <h3 v-else-if="age>14">允许 8 小时登录</h3>
    <h3 v-else> 不允许登录</h3>
  </div>
  <script type="text/javascript">
    let app = new Vue({
      el: "[[app]]", //绑定
      data: {
        username: "xiaoming",
        isVip: false,
        age: 18
      }
    })
  </script>
</body>
```

####  v-show

```html
<div id="app">
  <div v-show="isShow" id="pane">helloVue</div>
  <button type="button" @click="showBtn"> 切换显示内容 </button>
</div>
<script type="text/javascript">
  let app = new Vue({
    el:"[[app]]",
    data:{
      isShow:true
    },
    methods:{
      showBtn:function(e){      
        console.log(e);
        app.isShow = !app.isShow;
      }
    }
  })
</script>
```

- v-if 和 v-show 的区别
  - v-if 不显示时将第一次不渲染，如果内容已经显示则改为不显示，将内容从 DOM 直接去除，
  - v-show不显示时，会改为 display：none，但是会渲染在 DOM 上

> 反复需要切换的内容使用 v-show

- 实现点击 tab 切换页面的功能

```html
<div id="app">
  <h3 v-show="tab==1">首页</h3>
  <h3 v-show="tab==2">新闻页</h3>
  <h3 v-show> 个人中心</h3>
  <button type="button" v-on:click="tabChange" data-id="1">首页</button>
  <button type="button" v-on:click="tabChange" data-id="2">新闻</button>
  <button type="button" v-on:click="tabChange" data-id="3">个人中心</button>
</div>

<script type="text/javascript">
  let app = new Vue({
    el: "[[app]]",
    data: {
      tab: 1,
    },
    methods: {
      tabChange: function(e) {
        console.log(e);
        let tabId = e.target.dataset.id;
        app.tab = tabId;
      }
    }
  })
</script>
```

[[css&html零散[[Data]]-]]

### 列表渲染`v-for`

```html
<li  v-for =" item,index in <对象>" :key="index>
  {{item}}
</li>
<li v-for="(item,name) in students[0]" >
 //这样写 name 表示属性名
  {{name}}:{{item}}
</li>
students:[
  {
    stuName:"ming",
    age:16,
    school:"tsinghua"
  },
  {
    stuName:"hua",
    age:16,
    school:"shu"
  },
  {
    stuName:"hong",
    age:16,
    school:"jiaotong"
  }
]
//outputs
0,{ "stuName": "ming", "age": 16, "school": "tsinghua" }
1,{ "stuName": "hua", "age": 16, "school": "shu" }
2,{ "stuName": "hong", "age": 16, "school": "jiaotong" }
---------<hr>
stuName:ming
age:16
school:tsinghua
```

`:key`一般与 `v-for`一起使用，用来标识唯一的元素

>v-bind  *abbr* :



### 模板语法

模板的理解：

- 动态的 html 页面

- 包含了一些 JS 语法代码

  ​	双大括号表达式 {{}}

  ​	指令：以 v-开头的自定义标签属性



#### 文本数据绑定

`{{ <变量> }}`双大括号中的数据将会被解释为普通文本

- v-once

一次绑定，之后再修改数据不会改变

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

- v-html

解释为 html 代码

```html
<p v-html="rawHtml"</p>
rawHtml:"hello vue"
```

但是这样动态渲染 html 容易引发<span style="color:red"> xss 攻击</span>

> XSS攻击通常指的是通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序。

### 计算属性

什么时候执行：

- 初始化时

- 相关属性发生变化时

此时，publishedBooksManage 是计算属性

```vue
<div id="computed-basics">
  <p>Has published books:</p>
  <!--<span>{{ author.books.length > 0 ? 'Yes' : 'No' }}</span>-->
  <span>{{ publishedBooksMessage }}</span>
</div>
<script>
let app = new Vue({
  data: {    
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
  },
  computed: {
    // 计算属性的 getter
    publishedBooksMessage() {
      // `this` 指向 vm 实例
      return this.author.books.length > 0 ? 'Yes' : 'No'
    }
  }
})
</script>
```

#### 计算属性的 get()和 set()

```javascript
computed:{
绑定的方法名:{
             get(){
            return 读取当前属性时的返回值
          }
          set(value){
               当数据变化时的操作，value 和 get 的返回值相同格式
          }
	} 
}
```

#### 方法 & 计算属性 的区别

方法和计算属性的实现结果一样

但是**计算属性的反应依赖缓存**，计算属性只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `author.books` 还没有发生改变，多次访问 `publishedBookMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

每当触发重新渲染时，调用方法将总会再次执行函数

### 监听 watch

监听的两种写法

在 app.msg 改变的时候触发 watch 事件

- 方式一：在 vue 实例中

```html
<div id="app">
    {{msg}}
    <li v-for="item in arr ">{{item}}</li>
 </div>
<script>
  let app=new Vue({
    el:"[[app]]",
    data:{
      msg : "hello vue"                
    },
    //监听事件
    watch:{
      msg:function(value){
        console.log("监听事件-----");
        console.log(value);//输出更新后的数据
      },
       arr:function(val){
         console.log("监听事件-----");
         console.log(val);
      },
    },
  })
</script>

浏览器操作：
app.msg = "hello"
app.arr.push("huang")//和操作数组一样
```

- 方式二：调用 vue 实例

```html
 app.$watch(监听对象,function(value){
	this.msg = value;
})
```

### 事件处理

`v-on:【DOM 事件】`

1. 多事件用逗号隔开

```html
<button @click= "func1,func2"></button>
```

2. 可以传递参数

```html
<button @click="func(value)"></button>
```

3. 可以传递事件`&event`

```html
<button @click="warn('Form cannot be submitted yet.', $event)"> Submit</button>
methods: {
  warn(message, event) {
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```
[[js零散[[preventDefault]] DOM event]]

#### 事件修饰符

| 修饰符   | 说明                               |
| -------- | ---------------------------------- |
| .stop    | 阻止事件继续传播，阻止冒泡         |
| .prevent | 阻止默认事件的发生                 |
| .capture | 使用事件捕获模式                   |
| .self    | 事件不是内部元素产生，只是自己产生 |
| .once    | 事件只触发一次                     |
| .passive | 提升移动端性能                     |

#### 按键修饰符

`@keyCode.enter `

### 表单输入绑定

`v-model` 绑定变量

- text 和 textarea 元素使用 `value`  和 `input` 事件；

  >也就是说是用户输入内容

  - text的 value

  ```javascript
  <input v-model="message" placeholder="edit me" />
  <p>Message is: {{ message }}</p>
  ```
  
  - textarea的 value
  
  ```javascript
  <span>Multiline message is:</span>
  <p style="white-space: pre-line;">{{ message }}</p>
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
  ```
  
- checkbox 和 radio 绑定 `checked` 的 value 属性

  - 单选checkbox

  ```html
  <input type="checkbox" id="checkbox" v-model="checked" />
  <label for="checkbox">{{ checked }}</label>
  //true/false
  ```

  - 多个 checkbox，绑定 value

  ```html
  <div id="v-model-multiple-checkboxes">
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames" />
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
    <label for="mike">Mike</label>
    <span>Checked names: {{ checkedNames }}</span>
  </div>
  //Checked names:[value]
  ```

  - radio绑定 value
  
  ```html
  <div id="v-model-radiobutton">
    <input type="radio" id="one" value="One" v-model="picked" />
    <label for="one">One</label>
    <br />
    <input type="radio" id="two" value="Two" v-model="picked" />
    <label for="two">Two</label>
    <span>Picked: {{ picked }}</span>
  </div>
  //picked 值为 One/Two(value)
  ```
  
- select 绑定 `value`

```html
<div id="v-model-select" class="demo">
  <select v-model="selected">
    <option disabled value="">Please select one</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

### 指令Directives

### 常用内置指令

`v-text`：更新元素 textContent

`v-html`：更新 innerHTML

`v-if、v-else、v-show`：条件选择

`v-for`：遍历对象

`v-on`：绑定监听事件

`v-model`：数据双向绑定

`v-bind(:)`：强制绑定解析表达式

`ref`:指定唯一标识，vue 对象通过`$refs`属性访问这个元素对象

```html
<input ref="usernameInput"></input>
this.$refs.usernameInput
```

`v-cloak`:不需要表达式，直到编译结束不会显示，防止闪现，与 css配合使用`[v-cloak{display:none}]`

### 自定义指令

### 过滤器

管道符`|`

```html
//调用
<p>{{data | dataStr}}</p>
//定义
<script>

//1.全局定义
Vue.filter("dataStr",function(value,format){
	return moment(value).format(format || "YYYY-MM-DD HH:mm:ss");
})
//2.在组件中局部定义
filters:{
    dataStr:function(value){
	return moment(value).format(format || "YYYY-MM-DD HH:mm:ss");
}
}
</script>
```

1、 当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！

2、 一个表达式可以使用多个过滤器。过滤器之间需要用管道符“|”隔开。其执行顺序从左往右



### 过渡和动画

#### transition 组件

```css
 .fade-enter-active,
 .fade-leave-active {
   	transition: opacity 3s;
 }
 .fade-enter-from,
 .fade-leave-to {
  	opacity: 0;
 }
 <transition name="fade"></transition>
```

隐藏时的样式

xxx-enter-to,xxx-leave-to

显示的过渡效果

xxx-enter-active

隐藏的过渡效果

xxx-leave-active

在进入/离开的过渡中，会有 6 个 class 切换

1. `v-enter-from`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
2. `v-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
3. `v-enter-to`：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 `v-enter-from` 被移除)，在过渡/动画完成之后移除。
4. `v-leave-from`：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
5. `v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
6. `v-leave-to`：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 `v-leave-from` 被删除)，在过渡/动画完成之后移除。

## 组件

应用实例：每个 Vue 应用都是通过用 createApp 函数创建一个新的应用实例开始，应用实例是用来在应用中注册“全局”组件的。const app = Vue.createApp({})

组件：带有名称的可复用实例，可以把这个组件作为一个根实例中的自定义元素来使用。

根组件：传递给 `createApp` 的选项用于配置根组件。当我们挂载应用时，该组件被用作渲染的起点。

const RootComponent={}

const app = Vue.createApp({RootComponent})

### 组件注册

#### 全局注册

全局注册之后可以用在任何新创建的组件实例的模板中，但是全局注册所有的组件意味着即便你已经不再使用其中一个组件了，它仍然会被包含在最终的构建结果中。



```javascript

//创建 app 实例
const app = Vue.createApp({})
//注册一个名为component-a的全局组件
app.component('component-a', {
  /* ... */
})
//挂载根组件，该组件被用作渲染的起点
app.mount('#app')
```



```html
<template>
    <div id="app">
        <component-a></component-a>
    </div>
</template>
```

#### 局部注册

`components`对象，其 property 名就是自定义元素的名字，其 property 值就是这个组件的选项对象。

```javascript
const ComponentA = {
  /* ... */
}
const app = Vue.createApp({
  components: {
    //给 ComponentA 组件命名为 component-a
    'component-a': ComponentA,
  }
})
```

### 组件实例 property

data

methods

computed

inject

setup

## 插槽

### 插槽slot/单个插槽/匿名插槽

为什么要使用插槽 > 实现组件之间的通信

- slot 作为想要插入内容的占位符（子
- slot 作为分发内容的出口（父
- 插槽模板是slot，它是一个空壳子，因为它显示与隐藏以及最后用什么样的html模板显示由父组件控制。但是插槽显示的位置确由子组件自身决定，slot写在组件template的哪块，父组件传过来的模板将来就显示在哪块。

一个组件中只能有一个单个插槽，具名插槽就可以有很多个，只要名字（name属性）不同就可以了。

渲染作用域：

父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

### 具名插槽：实现插槽复用

slot的name属性

```html
<header>
  ## header 占位
   	<slot name="header"></slot>
</header>
```

使用：v-slot 只能添加在 <template> 上

```html
<template  v-slot:header>
     <h1>Here might be a page title</h1>
</template>
```

### 作用域插槽：父组件访问子组件数据

作用域插槽实际上是带有数据的插槽，使得插槽内容能够访问子组件中才有的数据

定义

```html
<slot :item='item'></slot>

data(){
	item:['feed a cat']
}
```

使用

```html
<template v-slot:default="slotProps">
		{{ slotProps.item }}
  </template>
```

[参考](https://blog.csdn.net/houyibing930920/article/details/89513246)
