## form

### input

### label+input

value为提交给服务器的值

Label表示对用户界面中某个元素的说明

将label和radio/checkbox绑定时，不需要点击单选/复选框才能勾选，点击文字也可以勾选，此时的文字使用label

写法一：

```html
<label>xx
	<input type="radio" value="xx"></input>
</label>
```

写法二：

```html
<label for="username"></label>
<input type="text" id="username"></input>
```

### select

### datalist

下拉框选择，可文字搜索筛选

