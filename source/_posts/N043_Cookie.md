---
title: Cookie
date: 2018-02-09 16:02:49
tags: 前端知识点
---

### 什么是Cookie

[Cookie维基百科](https://zh.wikipedia.org/wiki/Cookie)

[方大文章](https://zhuanlan.zhihu.com/p/22396872?refer=study-fe)

### 出现场景

登陆注册

1. 在你登录成功的一瞬间，后台设置cookie

```
// node.js后端设置响应头
// 假如你的登录邮箱是 
let email = 'trustfor@sian.cn';
//登录成功设置响应头
response.setHeader('Set-Cookie':'sign_in_email=${email}')
```

你可以在浏览器里看到你设置的响应头 (Response Headers)

```
Set-Cookie:sign_in_email=trustfor@sian.cn
```


2. 只有被设置了Cookie,你每次访问同一域名下资源都会在请求头里传递 Cookie
```
Cookie:sign_in_email=trustfor@sian.cn
```

3. 后台得到Cookie就知道用户是谁了
> #### 将Cookie映射到生活中

>  - 你去公园，公园给你发了个票，票的有效期是2天 
>  - 假设你中途离开了公园，下午再次进入你就需要提供你的门票
> - 即使你是去看公园里的狗，也得有门票才行

### Cookie的特点

1. 服务器通过Set-Cookie 响应头设置 Cookie
2. 浏览器得到Cookie之后，每次请求都要带上Cookie
3. 服务器读取Cookie 就知道登录用户的信息(email)

> ### 问题1

我在Chrome登录了并得到了Cookie,用Safari访问Safari会带上Cookie吗？

> ##### No，北京动物园和天津动物园的门票你觉得能一样吗？

>  ### 问题2

> ##### Cookie 存在哪，你的硬盘上(如果是票当然是你的口袋里)


>  ### 问题3

票能作假吗？

> ##### 可以，只要你稍微有一点前端知识就可以伪造Cookie
> ##### 如果是Chrome你可以F12-->Application-->Cookies就能修改了

>  ### 问题4

Cookie 有有效期吗？

> ##### 当然有了，你见过7天免登陆吗？
> ##### 默认有效期20分钟，防止别人拿着你的票查看你隐私信息，

>  ### 问题5

Cookie 遵守同源策略吗？

> ##### 也有，不过跟 AJAX 的同源策略稍微有些不同。
> ##### 当请求 qq.com 下的资源时，浏览器会默认带上 qq.com 对应的 Cookie，不会带上 baidu.com 对应的 Cookie
> ##### 当请求 v.qq.com 下的资源时，浏览器不仅会带上 v.qq.com 的Cookie，还会带上 qq.com 的 Cookie
> ##### 另外 Cookie 还可以根据路径做限制，请自行了解，这个功能用得比较少。

#### 注意

> 这里的场景是非常非常简单的，后台设置cookie肯定不会拿你的邮箱，这样太好冒充了