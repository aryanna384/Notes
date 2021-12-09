[TOC]

## 简介

理解

- Node.js 是一个能在服务器端运行 JS 的开放源代码、跨平台 JavaScript 运行环境

- 和系统进行交互，将JS 运行从浏览器到后台，可以通过 JS 开发系统（可做的工作会更多）

- 采用 Google 开发的`V8引擎`运行 js 代码，使用`事件驱动`，`非阻塞`和`异步 I/O `模型

- <u>node是`单线程`(特点)</u>，但是处理速度很快，提高 I/O 和服务器的交互

  (多线程系统对硬件要求高，但是单线程也决定了无法处理太多任务)

最初创建作用：用 JS 编写高性能的 Web 服务器

发展：形成了一个庞大的生态，不止用于编写 web 服务器，无限扩充功能

Node 包管理器：npm

单线程解决：可以使用分布式

## 模块化

遵循[commonJS 规范](http://javascript.ruanyifeng.com/nodejs/module.html)

- 一个 js 文件就是一个模块，使用` require()`引入外部模块

- 模块内的变量默认对外部不可见，希望外部文件访问到的对象需要使用 `module.exports`或者 `exports`

  - exports 和 module.exports

    - exports 只能使用`.`方式向外暴露内部变量：exports.xxx = yyy

    - module.exports 既可以使用`.`方式也可以直接赋值:exports.xxx = yyy、module.exports = {}

- 在全局中创建的变量/方法会作为 global 的属性/方法保存

- 模块中的代码都是包装在一个函数中执行的，使用`console.log(arguments.callee+"");`可以看到结构：

```javascript
function (exports, require, module, __filename, __dirname) {
}
```

### CommonJS模块特点

- 所有代码都运行在模块作用域，不会污染全局作用域。
- 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
- 模块加载的顺序，按照其在代码中出现的顺序。

## 包 package

包实际上就是压缩文件，解压以后还原为目录，符合规范的目录应该包含以下文件：

- **==package.json 描述文件==** 
- bin 可执行二进制文件
- lib js 代码
- doc 文档
- test 单元测试

CommonJS包规范允许将一组相关的模块组合到一起形成一组完整的工具

CommonJS 包规范构成：

- 包结构：用于组织包中的各种文件

- 包描述文件：package.json 描述包的相关信息，以供外部读取分析。用于表达非代码相关的信息，是一个 JSON 格式的文件，位于包的根目录下，是包的重要组成部分

  [package.json 中的字段](https://www.cnblogs.com/liaojie970/p/7155903.html)name、description、version、keywords.....

## AMD  & CommonJS

- CommonJS
  - CommonJS 是以在浏览器环境之外构建 javaScript 生态系统为目标而产生的写一套规范，主要是为了解决 javaScript 的作用域问题而定义的模块形式，可以使每个模块它自身的命名空间中执行，该规范的主要内容是，模块必须通过 module.exports 导出对外的变量或者接口，通过 require() 来导入其他模块的输出到当前模块的作用域中；目前在服务器和桌面环境中，node.js 遵循的是 CommonJS 的规范；
  - CommonJS 对模块的加载时同步的；

- AMD
  - AMD 主要是为前端 js 的表现指定的一套规范；而 CommonJS 是主要为了 js 在后端的表现制定的，它不适合前端；
  - AMD (Asynchronous Module Definition)，异步模块定义；采用的是**异步**的方式进行模块的加载，在加载模块的时候不影响后边语句的运行；
  - AMD 也是采用 require() 语句加载模块的，但是不同于 CommonJS ，它有两个参数；`require(['模块的名字']，callBack)；`requireJs 遵循的就是 AMD 规范；

## Buffer 缓冲区

从结构上看 buffer 非常像数组，二进制存储 16 进制显示，存在为了弥补数组的不足，存储图片、mp3、视频等二进制文件

使用 buffer 不需要引入模块

buffer 中的一个元素占用内存的一个字节，一个汉字三个字节

`buffer.length` 返回的是占用字节

## fr 文件系统

通过 node 操作系统中的文件

-Sync同步，同步文件系统会阻塞程序的执行

异步文件系统不会阻塞

| Flag | 描述                                         |
| :--- | :------------------------------------------- |
| r    | 以读取模式打开文件。如果文件不存在抛出异常。 |
| w    | 以写入模式打开文件，如果文件不存在则创建。   |
| a    | 以追加模式打开文件，如果文件不存在则创建。   |

//引入

`var fs =  require("fs")`

写入文件（必须一次性写入，不适合大文件）

- `fs.write(fd, buffer[, offset[, length[, position]]], callback)`异步

- `fs.writeSync(fd, buffer[, offset[, length[, position]]])`同步

- `fs.writeFile(file, data[, options], callback)`异步
  - options：使用花括号、键值对{ flag:'w' }
    - encoding  |  默认值: 'utf8'
    - mode  默认值: 0o666
    - flag  请参阅对文件系统 flags 的支持。 默认值: 'w'.
    - signal  允许中止正在进行的写入文件
- callback
  - err  错误

`fs.writeFileSync(file, data[, options])`同步

```javascript
fs.open("write.txt",'a',function(err,fd){
    //判断是否出错
    if(!err){
        fs.write(fd,"这是异步写入的内容",function(err){
            if(!err){
                console.log("\n写入成功");
            }
        })
        fs.close(fd,function(err){
            if(!err){
                console.log("文件已关闭")
            }
        });
    }else{
        console.log(err);
    }
})
```

流式文件写入

`fs.createWriteStream(path[, options])`

读取数据

`fs.readFile(path[,options],callback(erro,data))`

## npm

NPM(Node Package Manager)是 nodeJS 的实践，npm 完成了第三方模块的发布、安装和依赖。借助 NPM，node 与第三方模块之间形成了很好的生态系统

### npm 命令

```shell
npm install <module name>  -g   --save#全局安装
npm install <module name>  -D  --save#本地安装
npm install 			#下载当前项目所依赖的包 
npm uninstall  <module name>
npm remove/r   <module name> #删除
npm init          		# 创建模块 创建模块会生成一个 package.json
npm search math   #搜索模块
```

**局部安装**

- 将安装包放在` ./node_modules `下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
- 可以通过` require() `来引入本地安装的包。

**全局安装**

- 将安装包放在/usr/local/bin/node 或者 <u>node 的安装目录</u>(`which node`查看)。
-  可以直接在命令行里使用。

i 是 install 的简写 安装时使用 install 和 i 的区别（windows)：

1. 用npm i安装的模块无法用npm uninstall删除，用npm uninstall i才卸载掉
2. npm i会帮助检测与当前node版本最匹配的npm包版本号，并匹配出来相互依赖的npm包应该提升的版本号
3. 部分npm包在当前node版本下无法使用，必须使用建议版本
4. 安装报错时install肯定会出现npm-debug.log 文件，npm i不一定



`--save-dev`和`--save`下载选择 安装并添加到依赖

- package.json 文件依赖包添加不同
  - –save 会把依赖包名称添加到 dependencies 键下，dependencies是运行时依赖
  - –save-dev 则添加到 devDependencies 键下，devDependencies是开发时的依赖，这里添加的在开发完之后运行不了

添加到依赖的作用？ ----- 相当于 python 的环境配置，缺包就 pip install 







