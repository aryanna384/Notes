[TOC]

## 介绍

webpack 是一个模块化打包工具，根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。本身只能加载 JS 和 JSON 文件，如果要加载其他类型的文件，就需要相应的 loader进行转换/加载配置文件（默认）

- webpack.config.js：固定的配置文件名，一个 node 模块，返回一个 json 格式的配置信息对象
- 箭头指向被依赖的对象

### webpack 安装

```shell
npm install webpack -g //全局
npm install webpack@[版本] --save-dev // 局部安装
```

### 核心

- 入口(`entry`):入口起点指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。
- 输出(`output`)：告诉 webpack 在哪里输出它所创建的 bundle，以及如何命名这些文件。
- loade（`module>rules`)：webpack 只能理解 JavaScript 和 JSON 文件。loader 让 webpack 能够去处理其他类型的文件，并将它们转换为有效模块，以及添加到依赖图中。
- 插件(`plugins`):用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。
- 模式(`mode`):开发模式`development`和生产模式`production`。

  

## webpack.config.js 标准模板

```javascript
module.exports ={
   	entry:"",//入口
  	output:{//输出
      	path:,//输出地址
      	filename:,//绑定的名称
   	 },
  	module:{
      	rules:[
          	{	
              	//指定匹配的文件
              	test:,
              	//使用的资源
              	use:[]//              
            }
          ]
      },
  	plugins:[],
  	mode: 'production',
}
```

## 初始化：打包js、json

```shell
npm init  -y #创建模块 会在文件夹下创建 package.json
				#-y 表示全部默认 
npm  i webpack webpack-cli -g //全局安装
npm i webpack webpack-cli -D //局部安装 
```



## 文件目录

```shell
.
├── build #输出文件夹
│   │   ├── index.html
│   │   ├── main.js  #指定输出文件夹后自动生成的文件
├── node_modules  #局部安装时生成的模块
│   ├──....
├── package-lock.json
├── package.json #init 创建模块之后生成的文件
├── src   #源代码目录
│   ├── css
│   │   └── index.css
│   ├── data
│   │   └── data.json
│   │   └── js
│   ├── index.js #入口文件
└── webpack.config.js  #webpack 配置文件，配置好之后打包可以直接使用 webpack
```

```shell
//在还未配置webpack.config.js 时打包命令
sh-3.2\# webpack --entry ./src/index.js -o ./build --mode=development
asset main.js 1.5 KiB [emitted] (name: main)
./src/index.js 311 bytes [built] [code generated]
webpack 5.38.1 compiled successfully in 60 ms
```

不能打包 css 文件

在入口文件 index.js 中引入

```javascript
import "./index.css";//引入时打包报错
```

打包报错：

```shell
ERROR in ./src/index.css 1:0
Module parse failed: Unexpected token (1:0)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
> *{
|     padding: 0;
|     margin: 0;
 @ ./src/index.js 15:0-21
```

于是：

## 打包需要css/less文件（loader配置

步骤：下载 --> 配置

### 下载

不同的资源需要不同的 loader 

```shell
npm i css-loader style-loader --save-dev
npm i file-loader url-loader --save-dev
npm i less-loader -D
```

### 配置 loader

最基本的配置

```javascript
/* 
    webpack.config.js webpack 的配置文件
    作用：指示 webpack 干那些活（当你运行 webpack 指令时，会加载里面的配置
    所有工具都是基于 nodeJS 平台运行的，模块化默认采用 commonjs
 */
const {resolve} = require('path');
module.exports ={
    //入口起点
    entry:'./src/index.js',
    output:{
        //输出文件名 
        filename:'main.js',
        //__dirname nodejs变量，代表当前文件的目录绝对路径
        //输出到 build 文件夹下 的 mian.js 文件
        path:resolve(__dirname,'build'),
    },
    module:{
        rules:[
            //详细 loader 配置
        ]
    },
    plugins:[
        //详细 plugIns 配置
    ],
    //模式
    mode:'development',
}
```

配置完成，打包

```shell
webpack
```

处理图片资源

```javascript
module:{
  	rules:[
      {
        test:/\.(jpg|png|gif)$/,
        //下载 url-loader,file-loade
        loader:'url-loader',
        options:{
          //图片大小小于 8kb，会被 base64 处理
          //优点：减少请求数量（减轻服务器压力）
          //缺点：图片体积会更大（文件请求速度更慢）

          limit:8*1024,
        }
      }
    ]
}
```



## 打包 html 文件（plugins 配置

下载 --> 引用 --> 配置

```shell
 npm install  html-webpack-plugin -D
```

```javascript
const HTMLWebpackPlugin = require('html-webpack-plugin');//引入
module.exports={
	plugins:[
    	////功能：默认会创建一个空的 HTML ，自动引入打包输出的所有资源（js/css)
    	new HTMLWebpackPlugin();
  ]
}
```

## 打包其他资源

字体图标啥的，使用` loader:'file-loader'`

## 开发环境：devServe

在每次编译代码时手动运行 npm run build 会很麻烦，开发环境可以是*开发*体验更轻松

开发服务器devServer：用来自动化（自动编译，自动打开浏览器，自动刷新浏览器....

`webpack-dev-middleware` 是一个封装器(wrapper)，它可以把 webpack 处理过的文件发送到一个 server

## 为什么需要打包工具？

使用打包工具可以把很多资源整合到一起，在 html 引入时只需要引入一个文件就会有 js、css 等文件



















