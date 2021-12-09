[TOC]

参考教程：[黑马](https://www.bilibili.com/video/BV1x64y1S7S7?)

可学习内容：完整的项目流程

缺点：组件化、组件传值、vuex的内容没有涉及

## 步骤

1. 创建项目

   ```
   vue create <项目名：vue_shop>
   ```

2. 配置 git 仓库

- 创建 git

3. 配置数据库

- 安装数据库

- 创建数据库

- 导入数据 mydb.sql

4. postman测试后台接口

- 安装 postman

- 查看接口文档：接口基准地址：`http://127.0.0.1:8888/api/private/v1/`

```
//文档内容
请求路径：login
- 请求方法：post
- 请求参数

| 参数名   | 参数说明 | 备注     |
| -------- | -------- | -------- |
| username | 用户名   | 不能为空 |
| password | 密码     | 不能为空 |
```

传递的参数为 usernam、password，可以先在数据库中创建 admin 123456

启动后台服务`node ./app.js`

在 postman 中输入测试，以下为成功返回数据的页面

![截屏2021-06-12 10.45.07](../../images/截屏2021-06-12 10.45.07.png)

5. 启动项目

```shell
npm run serve 			#启动服务
# 可选
vue ui						 #启动页面项目管理
```

ps:在做界面时，先 git 到分支，再合并

## 使用工具

- element-ui ：饿了么开发的前端组件

- axios：用来提交表单

- postman：测试后台数据库接口

- mysql：数据库

## 项目打包

进入项目文件夹`npm run build`会生成 dist 文件夹

发布 1 使用静态服务器工具包

``npm install -g serve``

`serve dist`

http://localhots:5000

发布2 使用动态服务器工具包`tomcat`



## 目录结构

### 基本文件目录

使用 api

```shell
#swiper 轮播
npm install swiper --save
#fontawsome 
npm install font-awsome --save
```

Main.js

```
import 'font-awesome/css/font-awesome.min.css'
```

## 功能实现

### 路由

`router`

### 路由导航守卫

路由导航守卫控制访问权限，如果用户没有登录，而是直接通过 URL 访问特定页面，需要重新导航到登录页面

Vue.router `router.beforeEach` 导航守卫

```javascript
//挂载路由导航守卫
router.beforeEach((to, from, next) => {
      // to 将要访问的路径
      // from 从哪个路径跳转而来
      // next ()>放行， next（'/login'）强制跳转到 login
      if (to.path === '/login') return next();//to.path 和上面 router 设置的 path 一致
      //获取 token
      else {
            const tokenStr = window.sessionStorage.getItem('token');
            if (!tokenStr) return next('/login');
            else next();
      }
})
```

### 提交之前验证

1.登录成功之后的 token 保存到客户端的 sessionStorage 中

1.1 项目中除了登录之外的其他 API 接口，必须在登录之后才能访问

1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中

2.登陆成功通过编程式导航跳转到后台主页，路由地址是 /home       

element-ui `validate`

```javascript
loginVerify() {
          this.$refs.loginFormRef.validate(async valid => {
                  if (!valid) return;
                  const {data: res} = await this.$http.post('login', this.loginForm);
                  if (res.meta.status != 200) {
                        return this.$message.error('登录失败');
                  }
                  this.$message.success('登录成功');                  
                  window.sessionStorage.setItem('token', res.data.token);
                  this.$router.push('/home');
})
```

### 退出登录功能

销毁本地 token，并退出到 login 界面

```javascript
logout() {
      //清除 token 返回 login 界面
      window.sessionStorage.clear()
      //进入 login 界面/返回需要返回的页面
      this.$router.push('/login');
},
```

### 格式化冲突问题

`.prettierrc `用来设置格式化方法，自动格式化和 eslint有冲突

`.eslintric.js`设置 console 不检查和括号前不空格不报错

```javascript
module.exports = {
    root: true,
    env: {
        node: true
    },
    extends: [
        'plugin:vue/essential',
        '@vue/standard'
    ],
    parserOptions: {
        parser: 'babel-eslint'
    },
    rules: {
        'no-console': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
        'no-debugger': process.env.NODE_ENV === 'production' ? 'warn' : 'off',
        //禁止console报错检查
        'no-console':  'off',
        //禁止空格报错检查
        'generator-star-spacing': 'off'
    }
}
```



### 通过接口获取菜单数据

需要授权 API，必须在请求头中使用 Authorization 字段提供 token 令牌

<img src="../../images/截屏2021-06-12 14.14.05.png" alt="截屏2021-06-12 14.14.05" style="zoom:50%;" />

通过 axios 请求拦截器添加 token，保证拥有获取数据的权限

```javascript
//请求拦截器
axios.interceptors.request.use(config =>{
    console.log('axios>config',config);
    config.headers.Authorization = window.sessionStorage.getItem('token');
    //固定写法：最后必须 return config
    return config
})
```



![截屏2021-06-12 14.13.41](../../images/截屏2021-06-12 14.13.41.png)

## 组件详解

Home.vue

```html
<el-menu
            :default-active="activePath"      #动态绑定显示高亮的菜单
            :unique-opened="true"          	#是否选中一个展开折叠其他
            class="el-menu-vertical-demo" #菜单类型
            background-color="#285e8e"  #背景颜色
            text-color="#fff"						 #文字颜色
            active-text-color="#5AC7FA"	 #选中菜单的文字颜色
            :collapse="iscollapse"  		  #动态绑定折叠
            :collapse-transition="false"    #是否开启折叠动画
            :router="true"     					#是否开启点击菜单跳转路由
         >  					
```

## 项目优化

### cdn静态资源优化

[cdn](http://staticfile.org/)

1. Vue.config.js 设置 external项
2. main-prod.js 文件注释相应的import 样式文件

### 路由懒加载

1. 安装`@babel/plugin-syntax-dynamic-import`

2. Babel.config.js 声明：

   ```
   plugins:[
   	'@babel/plugin-syntax-dynamic-import'
   ]
   
   ```

3. router.js 修改import 方式

webpackChunkName设置分组，相同的名会被加载到一个文件里

```
const Login=()=>import(/*webpackChunkName:"login_home_welcome"*/ './components/Login')
const Home=()=>import(/*webpackChunkName:"login_home_welcome"*/ './components/Home')
const Welcome=()=>import(/*webpackChunkName:"login_home_welcome"*/ './components/Welcome')
```

## 项目上线

1.通过 node 创建 web 服务器

2.开启 gzip 设置 传输压缩

3.配置 http 服务

4.使用 pm2 管理网站引用



### 创建 web 服务器

express 快速创建 web 服务器，将 vue 打包生成 dist 文件，托管为静态资源即可，关键代码



```javascript
const express = require('express');
//创建 web 服务器
const app = express();
//托管静态资源
app.use(express.static('./dist'))
//启动 web 服务器
app.listen(80,()=>{
  console.log('web server running at http://127.0.01')
})

```

## 错误合集

1. 出现错误:应该主要是没有 Xcode

```shell
gyp: No Xcode or CLT version detected!
npm ERR! gyp ERR! configure error 
npm ERR! gyp ERR! stack Error: `gyp` failed with exit code: 1
npm ERR! gyp ERR! stack     at ChildProcess.onCpExit (/Users/ynchen/aLearning/frontEndCode/calendar_pro/node_modules/node-gyp/lib/configure.js:345:16)
npm ERR! gyp ERR! stack     at ChildProcess.emit (events.js:315:20)
npm ERR! gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12)
npm ERR! gyp ERR! System Darwin 20.3.0
npm ERR! gyp ERR! command "/usr/local/bin/node" "/Users/ynchen/aLearning/frontEndCode/calendar_pro/node_modules/.bin/node-gyp" "rebuild"
npm ERR! gyp ERR! cwd /Users/ynchen/aLearning/frontEndCode/calendar_pro/node_modules/node-sass
npm ERR! gyp ERR! node -v v14.15.5
npm ERR! gyp ERR! node-gyp -v v3.8.0
npm ERR! gyp ERR! not ok
```

解决：[解决 mac 报错gyp: No Xcode or CLT version detected!](https://blog.csdn.net/howlaa/article/details/108262269)

2. stylus 加载不出来

```shell
error in ./src/App.vue?vue&type=style&index=0&id=7ba5bd90&lang=stylus&rel=stylesheet%2Fstylus

Syntax Error: TypeError: this.getOptions is not a function

@ ./node_modules/vue-style-loader??ref--11-oneOf-1-0!./node_modules/css-loader/dist/cjs.js??ref--11-oneOf-1-1!./node_modules/vue-loader-v16/dist/stylePostLoader.js!./node_modules/postcss-loader/src??ref--11-oneOf-1-2!./node_modules/stylus-loader/dist/cjs.js??ref--11-oneOf-1-3!./node_modules/cache-loader/dist/cjs.js??ref--0-0!./node_modules/vue-loader-v16/dist??ref--0-1!./src/App.vue?vue&type=style&index=0&id=7ba5bd90&lang=stylus&rel=stylesheet%2Fstylus 4:14-450 15:3-20:5 16:22-458
@ ./src/App.vue?vue&type=style&index=0&id=7ba5bd90&lang=stylus&rel=stylesheet%2Fstylus
@ ./src/App.vue
@ ./src/main.js
@ multi (webpack)-dev-server/client?http://192.168.1.101:8080&sockPath=/sockjs-node (webpack)/hot/dev-server.js ./src/main.js

```

- 🚫[stylus 版本匹配问题？](https://blog.csdn.net/Aeroxian/article/details/111136795)

尝试 stylus 0.54.5 stylus-loader 3.0.1  以及 stylus 0.54.7 stylus-loader 3.0.2 

npm install 

- [去掉 lang=“stylus”?](https://blog.csdn.net/u012317188/article/details/70859376)

### 页面不显示

初步判定是路由设置不完整

1. 查看 main.js 有没有引入路由
2. 查看vue-router 版本是否匹配



