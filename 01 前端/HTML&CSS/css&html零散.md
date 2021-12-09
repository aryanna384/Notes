[TOC]

## ⭐️易混淆定义

### 包含关系·html

1. p元素不能包含任何块级元素(包括自身)
2. a元素可以包含任何其他元素(除了自身)
3. document是文档(整个DOM树)的根节点

### 控件

- input[[标签input]]：规定用户可以在其中输入数据的输入字段
- select：多选/单选下拉菜单
- textarea：多行文本
- label：展示文本框
- filedset：对表单中的相关元素进行分组，会在表单元素周围绘制边框

### 超文本

**超文本**是用**超链接**的方法，将各种不同空间的文字信息组织在一起的网状文本。

## ⭐️难点

### 客户端存储数据


#### cookie

- 暂时将一些数据存储在电脑中，默认有效期是浏览器关闭 cookie 就删除，浏览器关闭了也不会消失
- 作用：
  - 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
  - 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录

要求服务器在相应 HTTP 请求时，通过发送 set-cookieHTTP 头部包含会话信息。 

设置 cookie，注意需要服务器打开才行地址栏是 localhost

```html
<script>
  //设置 cookie
  document.cookie = " xm = aki;";
</script>
```

<img src="/Users/ynchen/Library/Mobile Documents/com~apple~CloudDocs/Typora笔记/NOTE/css&html零散.assets/截屏2021-05-19 13.42.44.png" alt="截屏2021-05-19 13.42.44" style="zoom:50%;" />

`expirse`设置有效期

####  Web storage

本地存储技术，在客户端保存数据的功能

js 为了减少与服务器的通信，经常会用到保存的数据到本地的功能，例如本地用户信息保存

作用：

- 提供在 cookie 之外的存储会话数据的途径; 

- 提供跨会话持久化存储大量数据的机制

客户端存储两个对象：

- localStorage：用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除

- sessionStorage：存储会话数据，即用于临时保存统一窗口（或标签页）的数据，在关闭窗口或标签页之后会删除

localStorage和 sessionStorage是 window 对象下的一个属性对象

## ⭐️属性和标签

### animation-fill-mode

`animation-fill-mode:forwards`   当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式

### object-fit

object-fit 属性指定元素的内容应该如何去适应指定容器的高度与宽度。

object-fit 一般用于 img 和 video 标签，一般可以对这些元素进行保留原始比例的剪切、缩放或者直接进行拉伸等。

cover 保持原有比例但可能被裁切

### 自定义属性

`--*`来声明变量名，`var(--*)`来使用

```css
{
    --text-color-light:#cccc;
}
{
  color:var(--text-color-light);
}
```

### text-align | Atr

文本水平对齐方式

### min-width | Atr

设置段落最小宽度

### z-index 叠放顺序

一定是在` position: obsolute `时才奏效

正值离用户更近，负值离用户更远

### Data-*·html|属性

[自定义数据](https://www.w3school.com.cn/tags/att_global_data.asp)

用于存储页面或应用程序的私有自定义数据

使用例：[[vue#Ex3 实现点击 tab 切换页面的功能]]、

获取：

```javascript
对象.target.dataset.index
```

### word-break·CSS|属性

<span style="color:red">word-break</span> 属性：规定单词的自动换行的处理方法，可以让浏览器实现在任意位置的换行

```css
word-break:nomal|break-all|keep-all;
```

normal：使用浏览器默认的换行规则

break-all：允许在单词内换行

keep-all：只能在半角空格或连字符处换行

```html
<style> 
p.test1
{
  width:11em; 
  border:1px solid #000000;
  word-break:break-all;//自动换行
}
</style>
<p class="test1">This is a veryveryveryveryveryveryveryvery-veryvery long paragraph</p>
```

<img src="./images/截屏2021-05-19 18.21.47.png" alt="截屏2021-05-19 18.21.47" style="zoom:50%;" />

### white-space·CSS|属性

<span style="color:red">white-space </span>属性规定段落中的的换行方式

(https://developer.mozilla.org/zh-CN/docs/Web/CSS/white-space)

设置如何处理元素中的空白 (en-US)。

```css
white-space:normal|pre|nowrap|pre-wrap|pre-line|inherit
```

```html
<p>
  床 前    明月光，疑是地上霜
  举头望明月，低头	思故乡
</p>
//床(空格)前(空格)(空格)(空格)(空格)明月光，疑是地上霜（回车）举头望明月，低头(Tab)思故乡
```

<img src="../../images/截屏2021-05-19 13.06.01.png" alt="截屏2021-05-19 13.06.01" style="zoom:50%;" />

| 属性         | 效果                                                 | 兼容               |
| :----------- | :--------------------------------------------------- | :----------------- |
| normal(默认) | 所有空格、回车、制表符都合并成一个空格，文本自动换行 | IE7\IE6+           |
| nowrap       | 所有空格、回车、制表符都合并成一个空格，文本不换行   | IE7\IE6+           |
| pre          | 所有东西原样输出，文本不换行                         | IE7\IE6+           |
| pre-wrap     | 所有东西原样输出，文本换行                           | IE8+               |
| pre-line     | 所有空格、制表符合并成一个空格，回车不变，文本换行   | IE8+               |
| *inherit*    | *继承父元素*                                         | *IE不支持，不推荐* |

---



### date·html | 属性

```
<input type="date" name="bday">
<input type="datetime-local" name="bdaytime">//不带时区
```

![截屏2021-05-19 14.37.17](../../images/截屏2021-05-19 14.37.17.png)

![截屏2021-05-19 14.37.44](../../images/截屏2021-05-19 14.37.44.png)

---

### Bootstrap 按钮组

btn-group 能将按钮组成按钮组

btn-toolbar 能将 btn 做成复杂组件

btn-group 可以嵌套使用

可以使用 btn-group-lg，btn-group-sm 来调整按钮大小

> Bootstrp 是 twitter推出的一个插件
>
> 使用时在header 里添加
>
> ```html
> 	<script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
> ```

---

### HTML5新增规范

sessionStorage、localStorage(Web storage)(JS)

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。



### flex·CSS|属性

让所有弹性盒模型对象的子元素都有相同的长度，且忽略它们内部的内容

```
[[main]] div
{
    flex:1;
}
```



### target·html | 属性

`a href=“link” target=“_blank”`

在新标签打开页面

标签的target属性有5个值： 

- _self：在当前框架中打开链接

- _blank：在全新的空白窗口中打开链接

- _top：在顶层框架中打开链接

- _parent：在当前框架的上一层打开链接 framename：在指定的框架或浮动框架内打开链接（框架名可以自定义）



### float ·CSS|属性

下面这个wrap 的 height是多少？

```html
.a,.b,.c {
            box-sizing: border-box;
            border: 1px solid;
        }

        .wrap {
            width: 250px;
        }
        .a {
            width: 100px;
            height: 100px;
            float: left;
            background-color: aqua;
        }
        .b {
            width: 100px;
            height: 50px;
            float: left;
            background-color: bisque;
        }

        .c {
            width: 100px;
            height: 100px;
            display: inline-block;
            background-color: blue;
        }
<div class="wrap">
  <div class="a">a</div>
  <div class="b">b</div>
  <div class="c">c</div>
</div>
```

<img src="../../images/截屏2021-05-18 12.33.17.png" alt="截屏2021-05-18 12.33.17" style="zoom:50%;" />

答案：150px

其实和 c 不指定 inline-block 指定 float：left 是一样的

a、b没有浮动的情况

<img src="../../images/截屏2021-05-18 12.33.52.png" alt="截屏2021-05-18 12.33.52" style="zoom:50%;" />

c 没有 inline-block 的情况

<img src="../../images/截屏2021-05-18 12.34.27.png" alt="截屏2021-05-18 12.34.27" style="zoom:50%;" />

c右浮动的情况

<img src="../../images/截屏2021-05-18 12.34.30.png" alt="截屏2021-05-18 12.34.30" style="zoom:50%;" />

### 空格·html|标签

`&nbsp;`实体，需要几个写几个

`<br>`

### textarea·html|标签

  多行的文本输入控件

<textarea>
## 
## ⭐️实用
## ⭐️实用方法
### 强制在一行显示，超出部分显示省略号

```css
white-space:nowrap;
overflow:hidden; 
text-overflow: ellipsis;
```

### 鼠标经过时变成手的图标·css|属性

规定要显示的光标的类型（形状）

[可能的值](https://www.w3school.com.cn/cssref/pr_class_cursor.asp)

`cursor:pointer；`

### 通栏样式的设置

和浏览器一样宽，不需要设置宽度

### 去除列表样式

`li{list-style:none;}` 



## ⭐️对比和区别

### id & class

- 在 html 中 ID 只能赋予一次而 class 可以赋予多个标签，但是浏览器不会检测 id的唯一性
- id可以配合href的锚点功能
- ID 选择符（#）不能连在一起使用而 class 选择符（.）可以
- id 样式的优先级 > class

### Canvas & SVG

SVG 为了之后的操作需要记录坐标，比较缓慢

Canvas 不能使用绘制对象的相关事件处理，因为没有他们的参考

### display & visibility

`visibility: hidden`会保留元素的空间，仅为视觉上完全透明

`display:none`不为被隐藏的对象保留其物理空间

### height & line-height

- height 是元素自身的高度， li 添加了红色的边框
- line-height 
  - 多行文本的间距
  - 块级元素：元素行盒的最小高度

<img src="../../images/image-20210510100531176.png" alt="image-20210510100531176" style="zoom:50%;" />



### margin & padding

margin：上 右 下 左 逆时针

padding：上 右 下 左 

在显示效果上差不多，但是什么情况下使用 margin 什么时候使用 padding？

`<li><a href="#">话费</a></li>`

比如这个情况，效果是

<img src="../../images/image-20210510102027223.png" alt="image-20210510102027223" style="zoom:50%;" />

border 内的部分都是可以点击的，算是 a 连接的一部分，但是 margin 就点击不了



