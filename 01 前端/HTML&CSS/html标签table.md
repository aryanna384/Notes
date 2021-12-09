## table

表格主要用于显示、展示数据

- 表头单元格默认居中加粗显示

- 默认没有边框

```html
<table>表格
  <tr>行
    <th>我是表头单元格</th>
    <td>单元格</td>  
  </tr>  
</table>
```

#### table属性

```html
 <table align = "center" 
        border="1" 
        cellpading="20" //默认 1px
        cellspacing="0"> //默认 2px
 </table>
```

- `cellpading`：单元格内部边框到文字的距离（就是单元格大小）
- `cellspacing`：单元格之间的距离，（相当于双线边框）
- table默认双线边框，`border:collapse`设置为单线

#### [tr属性](https://www.w3school.com.cn/tags/tag_tr.asp)

#### 其他

` <tbody>` 主体区域

`<thead>`组织属于表头的内容

`<caption>`表格标题

`<tfoot>`汇总内容

```html
<table>
<!--  表头 -->
  <thead>  
    <tr>
      <th>
      </th>
    </tr>
  </thead>
<!--  主体 -->
  <tbody>
    <tr>
      <td>
      </td>  
    </tr>  
  </tbody>
</table>
```

#### 合并单元格

跨行：`rowspan`行跨度= 合并单元格的个数，最上侧单元格为目标单元格，写合并代码

跨列：`colspan`列跨度=合并单元格的个数，最左侧单元格为目标单元格，写合并代码

![截屏2021-05-20 10.02.08](../../images/截屏2021-05-20 10.02.08.png)

合并步骤：

- 确定跨行/列

- 合并


```html
<table border="1" cellspacing="0" cellpadding="4">
  <thead>
    <tr>
      <th>1</th>
      <th colspan="2">2</th>
    </tr>
    <tr>
      <th>2-1</th>
      <th>2-2</th>
      <th>2-3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="2">单元格 1</td>
      <td>单元格 2</td>
      <td>单元格 3</td>
    </tr>
    <tr>
      <td>单元格 1</td>
      <td>单元格 2</td>
    </tr>
  </tbody>
</table>
```

<img src="../../images/截屏2021-05-20 10.20.12.png" alt="截屏2021-05-20 10.20.12" style="zoom:50%;" />



















