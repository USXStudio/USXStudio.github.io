---
title: Session
date: 2018-02-10 16:02:49
tags: 前端知识点
---

### Cookie存在的问题

> 用户可以篡改，假如是多个用户，我只要知道他的Cookie就能冒充他，访问他的隐私信息

### Session的出现

Session实质就是服务器的一小块内存——哈希

```
//node.js服务器
step000 session定义实际就是一个哈希
let sessions = {};

//step001 设置一个随机数的session_id
let session_id = Math.random()*100000;

//step002 将用户信息存入sessions里
sessions[session_id] = {sign_in_email:emaiil}

//ste003 在你登录的一瞬间 设置Cookie的值为session_id
response.setHeader('Set-Cookie', `session_id=${session_id}`)
```

```
//你登录成功后，访问自己的资源时候会传递cookie
//假如你的 session如下 cookie:session_id=54342.312312312

//*我这里就不进行是否传递cookie的校验了，你自己干掉cookie那就重新登录呗
//*我这里就不进行是否传递cookie的校验了，你自己干掉cookie那就重新登录呗
//*我这里就不进行是否传递cookie的校验了，你自己干掉cookie那就重新登录呗

let hash = {};
//可能有多个cookie
let cookies = request.headers.cookie.split(';');
for(let i=0;i<cookies.length;i++){
    //每个cookie形如 session_id=54342.312312312
    let current = cookies[i].split('=');
    let key = current[0]; // session_id
    let value = current[1]; //54342.312312312
    hash[key] = value
}

// 根据你登录时存的session_id 查询出 email
let email = sessions[hash['session']]['sign_in_email'];

```

#### 重回公园实例

> 公园系统升级了，因为他发现有人拿着别人的票混进去

- 于是你每次购票的时候发给你一个票，票上有一个「id号」 
```
//比如 购票者   
 12386 : 猴哥
 12903 : 八戒
 12982 : 沙师弟

 其实就是 服务器里的一小块内存
 sessions = {
    "12386" : User("猴哥"),
    "12903" : User("八戒"),
    "12982" : User("沙师弟")
 }

sessions的 key就是一个「随机数」绝不是流水号那样别人就能轻易冒充你
```
- 第二天猴哥去公园了，公园就会说 「欢迎你！猴哥」

#### 什么是session

1. 服务器通过cookie给用户一个session_id
2. session_id对应服务器里的一小块内存
3. 每次用户访问服务器的时候，服务器通过session_id读取用户信息，识别用户是谁

#### 为什么session_id是一个随机数

假如你的session_id = 321341.31239089089054

> ##### 请问用户2的session_id是什么?我怎么知道用户2的随机数是啥

> ##### 那我要是删了cookie呢？

- 你自己把票扔了，公园让你进去？
- 你说可以翻墙进公园啊，那我还说啥啊，你咋不开坦克碾过去呢

当然是重新买票了，或者重新登录喽

> ##### 为啥是随机数

因为如果是流水号你按顺序排下去就知道别人的session_id了

> ##### 暴力穷举呢？

有点难啊！

```
假如session_id是16位随机数字

那你可能要试10的16次方那就是 10,000,000,000,000,000

假如是session_id是16位随机的数字和字母

那你可能要试36的16次方那就是 。。。。

一天时间是86400秒 你够快1秒来一次 你去算吧！
```

> ##### 一个用户大约占据多少内存

如果1M一个用户 1000个用户就是1G内存了
但是实际上每个用户给个 「几K」就够了

### 重要概念

#### 什么是 Cookie

1. 服务器通过Set-Cookie 头给客户端的一串字符串
2. 客户端每次访问相同域名的网页时，必须带上这段字符串
3. 客户端要在一段时间内保存这个Cookie
4. Cookie 在用户关闭用户的时候就失效(后台代码可以任意设置过期时间)

#### 什么是 Session

1. 将 SessionID 通过Cookie发送给客户端
2. 客户端访问服务器时,服务器读取 SessionID
3. 服务器有一块内存(哈希表)保存了所有 session
4. 通过 SessionID 我们可以得到对应的用户隐私信息，如id/email
5. 这块内存(哈希表)就是服务器上的所有 session

#### 什么是 LocalStorage

> html5 提供的api

```
//注意只能存 string 即使你存一个对象它也会调用对象的 obj.toString()
window.localStorage.setItem('a','1')
windwo.localStorage.getItem('a')

//如果你想存复杂数据类型只能通过序列化
let a = {name:"aaa",age:18}
window.localStorage.setItem('a',JSON.stringify(a))
```

> session最大问题就是耗内存，因为存在服务器

> localStorage存在客户端的盘符里——数据的持久化存储

答案如下

1. LocalStorage 跟 HTTP 无关
2. HTTP 不会带上 LocalStorage 的值
3. 只有相同域名的页面才能相互读取 LocalStorage(没有同源那么严格)
4. 每个域名 LocalStorage 最大存储量不同为 5M左右(每个浏览器不一样)
5. 常用场景：提示信息（没有用的信息,不能记录密码）
6. LocalStorage 永久有效，除非用户自己清理缓存

#### 什么是 SessionStorage

1,2,3,4 同 LocalStorage

5. SessionStorage 在用户关闭用户的时候就失效 

#### 请问session和cookie什么关系

> session是基于cookie实现的因为session_id必须放到cookie里发给客户端,没有cookie就没有session,
cookie是session的基石

#### 请问 Cookie 和 LocalStorage 的区别是什么

> Cookie 是每次请求都带给服务器
> Cookie 只有4K

> LocalStorage 跟HTTP无关不会被携带到服务器
> LocalStorage 有5M

#### LocalStorage 和 SessionStorage 的区别

> SessionStorage 在用户关闭用户的时候就失效 (会话结束)

### 不基于 Cookie 的 Session

实现如下

```
后端
1.登录的时候不传递cookie
2.通过json传递给前端
response.write(`{"session_id":${session_id}}`)

前端
1.登录的时候通过响应的第四部分也就是json解析传递的session_id
2.存入到localStorage

localStorage.setItem('session_id',obj.session_id) 

当访问用户信息的时候通过url参数传递session_id

window.location.href = `/?session_id=${obj.session_id}`
```

> #### session大部分时间是基于cookie来存储id的，也可以通过localStorage + 查询参数传递

### Cookie 的问题(localStorage出来之前)

> 前端那时无法持久化数据所以跨页面数据有时就使用了 Cookie

> ##### 问题来了 Cookie每次请求是会被带到服务器的

假设你有1K的cookie这样每个请求就要多携带1K的cookie到服务器，而服务器的数据可能只有 200B (请求就慢了)

##### 前端永远不要读/写 Cookie,这些都是后端的事

就像公园卖票一样，前端就相当于游客，你自己造票？