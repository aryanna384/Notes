[TOC]



> JavaScript的组成
>
> JavaScript基础分为三个部分：
>
> - ECMAScript：JavaScript的语法标准。包括变量、表达式、运算符、函数、if语句、for语句等。
> - DOM：文档对象模型（Document object Model）。
> - ==BOM：浏览器对象模型（Browser object Model）==

## BOM 概述

- 提供了独立于内容而与浏览器窗口进行交互的对象，核心对象是 `window`对象

- 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性

- 缺乏标准，JS 标准化组织是 ECMA，DOM 标准化组织是 W3C，BOM 最初是 Netscape 浏览器标准的一部分

==window对象==

- JS 访问浏览器的接口
- 全局对象，是 JS 中的 Global、this、frame、self
  - window.alert()、window.console

### DOM和 BOM

DOM（document） 是 BOM 的一部分

![Snipaste_2021-05-29_12-06-52](../../images/Snipaste_2021-05-29_12-06-52.png)

<img src="../../images/截屏2021-05-29 12.10.04.png" alt="截屏2021-05-29 12.10.04" style="zoom:50%;" />

<img src="../../images/Snipaste_2021-05-29_12-08-49.png" alt="Snipaste_2021-05-29_12-08-49" style="zoom:75%;" />



## 事件

### 窗口加载事件

浏览器在加载一个页面时，是按照自上向下的顺序加载的，读取到一行就运行一行。如果将script标签写到页面的上边，在代码执行时，页面还没有加载，页面没有加载DOM对象也没有加载，会导致无法获取到DOM对象。

```javascript
window.onload()
window.addEventListener("load",function(){});
```

`onload` 事件会在整个页面加载完成之后才触发。为 window 绑定一个onload事件，该事件对应的响应函数将会在页面加载完成之后执行，这样可以确保我们的代码执行时所有的DOM对象已经加载完毕了。

注意：多个 onload 事件存在的情况下：

onload()的方式只会执行最后一个 onload事件

`window.addEventListener("load",function(){})`则没有限制

```javascript
document.addEventListener("DOMContentLoaded",function(){});
```

仅当 DOM 加载完成，不包括样式表，图片、flash 等。当页面图片很多时，从用户访问到 onload 触发可能需要较长事件，交互效果不能实现，用户体验就很差。此时使用 DOMContentLoaded。此时的执行顺序是DOMContentLoaded>onload。

### 调整窗口大小事件

```javascript
window.onresize();
window.addEventListener("resize",function(){});
```

可以用来做响应式布局

`window.innerWidth` 浏览器窗口宽度

### 定时器

设置定时器

```javascript
window.setTimeout(回调函数，延迟的毫秒数);
window.setInterval(回调函数，[间隔的毫秒数]);
```

`setTimeout`隔一段时间之后调用回调函数

`setInterval`每隔一段时间调用回调函数

可以给定时器声明变量

```javascript
var timeout1 = window.setTimeout();
```

清除定时器

```javascript
window.clearTimeout(timeoutID);//timeoutID 为设置的定时器的标识符
window.clearInterval(intervalID);//intervalID最好是全局对象var timer=null
```

## Location 对象

URL 语法格式

```
protocol:// hostname[:port] / path / [;parameters][?query]#fragment
```



| 对象属性          | 返回值                              |
| ----------------- | ----------------------------------- |
| location.href     | 获取或设置整个 URL                  |
| location.host     | 返回主机域名                        |
| location.port     | 返回端口号，如未写返回空字符串      |
| location.pathname | 返回路径                            |
| location.search   | 返回参数                            |
| location.hash     | 返回片段，#后面内容，常见于链接锚点 |

| 方法               | 返回值                                   |
| ------------------ | ---------------------------------------- |
| location.assign()  | 跳转页面，可以记录浏览历史，实现后退功能 |
| location.replace() | 替代当前页面，不可以记录浏览历史         |
| location.reload()  | 刷新页面                                 |

案例

```html
//login.html

<form action="index.html">//表单提交到 index.html
        用户名 <input type="text" name="username">
        <input type="submit" name="" value="登录">  //提交之后会自动跳转到 index.html
</form>

//index.html
<script>
        var username = location.search.substr(1).split('=')[1];//获取到的是 ?username=value , 提取字符串、分割得到 value
</script>
```



## Navigator 对象

对浏览器历史记录的操作

| 方法               | 返回值                                              |
| ------------------ | --------------------------------------------------- |
| navigator.foward() | 前进                                                |
| navigator.back()   | 后退                                                |
| navigator.go()     | go(1)== forward，是前进一个，go(-1)==back是后退一个 |

## history 对象

history 对象表示当前窗口首次使用以来用户的导航历史记录。
