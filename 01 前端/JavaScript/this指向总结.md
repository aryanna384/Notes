### 不同环境下全局作用域的获取方式

web：`window`、`self`、`frames`、`this`

node：`global`

worker：`self`

通用：`globalThis`

严格模式`"use strict"`预处理指令，选择这种语法形式的**目的**是不破坏 ECMAScript 3 语法

```javascript
function test(){
  return this;
}
console.log((test()));//window
console.log((window.test()));//window
```

严格模式下：

```javascript
function test(){
  'use strict';
  return this;
}
console.log((test()));//undefined
console.log((window.test()));//window
```

`call()`、`apply()`、`bind()`

`bind()`只生效一次

1. 改变 this指向
2. 第一个参数是 this 的值，后面的参数是函数接受的参数的值
3. 返回值不变

```javascript
function fun(a, b) {
    console.log("a=" + a);
    console.log("b=" + b);
    console.log("in func", this.name);
    console.log("in func", this.sayName());
}

var obj = {
    name: "obj",
    sayName: function () {
        console.log("in obj", this.name);
    }
};

var obj2 = {
    name: "obj2",
    sayName: function () {
        console.log("in obj2", this.name);
    }
};

var test = fun.bind(obj, 2, 3)//.bind(obj2, 4, 5);
test.bind(obj2, 4, 5)
console.log(test);
console.log(test());
//////输出
//[Function: bound fun]
//a=2
//b=3
//in func obj
//in obj obj
```

其中 23、24 行和合在一起是一样的

```java
var test = fun.bind(obj, 2, 3).bind(obj2, 4, 5);
```

### 箭头函数



### 对象中的 this

一般情况：指向最近的引用

但是：

<img src="../images/截屏2021-05-25 22.43.52.png" alt="截屏2021-05-25 22.43.52" style="zoom:50%;" />

<img src="../images/截屏2021-05-25 22.47.16.png" alt="截屏2021-05-25 22.47.16" style="zoom:50%;" />

### 事件中的 this
`this` 返回的是绑定事件的对象（元素）  element.addEventListener
>`e.targrt` 返回的是触发事件的对象（元素）

绑定了 ul 点击的是 li

```html
<ul>
   <li>lilililili</li>
</ul>
<script>
	var li = document.querySelector("li");
	var ul = document.querySelector("ul");
	ul.addEventListener('click',function(e){
		console.log(e.target);//e.target -> li
		console.log(this);    //this -> ul
	})
 </script>
```

