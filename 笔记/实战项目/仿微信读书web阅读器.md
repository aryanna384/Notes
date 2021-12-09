[TOC]

### 项目配置

- epubJS https://github.com/futurepress/epub.js

- 安装nvm: npm和node的管理包

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh|bash
```

- node 10.10.0

- vue/cli 3.0.4

### 资源引入

![截屏2021-07-07 下午4.57.56](/Users/hb/Library/Mobile Documents/com~apple~CloudDocs/笔记/哈啰实习log/image/截屏2021-07-07 下午4.57.56.png)

- font字体:iconfont+iconmoon

- web字体

引入方式：

1. dist/index.html 

```
<link rel="stylesheet" href="<%= BASE_URL %>fonts/daysOne.css" /> 
```

字体文件存储位置：public/fonts

2. src/main.js

```javascript
import './assets/fonts/daysOne.css'
```

字体文件存储位置：src/assets/fonts

使用

```html
<span class =  'text'> ABCD</span>
<style>
  .text{
    font-family:"Days One"
  }
</style>
```

### 静态资源配置nginx使用

安装

```
brew install nginx
```

启动服务

```sheel
sudo nginx
```



配置本地静态资源文件`/usr/local/etc/nginx/nginx.conf`

添加`server`字段：

```txt
server {
  listen 8081;
  server_name resource;
#resource地址
   root /Users/ynchen/damncode/vueProject/resource;
   autoindex on;
#配置跨域
  location / { 
  	add_header Access-Control-Allow-Origin *;
  } 
#不做缓存
	add_header Cache-Control "no-cache,must-recalidate";
}
```

查看语法是否正确

```
nginx -t
```

修改生效

```shell
sudo nginx -s reload
```

404没有访问权限

`/usr/local/etc/nginx/nginx.conf`

```
user [用户名] owner;
```

先进入resource目录再nginx启动，localhost：8081访问

