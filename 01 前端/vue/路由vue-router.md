[TOC]

### 路由

路由：通过互联的网络把信息从源地址传输到目的地址的活动

路由器：路由和传送

- 路由是决定数据包从来源到目的地的路径

- 转送将输入端的数据转移到合适的输出单

路由就是用来跟后端服务器进行交互的一种方式，通过不同的路径，来请求不同的资源，请求不同的页面

#### 后端路由

整个页面的模块由后端编写和维护，页面中需要请求不同的路径内容时，交给服务器处理，服务器渲染好整个页面并将页面返回给客户端。

#### 前端路由

随着 Ajax 的出现，有了前后端分离的开发模式，后端只提供 API 返回数据，前端通过 Ajax 获取数据

本质上就是检测 url 的变化，截获 url 地址，然后解析来匹配路由规则。

核心：改变 URL，但是页面不进行整体的刷新

#### hash 模式和 history 模式

localhost：8080/

- hash 模式

`localhost：8080#/home`

比如这个 URL：http://www.abc.com/#/hello，hash 的值为 `#/hello`。

特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面，用户体验流畅

```html
<script>
  //跳转到 #/
  //监听 hashchange 事件
 window.onhashchange = (e) => {
        console.log('old:', e.oldURL);
        console.log('new:', e.newURL);
        console.log('hash', location.hash);
  }
</script>
```

方式：

- 地址栏直接改变

- 浏览器前进/后退按钮

原理：监听hash 值的变化，重新渲染视图

- history 模式（vue create 的默认设置）

mode:'history'

`localhost：8080/home`

原理： 使用pushState() 和 replaceState() 方法

```html
<script>
    const myBtn = document.querySelector('.btn');
    window.addEventListener('DOMContentLoaded',()=>{
        console.log('path:',location.pathname);
    })
  //实现 点击按钮跳转到 /user
    myBtn.addEventListener('click',()=>{
        const state = {name:'user'}
        history.pushState(state,'','user');
        console.log('切换路由到user');
    })
</script>
```

区别：

一般场景下，hash 和 history 都可以。

路由的 history 模式，这种模式充分利用 history.pushState API 来完成URL 跳转而无须重新加载页面。

另外，调用 history.pushState() 相比于直接修改 hash，存在以下优势：

- pushState() 设置的新 URL 可以是与当前 URL 同源的任意 URL；而 hash 只可修改 # 后面的部分，因此只能设置与当前 URL 同文档的 URL；
- pushState() 设置的新 URL 可以与当前 URL 一模一样，这样也会把记录添加到栈中；而 hash 设置的新值必须与原来不一样才会触发动作将记录添加到栈中；
- pushState() 通过 stateObject 参数可以添加任意类型的数据到记录中；而 hash 只可添加短字符串；
- pushState() 可额外设置 title 属性供后续使用。

缺点

- hash 模式下，仅 hash 符号之前的内容会被包含在请求中，如 http://www.abc.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。
- history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.abc.com/book/id。如果后端缺少对 /book/id 的路由处理，将返回 404 错误。

### 路由守卫

`router.beforeEach((to,from,next)=>{})`

- `to: Route`: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- `from: Route`: 当前导航正要离开的路由
- `next: Function`: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - `next()`: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
  - `next(false)`: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - `next('/')` 或者 `next({ path: '/' })`: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
  - `next(error)`: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

### 路由懒加载

JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

### 具体使用

配置后端

koas serve

```shell
npm i koa-generator -g
koa2 [project-name]
cd [project-name]
npm install
npm i koa2-core -S
npm run dev
```

路由权限

1.登录  uid > 后端 API  > 路由权限 API

2 后端 > 用户对应路由权限列表 > 前端 > JSON 

3 JSON > 树形机构化

4 树形结构化的数据 > Vue路由结构

5 路由结构动态 > 静态路由

6 树形结构化的数据 > 菜单组件

形式：

```json
[
    {
          id:,
          pid:, //parentId
          path:,
          name:
          link:
          title
    }
]
```
