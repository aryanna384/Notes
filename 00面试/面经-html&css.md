# html&css

## form表单的属性 

```
<form 
	action=""  // 处理表单提交的url
	method="get"//提交方式
	enctype=""//决定表单向服务器发送数据的编码格式>
</form>
```

1. 当`method="get"`时，`enctype`无效，数据以URL查询方式发送

当`method=”post“`时，ectype将表单的内容提交给服务器的MIME类型，可能的取值有：

2. `enctype=''`

`application/x-www-form-urlencoded`：未指定属性时的默认值。

http请求如下：

```
Content-Type: application/x-www-form-urlencoded
```

3. `enctype="multipart/form-data"`：二进制方式发送，混合格式发送，当表单包含 `type=file` 的 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input) 元素时使用此值。

4. `enctype='text/plain'`：纯文本方式发送。

```
Content-Type: application/x-www-form-urlencoded
```

## 虚拟DOM和真实DOM

虚拟DOM：是用JavaScript对象模拟出一个DOM节点，把这个`JS`对象就称为是这个真实`DOM`节点的虚拟`DOM`节点

```
// 真实DOM
<div class="a" id="b">我是内容</div>
// 虚拟DOM
{
  tag:'div',        // 元素标签
  attrs:{           // 属性
    class:'a',
    id:'b'
  },
  text:'我是内容',  // 文本内容
  children:[]       // 子元素
}
```

作用/产生原因：对虚拟DOM修改时不会引起**回流**和**重绘**，通过**diff算法**比较虚拟DOM和真实DOM的不同，找出最小变更，再把这些变更写入真实的DOM中，**减少了实际DOM操作次数，性能会有较大提升。**（用`JS`的计算性能来换取操作`DOM`的性能）

> Diff 算法

# CSS

## 伪元素和伪类

伪类和伪元素的根本区别在于：**它们是否创造了新的元素。**

伪元素/伪对象：不存在在DOM文档中，是虚拟的元素，是创建新元素。代表某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。

## 伪类和伪元素

###  伪类

伪类是选择器的一种，它用于选择处于特定状态的元素，比如当它们是这一类型的第一个元素时，或者是当鼠标指针悬浮在元素上面的时候。伪类选择符`:`

| 伪类         |                |                          |
| ------------ | -------------- | ------------------------ |
| 简单伪类     | :first-child   |                          |
| 动态伪类     |                |                          |
| 超链接伪类   | :link          |                          |
|              | :visited       |                          |
| 用户行为为例 | :hover         |                          |
| 结构伪类     | :root          | 选择文档的根元素         |
|              | :nth-child(N)  | 第 N 个子元素            |
|              | :last-child    | 最后一个子元素           |
|              | nth-of-type(N) | 选择第 N 个 p 元素的每个 |

```html
<style>
	p:nth-of-type(3) {
	      background: red;
    }

	p:nth-child(3) {
    	  background: yellow;
    }
</style>

<div id="wrap">
    <p>one</p>
    <div>我是div</div>
    <p>two</p>
    <p>three</p>
    <p>four</p>
    <p>five</p>
    <p>six</p>
</div>
```

`p:nth-of-type(3)`代表的就是，所有兄弟节点中找标签为p的，然后找到的第三个p标签背景为红色

`p:nth-child(3)`找父元素（wrap)的第三个子元素，如果该子元素为p，则其变为黄色，如果，第三个子元素不是p元素，则没有子元素的背景变为黄色

### 伪元素

| 伪元素 |                |            |
| ------ | -------------- | ---------- |
|        | ::first-line   | 第一行     |
|        | ::first-letter | 第一个字符 |
|        | ::before       |            |
|        | ::after        |            |



## 清除浮动

问题：父元素不方便给高度，子元素全部浮动时，父元素高度为 0，下面再给个标准流的盒子时，就会接在父元素下面被压在浮动的子元素下面，（理应是接着子元素的）

注意：父元素有高度时不需要清除浮动

**清除浮动方法**

1. （不常用）额外标签法

在最后一个浮动的子元素后面额外建立标签，添加清除浮动样式

**新增的盒子必须是块级元素**

`clear : none/left/right/both`

```html
.clear{
	clear:both
}
<div Class="clear"></div>
```

2. 父级添加 overflow

`  overflow:hidden/auto/scroll;`

```html
.parent{
  overflow:hidden;
}
```

代码简洁但是无法显示溢出的部分

3. after 伪元素

父元素添加伪元素，额外标签法的升级版

```html
.clearfix:after{
	content:"";
	display:block;
	height:0;
	clear:both;
	visibility:hidden;
}
<div class = "box clearfix">
</div>
```

4. 双伪元素

```html
.clearfix:before,
.clearfix:after:{
	content:""; // content必须要有
	display:table;
}
<div class = "box clearfix">
</div>
```

## 盒模型

margin、border、padding、content

盒子宽高的计算方式会根据盒子的类型而不同

盒子的类型通过 `box-sizing `设置

| 类型                       | 宽度                   |
| -------------------------- | ---------------------- |
| `box-sizing ：content-box` | width 为内容区域的大小 |
| `box-sizing :  border-box` | width 为整个盒子的大小 |

## BFC块级上下文

**块级格式化上下文（BFC，Block formatting context）**

它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。触发bfc的元素会脱离**文档流**。脱离文档流之后，将不再在文档流中占据空间。

- BFC 创建条件：

  - `float : 不为none`
  - `position : absolute | fixed`
  - `display : inline-block | table-cell | table-caption | flex | inline-flex`
  - `overflow : hidden | auto | scroll（不为visible）`
  - 父元素与正常文件流的子元素（非浮动）自动形成一个BFC

- BFC 布局规则/特点：
  - 内部的Box会在垂直方向，一个接一个地放置。所有子元素（包含浮动元素）与容器（父元素）左边对齐。Box垂直方向的距离由`margin`决定。==<u>属于同一个BFC的两个相邻Box的margin会发生重叠</u>==。若2个元素属于不同的BFC，则垂直方向不会重叠。
  - 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
  - BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。2个同级的BFC 不会重叠。（BFC里面的同级BFC之间也不会互相影响）
  - BFC区域不会与float元素区域重叠
  - 可以自动撑开容器（若子元素是float的，父元素设置overflow:hidden，父元素就形成一个BFC）。反之也如此。
  - 计算BFC的高度时，浮动元素也参与计算。

主要作用

1）清除元素内部浮动

2）解决外边距合并

## 浮动

为什么需要浮动？

- 完成标准流无法完成的布局

- `display:inline-block`盒子之间会出现缝隙

典型应用：可以让多个块级元素一行内排列显示

<span style="color:red">纵向排列--标准流，横向排列--浮动</span>

1. 浮动和标准流的父盒子搭配

先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置

2. 一个元素浮动了，理论上其余兄弟元素也要浮动

浮动的盒子只会影响盒子后面的标准流不会影响前面的标准流

应用场景：文字包围图片、首字符下沉、多列布局

### 父元素高度塌陷

场景：包含浮动子元素的父元素高度会变成 1，解决：

```css
.father:after{
    content:"";
    display:block;
    clear:both;
}
```

### 浮动

问题：

父元素不方便给高度，子元素全部浮动时，父元素高度为 0，下面再给个标准流的盒子时，就会接在父元素下面被压在浮动的子元素下面，（理应是接着子元素的）

父元素有高度时不需要清除浮动

![截屏2021-05-18 14.06.52](/Users/ynchen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Vault/00前端/images/截屏2021-05-18 14.06.52.png)

![截屏2021-05-18 14.10.45](/Users/ynchen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Vault/00前端/images/截屏2021-05-18 14.10.45.png)

### 清除浮动方法：

1. 额外标签法

在最后一个浮动的子元素后面额外建立标签，添加清除浮动样式（不常用）

**新增的盒子必须是块级元素**

`clear : none/left/right/both`

```html
.clear{
	clear:both
}
<div Class="clear"></div>
```

2. 父级添加 overflow

`  overflow:hidden/auto/scroll;`

```html
.parent{
  overflow:hidden;
}
```

代码简洁但是无法显示溢出的部分

3. after 伪元素

父元素添加伪元素，额外标签法的升级版

```html
.clearfix:after{
	content:"";
	display:block;
	height:0;
	clear:both;
	visibility:hidden;
}
<div class = "box clearfix">
</div>
```

4. 双伪元素

```html
.clearfix:before,
.clearfix:after:{
	content:"";
	display:table;
}
<div class = "box clearfix">
</div>
```

## flex布局

行内元素开启flex布局后会变成块状元素。

使用`display:inline-flex`; 如果没有指定父元素的宽度，那么父元素的宽度就由里边的内容来撑开。

设置为 flex 布局之后，子元素的`float`、`clear`和`vertical-align`属性将失效。

主轴和交叉轴垂直

### 容器属性

| 容器属性          | 含义                                                         | 值**（默认）**                                               | 示例                                                         |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `flex-direction`  | 设置主轴方向，row 为起始左终止右                             | column-reverse \| column \| **row** \| row-reverse           | <img src=".images/截屏2021-05-31 16.16.25.png" alt="截屏2021-05-31 16.16.25" style="zoom:33%;" /> |
| `flex-wrap`       | 如果一条轴线排不下，如何换行                                 | **nowrap** \| wrap \| wrap-reverse                           | <img src="./images/截屏2021-05-31 16.19.32.png" alt="截屏2021-05-31 16.19.32" style="zoom:33%;" /> |
| `flex-flow`       | flex-direction属性和flex-wrap属性的简写形式                  | **row nowrap**                                               |                                                              |
| `justify-content` | 项目在主轴上的对齐方式                                       | **flex-start** \| flex-end \| center \| space-between \| space-around | <img src="./images/截屏2021-05-31 16.21.42.png" alt="截屏2021-05-31 16.21.42" style="zoom:33%;" /> |
| `align-items`     | 在交叉轴上如何对齐                                           | flex-start \| flex-end \| center \| baseline \| **stretch**  | <img src="./images/截屏2021-05-31 16.22.06.png" alt="截屏2021-05-31 16.22.06" style="zoom:33%;" /> |
| `align-content`   | 定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用 | flex-start \| flex-end \| center \| space-between \| space-around \| **stretch** | <img src="./images/截屏2021-05-31 16.22.49.png" style="zoom:33%;" /> |

子元素属性

| 属性          | 说明                                                         | 值**（默认）**                                               | 示例                                                         |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `order`       | 项目的排列顺序                                               |                                                              | <img src="./images/截屏2021-05-31 16.25.44.png" alt="截屏2021-05-31 16.25.44" style="zoom:33%;" /> |
| `flex-grow`   | 属性定义项目的放大比例                                       | **0**                                                        | <img src="./images/截屏2021-05-31 16.26.30.png" alt="截屏2021-05-31 16.26.30" style="zoom:33%;" /> |
| `flex-shrink` | 定义了项目的缩小比例,负值对该属性无效                        | **1**                                                        |                                                              |
| `flex-basis`  | 在分配多余空间之前，项目占据的主轴空间                       | **auto**                                                     |                                                              |
| `flex`        | Flex-grow flex-shrink flex-basis 的简写                      |                                                              |                                                              |
| `align-self`  | 允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性 | **auto** \| flex-start \| flex-end \| center \| baseline \| stretch; |                                                              |



## flex:1是什么意思？
flex 为 flex-grow（扩展比例） 、 flex-shrink（收缩比例） 和 flex-basis （子项的基础占据的空间，不可以为负数）的简写属性

当设置flex为1时，它们将等分父元素的<u>剩余空间</u>（剩余空间是flex容器的大小减去所有flex项的大小加起来的大小。）

如下，right和left将分别占据5/8和3/8的水平空间

```html
<style>
  .box{
    height: 100px;//不能少
    display: flex;//不能少
  }
  .right{
    flex:5;
    background-color: blue;
  }
  .left{
    flex:3;
    background-color: pink;
  }
</style>


<div class="box">
<div class="right"></div>
<div class="left"></div>
</div>
```

```
/* 一个值, 无单位数字: flex-grow */
flex: 2;

/* 两个值: flex-grow | flex-basis:指定 flex 元素在主轴方向上的初始大小 */
flex: 1 30px;

/* 两个值: flex-grow | flex-shrink */
flex: 2 2;

/* 三个值: flex-grow | flex-shrink | flex-basis */
flex: 2 2 10%;
```

面试题：a和b各占多少空间

>参考：https://blog.csdn.net/u010377383/article/details/79661859

```
.container {
   width: 600px;
	 display:flex;
}
.a {
  flex: 1 0 300px;
}
.b {
  flex: 2 0 100px;
}

<div class="container">
    <div class="a">a</div>
    <div class="b">b</div>
</div>
```

计算：

a: [600-(300+100)]*1/(2+1)+300 = 300 + 200 * 1/3 = 366.67

b: [600-(300+100)]*2/(2+1)+100 = 233.3

## 水平居中

`text-align`：相对它的块父元素对齐方式



| 父元素块级元素                                               | 子元素块级元素                                               | 子元素行内元素                                            | 说明 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------------- | ---- |
| √                                                            | text-align:center                                            |                                                           |      |
| text-align:center                                            |                                                              | √                                                         |      |
| text-align:center                                            |                                                              | position:absolute;<br>left:0;<br>top:0;<br>margin:0 auto; |      |
| √                                                            | margin: 0 auto                                               |                                                           |      |
| position:relative<br><span style="color:grey">ps 不设置高度的话父元素会塌陷</span> | position:absolute;<br>left:50%;<br>width:x;<br>margin-left:-(x/2); |                                                           |      |
| position:relative<br />                                      | position:absolute;<br/>left:50%;<br/>transform:translateX(-50%) |                                                           |      |

## 垂直居中

>参考：https://www.bilibili.com/video/BV167411y7m5?from=search&seid=14952174208990870575&spm_id_from=333.337.0.0

`line-hight`：设置多行元素的空间量，如多行文本的间距。对于块级元素，它指定元素行盒（line boxes）的最小高度。

 **`vertical-align`** 用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。

| 情况 | 父元素块级元素                                               | 兄弟元素                                          | 子元素块级元素                                               | 子元素行内元素     | 说明                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
|      | padding-top:x;<br>padding-bottom:x;                          |                                                   |                                                              | √                  | 父元素不能设置固定高度                                       |
|      | width:x<br>height:y                                          |                                                   | line-height:y;<br/>p标签需要额外设置 margin:0                | line-height:y;<br> | 父元素宽度必须设置得当，只可以用于单行文字，多行会超出父元素 |
|      | display:flex;<br>flex-direction:column;<br>justify-content:center |                                                   | √                                                            |                    |                                                              |
|      | Display:grid;<br>grid-template-colums:repeat(x,1fr);<br>align-items:center;<br>justify-content:center |                                                   | √                                                            |                    |                                                              |
|      | position:relative;                                           |                                                   | height:H;<br/>position:absolute;<br>top:50%;<br>transform:translateY(-50%); |                    | height:H;<br/>position:absolute;<br/>top:50%;<br/>是将子元素相对于父元素的顶部进行偏移，因此还需要将子元素向上偏移 |
|      |                                                              | display：inline-block<br />vertical-align：middle | display：inline-block；<br />hight:100%;<br />vertical-align: middle； |                    |                                                              |

## CSS 单位

| 单位 | 描述                                                         |           |
| ---- | ------------------------------------------------------------ | --------- |
| em   | 相对单位，相对于父元素的字体大小（不推荐使用）               |           |
| px   | 像素单位                                                     |           |
| rem  | 相对基础字体单位的缩放倍数                                   | 1rem=16px |
| vw   | 相对于视窗宽度的百分比，10vw= 10%视口宽度                    |           |
| vh   | 相对于视窗高度的百分比                                       |           |
| vmin | 取视窗宽度/高度较小的值的百分比                              |           |
| vmax | 相对于视窗宽度/高度较大的值的百分比                          |           |
| dp   | 像素密度，不同手机不一样                                     |           |
| fr   | 栅格布局特定单位，用来定义网格轨道大小，按比例分配剩余的可用空间 |           |



## rem和vw的使用场景

>参考：https://juejin.cn/post/6916473795490349063

## rem 和 em

>参考：https://www.bilibili.com/video/BV1P7411C7EP/?spm_id_from=333.788.videocard.-1

```
html{
	font-size:2rem; // 默认是 1rem = 16px
	font-size：10px;
}
div{
	font-size:2rem;
	// 2*16px = 32px
	// 2*10px = 20px
}
```

## 骰子3的css实现

```css
.box{
            width: 200px;
            height: 200px;
            border: 2px solid #ccc;
            border-radius: 10px;
            padding: 20px;
            display: flex;
            /* 保证在方形的对角线上 */
            justify-content: space-between;
        }
        .item{
            display: block;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: #666;
        }
        .item:nth-child(2){
            align-self: center;
        }
        .item:nth-child(3){
            align-self: flex-end;
        }
```
