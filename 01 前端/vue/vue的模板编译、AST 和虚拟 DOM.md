[TOC]

### AST （abstract synax tree)

抽象语法树是源代码的抽象语法结构的树状表示，树上的每个节点都表示源代码中的一种结构。抽象语法树在很多领域有广泛的应用，比如浏览器，智能编辑器，编译器。

### 为什么要转成AST

因为在

虚拟 DOM ：用普通js对象来描述DOM结构，因为不是真实DOM，所以称之为虚拟DOM。
虚拟 DOM 是相对于浏览器所渲染出来的真实 DOM 而言的，在react，vue等技术出现之前，我们要改变页面展示的内容只能通过遍历查询 DOM 树的方式找到需要修改的 DOM 然后修改样式行为或者结构，来达到更新 UI 的目的。这种方式相当`消耗计算资源`，因为每次查询 DOM 几乎都需要遍历整颗 DOM 树，如果建立一个与 DOM 树对应的虚拟 DOM 对象（ js 对象），以对象嵌套的方式来表示 DOM 树及其层级结构，那么每次 DOM 的更改就变成了对 js 对象的属性的增删改查，这样一来查找 js 对象的属性变化要比查询 dom 树的性能开销小。

真实 DOM：

真实 DOM 和其解析流程

 浏览器渲染引擎工作流程都差不多，大致分为5步

==创建DOM树——创建StyleRules——创建Render树——布局Layout——绘制Painting==

1. 用HTML分析器，分析HTML元素，构建一颗DOM树(标记化和树构建)。

2. 用CSS分析器，分析CSS文件和元素上的inline样式，生成页面的样式表。

3. 将DOM树和样式表，关联起来，构建一颗Render树(这一过程又称为Attachment)。每个DOM节点都有attach方法，接受样式信息，`返回一个render对象`(又名renderer)。这些render对象最终会被构建成一颗`Render树`。

4. 有了Render树，浏览器开始布局，为每个Render树上的节点确定一个在显示屏上出现的精确坐标。

5. Render树和节点显示坐标都有了，就调用每个节点paint方法，把它们绘制出来。 

### 模板编译的过程

Vue的模板编译是在 mount 的过程中进行的，在 mount 的时候执行了 compile 方法来将 template 里的内容转换成真正的 HTML 代码。complie 最终生成 render 函数，等待调用。
1. 获取 template : render（）> option.template  > html 中的 <template>
2. template --> AST
3. AST  --> render 函数 
4. render 函数--> 虚拟节点
5. 设置 patch --> 打补丁到真实 DOM

