### 简介

AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

Ajax = 异步 JavaScript + XML

Ajax 是一种用于创建快速动态网页的技术

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

基础：XMLHttpRequest (XHR) 对象

> 在出现 Ajax 之前才分出前后端，之前的网页编写使用的是混编模式

### 同步

Ajax 同步请求操作

1. JS 发送一个请求去请求数据
2. JS 等待后台返回数据
3. JS 处理完返回数据后再执行后面的操作

### 异步

Ajax 异步请求操作

1. JS 发送一个请求去请求数据
2. JS 不等待后台返回数据
3. JS 执行后面的操作

### Ajax 流程

1. 创建 ajax 对象
2. 设置请求，发送请求地址，发送请求的方式
3. 发送的数据(option)
4. 监听后台是否返回数据
5. 处理数据

```javascript
//1、创建对象
var xhrRe = new XMLHttpRequest();
//2、设置请求：方法，路径
xhrRe.open("GET", "http://127.0.0.1:5500/js_learn/Ajax/test.txt");
//3、发送数据
xhrRe.send("username=admin&password=123456");
//4、监听
xhrRe.onreadystatechange = function () {
     xhrRe.onreadystatechange = function () {
                if (xhrRe.readyState == 4 && xhrRe.status == 200) {
                    document.getElementById("myDiv").innerHTML = xhrRe.responseText;
                }
            }
}
```

```javascript
readyState ==4 和 status==200 说明请求成功
responseText
```

`window.XMLHttpRequest`

封装

```javascript
function getAjax(url,callbackFn) {
      var xhr = new XMLHttpRequest();
      xhr.open("GET",url);
      xhr.send();
      xhr.onreadystatechange = function(){
          if(xhr.status==200&&xhr.readyState==4){
            	callbackFn(xhr);
          }	
      }
 }
function traverseData(data) {
      var str = "?"
      for (var key in data) {
            console.log(key);
            str = str + key + "=" + data[key] + "&";
      }
      return str.substr(0,str.length-1);
}

var httpUrl = "https://api.apiopen.top/getJoke"
var data = {
        page: 1,
        count: 10,
        type: "video"       
}
var dataConcat = traverseData(data);
var url = httpUrl + dataConcat;
console.log(url);
getAjax(url,function(xhr){
      console.log('xhr',xhr);
      var dataObj = JSON.parse(xhr.response);
      console.log("dataObj",dataObj);
})
```













