[TOC]



> JavaScript的组成
>
> JavaScript基础分为三个部分：
>
> - ECMAScript：JavaScript的语法标准。包括变量、表达式、运算符、函数、if语句、for语句等。
> - ==DOM：文档对象模型（Document object Model）==
> - BOM：浏览器对象模型（Browser object Model）。



## 概念

### DOM

DOM：Document Object Model，文档对象模型。

- W3C 组织推荐的处理可扩展标记语言（HTML 或 XML）的标准编程接口；

- W3C已经定义了一系列 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式。
- 对于 JS，为了能使 JS 操作 HTML，JS 就有了一套自己的 DOM 编程接口
- 对于 HTML，DOM 使得 HTML 形成一棵 DOM 树，包含文档、元素、节点

DOM树：

![img](http://img.smyhvae.com/20180126_2105.png)

获取到的 DOM 元素是一个对象，所以叫文档「对象」模型。

### 节点

节点（Node）：构成 HTML 网页的最基本单元。网页中的每一个部分都可以称为是一个节点，比如：html标签、属性、文本、注释、整个文档等都是一个节点。

虽然都是节点，但是实际上他们的具体类型是不同的。常见节点分为四类：

- 文档节点（文档）：整个 HTML 文档。整个 HTML 文档就是一个文档节点。
- 元素节点（标签）：HTML标签。
- 属性节点（属性）：元素的属性。
- 文本节点（文本）：HTML标签中的文本内容（包括标签之间的空格、换行）。

节点的类型不同，属性和方法也都不尽相同。所有的节点都是Object。一共有 12 种节点类型，返回值为（1-12）

### DOM可以做什么

- 找对象（元素节点）
- 设置元素的属性值
- 设置元素的样式
- 动态创建和删除元素
- 事件的触发响应：事件源、事件、事件的驱动程序

## Node类型

### Type、nodeName、nodeValue

nodeType ，共 12 种

|节点| nodeType      | nodeNmae | nodeValue |
| ------------- | -------- | --------- | ------------- |
| Element元素节点 | nodeType == 1 |           ||
| Attr属性节点 | nodeType == 2 | ||
| Text文本节点 | nodeType == 3 | ||
| Comment注释节点 | nodeType == 8 | ||
| Document文档节点 | nodeType == 9 | ||

```HTML
<div id="box" value="111">生命壹号</div>
```

这个标签包含了三种节点：

- 元素节点（div）
- 属性节点  id
- 文本节点 “生命壹号”

获取三个节点的：nodeType、nodeName、nodeValue

- nodeValue 始终为 null

- nodeName 始终等于元素的标签名

```javascript
var element = document.getElementById("box1");  //获取元素节点（标签）
var attribute = element.getAttributeNode("id"); //获取box1的属性节点
var txt = element.firstChild;                   //获取box1的文本节点

//获取nodeType
console.log(element.nodeType);       //1
console.log(attribute.nodeType);     //2
console.log(txt.nodeType);           //3

console.log("--------------");

//获取nodeName
console.log(element.nodeName);       //DIV
console.log(attribute.nodeName);     //id
console.log(txt.nodeName);           //[[text]]

console.log("--------------");

//获取nodeValue
console.log(element.nodeValue);     //null
console.log(attribute.nodeValue);   //box1
console.log(txt.nodeValue);         //生命壹号
```

### 节点关系

| 父节点     | 兄弟节点               | 子节点            | 所有子节点 |
| ---------- | ---------------------- | ----------------- | ---------- |
| parentNode | nextSibling            | firstChild        | childNode  |
|            | nextElementSibling     | firstElementChild | children   |
|            | previousSibling        | lastChild         |            |
|            | previousElementSibling | lastElementChild  |            |

节点的访问关系，是以**属性**的方式存在的。

```javascript
someNode.firstNode == someNode.childNode[0] == someNode.childNode.item(0)
someNode.lastNode == someNode.childNode[someNode.length-1]
```

<img src="../../images/截屏2021-05-20 13.32.39.png" alt="截屏2021-05-20 13.32.39" style="zoom:50%;" />



## 常用节点的操作总结

|                             | 方法                                                         | 备注                                                         |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建                        | document.write()                                            | 使用少 |
|                             | element.innerHTML                                            |                                                              |
|                             | document.createElement()                                     |                                                              |
| 增加                        | appendChild                                                  | 末尾添加节点；<br />在节点 1 添加已有节点 2，节点 2 会移动至节点 1 下而不是复制 |
|                             | insertBefore                                                 | 父节点.insertBefore(newNode,refreceNode)                     |
| 删除                        | removeChild                                                  | someNode.removeChild(someNode.somechildnode);<br />node1.parentNode.removeChild(node1); |
| 修改[[#DOM对象的属性]]                        | 修改元素属性：src、href、title 等                            |                                                              |
|                             | 修改普通元素内容：innerHTML、innerText 等                    |                                                              |
|                             | 修改表单元素：value、type、disable 等                        |                                                              |
|                             | 修改元素样式：style、className 等                            | someNode.replaceChild(newNode,replacedNode)                  |
| 替代                        | replaceChild                                                 |                                                              |
| 复制                        | cloneNode                                                    | 要复制的节点.cloneNode();     //默认 false，只复制节点本身<br/>要复制的节点.cloneNode(true);//深复制，复制节点及所有子节点 |
| 查找                        | DOM 提供的 API 方法：getElementById、getElementsByTagName （不推荐） |                                                              |
|                             | H5提供：querySelector、querySelectAll（推荐）                |                                                              |
|                             | 利用节点操作获取元素：parentNode、chilren、previousElementSibling、nextElementSibling |                                                              |
| 属性操作[[#节点属性操作]] | setAttribute设置                                             |                                                              |
|                             | getAttribute 获取                                            |                                                              |
|                             | removeAttribute 移除                                         |                                                              |
| 事件                        | 鼠标事件 onclick、onmouseover、onmouseout、onfocus、onblur、onmousemove、onmouseup、onmousedown |                                                              |

### 创建

`ocument.write()`

```html
document.write("<div>123</div>");
```

直接写入页面的内容流，当文档流执行完毕，会导致页面全部重绘

>文档流：.html 代码全部执行完，此时再写入会使得重新绘制一个页面，之前的页面就被覆盖了
>
>如果是顺序执行问题不大，但是如果是鼠标、按钮等事件，这类事件往往是文档流执行完才会用户触发，是缺陷所以用的很少

`element.innerHTML`

- value：标签的value属性。
- **innerHTML**：双闭合标签里面的内容（识别标签）。
- **innerText**：双闭合标签里面的内容（不识别标签）。（老版本的火狐用textContent）

```html
somenode.innerHTML ="<a href='#'>百度</a>";
```

在循环创建多内容时效率很慢，因为字符串拼接。

`document.createElement()`

```html
element = document.createElement("a");//创建 a 标签复制给 element
parentNode.appendChild(element);//给父元素添加新天的元素
```

==创建多个元素时==

`element.innerHTML`的效率更高，必须采用数组拼接模式，结构稍复杂

`document.createElement()`效率稍低，但是结构更清晰

### 节点属性操作

统一拿下面这个标签来举例：

```html
<img src="images/1.jpg" class="image-box" title="美女图片" alt="地铁一瞥" id="a1">
```

下面分别介绍。

1、获取节点的属性值

**方式1**：

```
元素节点.属性名;
元素节点[属性名];
```

举例：（获取节点的属性值）

```html
<body>
<img src="images/1.jpg" class="image-box" title="美女图片" alt="地铁一瞥" id="a1">

<script type="text/javascript">
    var myNode = document.getElementsByTagName("img")[0];

    console.log(myNode.src);
    console.log(myNode.className);    //注意，是className，不是class
    console.log(myNode.title);

    console.log("------------");

    console.log(myNode["src"]);
    console.log(myNode["className"]); //注意，是className，不是class
    console.log(myNode["title"]);
</script>
</body>
```

上方代码中的img标签，有各种属性，我们可以逐一获取，打印结果如下：

![img](http://img.smyhvae.com/20180127_1340.png)

**方式2**：

```
元素节点.getAttribute("属性名称");
```

举例：

```
console.log(myNode.getAttribute("src"));
console.log(myNode.getAttribute("class"));   //注意是class，不是className
console.log(myNode.getAttribute("title"));
```

打印结果：

![img](http://img.smyhvae.com/20180127_1345.png)

方式1和方式2的区别在于：前者是直接操作标签，后者是把标签作为DOM节点。推荐方式2。

2、设置节点的属性值

方式1）

```
myNode.src = "images/2.jpg"   //修改src的属性值
myNode.className = "image2-box";  //修改class的name
```

方式 2

```
myNode.setAttribute("src","images/3.jpg");
myNode.setAttribute("class","image3-box");
myNode.setAttribute("id","你好");
```

 3、删除节点的属性

举例：（删除节点的属性）

```
myNode.removeAttribute("class");
myNode.removeAttribute("id");
```

**总结：**

获取节点的属性值和设置节点的属性值，都有两种方式，但这两种方式是有区别的。

- 方式一的`元素节点.属性`和`元素节点[属性]`:绑定的属性值不会出现在标签上。
- 方式二的`get/set/removeAttribut`: 绑定的属性值会出现在标签上。

这其实很好理解，方式一操作的是属性而已，方式二操作的是标签本身。

另外，需要注意的是：**这两种方式不能交换使用**，get值和set值必须使用用一种方法。

举例：

```html
<body>
<div id="box" title="主体" class="asdfasdfadsfd">我爱你中国</div>
<script>

    var div = document.getElementById("box");

    //采用方式一进行set
    div.aaaa = "1111";
    console.log(div.aaaa);    //打印结果：1111。可以打印出来，但是不会出现在标签上

    //采用方式二进行set
    div.setAttribute("bbbb","2222");    //bbbb作为新增的属性，会出现在标签上

    console.log(div.getAttribute("aaaa"));   //打印结果：null。因为方式一的set，无法采用方式二进行get。
    console.log(div.bbbb);                   //打印结果：undefined。因为方式二的set，无法采用方式一进行get。

</script>
</body>
```

## DOM对象的属性

DOM对象的属性和HTML的标签属性几乎是一致的。例如：src、title、className、href等。


- 通过`someNode.style `属性修改样式

```
var d1 = document.querySelector("[[d1]]");
d1.style.width = "200px";
d1.style.height = "200px";
d1.style.backgroundColor = "yellow";
```


- 通过添加`className`

```
.redBg{
	background-color: blue;
}

var d2 = document.querySelector("[[d2]]");
d2.className = "redBg";
```

- 通过`classList.add/remove/replace `改变类名增加样式

```
[class~=shadow]{
  width: 200px;
  height: 200px;
}

d2.classList.add("shadow");//class=redBg shadow
```

- 通过在 body 中添加 style 标签

```html
<div id="d3" class="donghuad3">helloworld 3</div>

// 创建标签
var style=document.createElement("style");
// 反引号可以包括多行字符串
style.innerHTML=` .donghuad3{
  width: 300px;
  height: 300px;
  background-color: skyblue;
  animation: donghua 3s alternate infinite;
}
@keyframes donghua {
  from{
  transform: translate(0,0);

  }
  to{
  transform: translate(600px,0);
  } 
}
var body = document.body;
body.appendChild(style);
```

## DocumentFragment 节点

作用：避免多次渲染

使用场景：

动态创建多个节点

```javascript
var oListWrapper = document.getElementsByTagName('ul')[0];
        for(var i=0;i<10;i++){
            	var oLi = document.createElement("li");
            	oLi.innerHTML = "列表" + (i+1) +"项";
           		(function(i){
                		oLi.onclick = function(){
                    		console.log(this.innerText);
                   		 console.log("点击",i);
               	}
            })(i);
            oListWrapper.appendChild(oLi);//给静态元素不断放入新的元素，会重绘重拍，影响性能
        }
```

`DocumentFragment`

文档片段本身永远不会被添加到文档树，可以通过` appendChild()`或 `insertBefore()`方法将文档片段的内容添加到文档。

```javascript
var oListWrapper = document.getElementsByTagName('ul')[0];
        var oFrag = document.createDocumentFragment();
        for(var i=0;i<10;i++){
            	var oLi = document.createElement("li");
            	oLi.innerHTML = "列表" + (i+1) +"项";
           		(function(i){
                		oLi.onclick = function(){
                    		console.log(this.innerText);
                   		 console.log("点击",i);
               	}
            })(i);
            oFrag.appendChild(oLi);
        }
oListWrapper.appendChild(oFrag);
```

