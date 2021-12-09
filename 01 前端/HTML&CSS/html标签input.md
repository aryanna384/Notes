## 表单控件 input

属性：

type: 定义输入标签的类型:text/password/submit

name: 提交数据的名称，提交之后可以fn+12,html>header>formdata中查看 `name：value` 这种键值对的形式

value: 提交数据的值

paceholder:预置文字

### type 属性

| 值       | 描述                                       |
| :------- | :----------------------------------------- |
| radio    | 定义单选按钮                               |
| checkbox | 定义复选框                                 |
| hidden   | 定义隐藏的输入字段                         |
| image    | 定义图像形式的提交按钮                     |
| password | 定义密码字段。该字段中的字符被掩码         |
| button   | 普通按钮                                   |
| reset    | 重置按钮。默认操作清除表单中的所有数据。   |
| submit   | 提交按钮。默认操作把表单数据发送到服务器。 |
| text     | 可输入单行文本                             |
| range    | 滑块                                       |

### text

`placeholder`提示输入，默认格式为灰色，光标移到输入框就消失

`value`用户输入

```html
<input class="userInput" placeholder="今日计划">
<script>
var inputDoc = document.querySelector(".userInput");
var value = inputDoc.value;
 </script>
```

### button

### Html5新增input控件

| Type       | 控件           |               |
| ---------- | -------------- | ------------- |
| color      | 颜色选择器     |               |
| date、time |                |               |
| email      |                | 自动校验      |
| file       | 文件选择       |               |
| number     | 数字           | 自动校验      |
| range      | 拖拽条（横向） |               |
| search     | 搜索框         | 多了clearable |
| url        |                | 自动校验      |

### file

`accept`:指定接收文件的类型

```html
<input class= "file">
```

