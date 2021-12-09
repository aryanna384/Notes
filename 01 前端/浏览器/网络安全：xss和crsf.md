- `crsf`跨站脚本攻击（Cross Site Scripting）

[可参考](https://zhuanlan.zhihu.com/p/26177815)

恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。使得用户浏览器执行本不存在的前端代码。

危害：

1. 窃取网页浏览中的cookie值。

在网页浏览中我们常常涉及到用户登录，登录完毕之后服务端会返回一个cookie值。这个cookie值相当于一个令牌，拿着这张令牌就等同于证明了你是某个用户。

如果你的cookie值被窃取，那么攻击者很可能能够直接利用你的这张令牌不用密码就登录你的账户。如果想要通过script脚本获得当前页面的cookie值，通常会用到document.cookie。

1. 劫持流量实现恶意跳转

```js
<script>window.location.href="http://www.baidu.com";</script>
```

那么所访问的网站就会被跳转到百度的首页。

- `crsf`跨站请求伪造(Cross-site request forgery)

[可参考](https://zhuanlan.zhihu.com/p/98062456)

是指黑客引诱用户打开黑客的网站，在黑客的网站中，利用用户的登录状态发起跨站请求。

![img](https://pic4.zhimg.com/v2-0bdbc7f56a9dac0b413f0e835acdbdfb_b.jpg)