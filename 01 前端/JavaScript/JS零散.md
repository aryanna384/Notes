[TOC]

## ⭐️概念问题

### 关于 DOCTYPE 文档声明

- 责任是告诉浏览器文档使用哪种 html 或 xhtml 规范
- 不同文档模式主要影响 css 内容的呈现，尤其是浏览器对盒模型的解析
- 不同浏览器在混杂模式下的行为差异很大
- 如果在文档开始出没有发现文档类型的声明，浏览器会默认开启混杂模式

### 浏览器的文档模式

- 标准模式（standards mode）：浏览器以其支持的最高标准呈现页面。
- 混杂模式（quirks mode）：页面以一种比较宽松的向后兼容的方式显示。混杂模式通常模拟老式浏览器的行为以防止老站点无法工作
- 准标准模式（almost standards mode）

严格性：标准＞准标准＞混杂

标准和混杂两种模式的区别

1. 对盒模型的解析：

​	混合模式盒模型：宽高=内容的宽高；

​	标准模式盒模型：宽高=内容的宽高+padding的宽高+border的宽高

2. 当一个块元素div中包含的内容只有图片时

   在标准模式下，不管IE还是标准，在图片底部都有3像素的空白

   混杂模式下，标准浏览器（Chrome）中div距图片底部默认没有空白。

3. 设置行内元素的高度

   标准模式，给span等行内元素设置width和height没有效果；

   混杂模式下，会生效

4. 设置百分比的宽度

   标准模式，一个元素的高度是由它包含的内容决定的，若父元素没有设置高度，子元素设置一个百分比的高度是无效的。

5. margin：0 auto设置水平居中在IE下会失效

   标准模式下可以使元素水平居中

   混杂模式下的解决办法用text-align

6. 混杂模式下table的自提属性不能继承上层的设置

7. 混合模式下white-space：pre会失效


判断浏览器模式

```javascript
if(document.compatMode == “CSS1Compat” ) {
	console.log('标准模式');
}else{//“BackCompat”-混杂模式
	console.log('混杂模式');
}
```



### JS运行环境

JavaScript 环境实际上是运行在托管操作系统中的虚拟环境。在浏览器中每打开一个页面,就会分配一个它自己的环境。这样,每个页面都有自己的内存、事件循环、DOM,等等。每个页面就相当于一个沙盒,不会干扰其他页面。对于浏览器来说,同时管理多个环境是非常简单的,因为所有这些环境都是并行执行的。

### 语法糖

- 语法糖（Syntactic sugar），也译为糖衣语法
- 指计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用。
- 通常来说使用语法糖能够增加程序的可读性，从而减少程序代码出错的机会。
- 语法糖”这个词绝非贬义词，它可以给我们带来方便，是一种便捷的写法，编译器会帮我们做转换；而且可以提高开发编码的效率，在性能上也不会带来损失。

### 关于 JS

JS动态语言，C++静态语言；JS运行时编译（Just In Time），C++AOT（Ahead of time）

JS 函数式编程+面向对象编程

JS 编译器



JS 运行原理：JS 源码---(解析器)---->抽象语法AST---(解释器)--->字节码---(编译器)--->机器代码



### JS 原始值

与原始值对应的是引用值
原始值：
- 保存原始值的变量是按值来进行访问的
- 在原始值的复制中，值会直接被复制到新变量的地方
- typeof判断原始值的类型
引用值：
- 引用值是保存在内存中的对象
- （浅拷贝）引用值在进行变量赋值的时候，存储在变量中的值也会被复制到新变量的位置，此时所复制的是一个指针，该指针指向在堆内存中的对象，两个变量此时都指向同一个对象，因此任意一个变量对其属性进行修改，都将会在另一个变量上表现出来
- 用 instanceof 判断引用值的类型
			

## ⭐️属性

## ⭐️方法



### preventDefault()|DOM event

[[DOM]]

阻止**特定事件的默认动作**，比如链接的默认行为是单击时导航到 href 指定的 url，如果想阻止这个行为，可以使用这个方法

```html
link.onclick = function(event){
	event.preventDefault();
}
```

### stopPropagation()|DOM

立即阻止事件流在 DOM 中传播，取消后续的事件捕获或**冒泡**。

```ad-quote
[[事件#事件冒泡和事件捕获]]
```


```javascript
let btn = document.getElementById("myBtn"); 
btn.onclick = function(event) {    
  console.log("Clicked");    
  event.stopPropagation(); 
};   
document.body.onclick = function(event) {   
  console.log("Body clicked"); 
};
```

btn 不调用stopPropagation的话会打印两条，调用了则不会触发 document.body.onclick 

### String.fromCharCode(Num)

从数字转换成 ASCII



### Math.random()

随机[0,1)



### toUpperCase()

字符串转换为全部大写



### setTimeout()|Window

回调函数，模拟无法立即执行的函数

`setTimeout(函数, 时间 )`用于在指定**毫秒**后调用函数或计算表达式

`setTimeout("alert('对不起, 要你久候')", 3000 )`

```javascript
var p = new Promise(()=>{})
setTimeout(console.log,0,p);
```







## ⭐️读代码

1.

```
(()=>{}).length
1&2
+[]
[1,2,-3].reduce((a, b) => a - b, 0)
```

1. (()=>{}).length; 获取方法形参个数，形参为0
2. 1=0001 2=0010 按位与运算，同为1才为1，否则返回0
3. +[] 隐式类型转换，因为[]是对象，所以toPrimitive->valueOf->toString为''，结果就是+''===0
4. reduce对数组中的每个元素执行一个reducer函数(升序执行)，将其结果汇总为单个返回值。a为累计器累计回调的返回值，b为数组的每一项元素，传入初始值0->0-(1)->(-1)-2->(-3)-(-3)->0



## ⭐️实用

### 接收用户输入

```javascript
window.onload = function(){
     var input = document.getElementById("input");
     input.onblur = function(){
         alert(input.value)
     } 
}
```



### console.time() &console.timeEnd()计时器

在需要测试的代码段前	`console.time("<TEST>")`

结束的部分  `console.timeEnd("<TEST>")`

<TEST>为传递的参数值，作为计时器的参数

## ⭐️default（暂时不知道归类到哪）

### 模板字面量
```ad-quote
[[数据类型#模板字面量]]
```

