## BFC块级元素

- 定义

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

**问题描述**

添加`.son1,.son2{float:left}`会使得：

浮动不占位置，父元素高度变为 0

**解决方法**：父元素设为 BFC

`.father{overflow:hidden;}`

`.father{overflow:a`

`uto;}`

`.father{position:absolute;}`

`.father{float:left;}`

**主要用到**：计算 BFC 高度时，会自动检测浮动的盒子高度

2）解决外边距合并

**问题描述**垂直的两个BFC元素设置的外边距 margin 会有部分重叠

**主要原因**：盒子垂直方向的距离由 margin 决定，属于同一个 BFC 的两个相邻盒子的 margin 会发生重叠

设置 son1 下边距 50 px,son2上边距100 px，两个元素中间理应 150px，但是只有 100px，即发生了外边距合并情况

**解决方法**

创建一个新的 BFC嵌套子元素，使两个son不在同一个 BFC



3）制作右侧自适应的盒子问题

