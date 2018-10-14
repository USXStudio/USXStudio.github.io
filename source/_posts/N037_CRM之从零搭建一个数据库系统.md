---
title: CRM之从零搭建一个数据库系统
date: 2017-12-09 15:43:46
tags: 前端知识点
---

### 引入LeanCloud

在 [https://leancloud.cn/](https://leancloud.cn/) 上注册一个账户。

> 前端很少涉及数据库 把数据永久的保存在一个地方 跨设备访问数据

> 阿里云最低配的30元/月

> ### leancloud这是一个将服务器能力免费给你用的网站

#### 注册的时候有一点要注意 至少8位密码 包含数字大写小写字母

#### 一个自带数据库和增删改查（CRUD）功能的后台系统。

拥有：

1. 登录注册、手机验证码功能（收费）
2. 存储任意信息
3. 读取任意信息
4. 搜索任意信息
5. 删除任意信息
6. 更新任意信息


等功能。

#### 没有见过的东西？！

> 不要慌，使用 Copy-Run-Modify 套路即可。

> 演示如何使用 LeanCloud 存一个 Hello World

1. 创建一个应用 cv-20180128
2. 引入 av-min.js，得到 window.AV
3. 初始化 AV 对象（代码直接拷）
4. 新建一条数据（代码直接拷）


- 创建你的应用之后
- 点击右上角帮助/快速入门  
- 选择我们的语言 js
- 然后开始看文档 copy


#### 先引入cdn 验证是否成功

```
<script src="//cdn1.lncld.net/static/js/3.5.0/av-min.js"></script>
//F12 
console.log(AV) //不报错代表成功
```

### 注意每个应用有自己的key 不要使用我的key

> 加入留言模块

#### message.js
```
var APP_ID = 'fqJwht8RtYXK5nVQ1uaDjUc5-gzGzoHsz';
var APP_KEY = 'rLVeL79CM0mAVV6GNa27xDpb';

AV.init({
  appId: APP_ID,
  appKey: APP_KEY
});

console.log('ok') //成功打印代表成功了
```

#### 验证 你的应用跟我不一样

```
ping "fqjwht8r.api.lncld.net"
```

#### ping成功后在继续测试

> 然后在项目中编写如下测试代码：

#### message.js

```
//创建了  TestObject表
var TestObject = AV.Object.extend('TestObject');
// 在表中创建一行数据
var testObject = new TestObject();
// 数据的内容是 words:'Hello World' 保存
// 如果保存成功，则允许alert
testObject.save({
  words: 'Hello World!'
}).then(function(object) {
  alert('LeanCloud Rocks!');
})
```

> [控制台 > 存储 > 数据 > TestObject](https://leancloud.cn/dashboard/data.html?appid=fqJwht8RtYXK5nVQ1uaDjUc5-gzGzoHsz#/TestObject)你会看到变化

#### CRM之 M

```
var hjx = AV.Object.extend('Hjx');
var obj = new hjx();
obj.save({
  words: 'hi'
}).then(function(object) {
  alert('LeanCloud Rocks!');
})
```

> 刷新你会发现多了个Hjx的表

#### 再次修改

```
var hjx = AV.Object.extend('Hjx');
var obj = new hjx();
obj.save({
  words: 'hi',
  xxxx:'sdfjklsdjflsdjkfadklas',
}).then(function(object) {
  alert('LeanCloud Rocks!');
})
```

> 刷新之后你会发现 Hjx里 多了一列 xxxx 这就是数据库

#### 套路

1. 创建一个应用 cv-20180128
2. 引入av-main.js得到window.AV
3. 初始化 AV 对象 (代码直接拷)
4. 新建一条数据(代码直接拷)


#### 添加section留言模块

```
<section>
    <h2>留言</h2>
    <form id="postMessageForm" style="width:700px;margin:50px auto;">
        <input type="text" name="content">
        <input type="submit" value="提交">
    </form>
</section>
```

```
var APP_ID = 'fqJwht8RtYXK5nVQ1uaDjUc5-gzGzoHsz';
var APP_KEY = 'rLVeL79CM0mAVV6GNa27xDpb';

AV.init({
  appId: APP_ID,
  appKey: APP_KEY
});

let myForm = document.querySelector('#postMessageForm')

myForm.addEventListener('submit',function(e){
    e.preventDefault(); //防止刷新页面
    let content = myForm.querySelector('input[name=content]').value;
    var Message = AV.Object.extend('Message');
    var message = new Message();
    message.save({
        'content':content,
    }).then(function(object) {
        alert('存入成功')
        console.log(object)
    })
})

// var hjx = AV.Object.extend('Hjx');
// var obj = new hjx();
// obj.save({
//   words: 'hi',
//   xxxx:'sdfjklsdjflsdjkfadklas',
// }).then(function(object) {
//   console.log(object)
// })
```

##### 技术细节 监听submit同时能监听用户点回车 

> 如果你不这么写就要这样写

```
let btn = myForm.querySelector('input[type=submit]')
btn.addEventListener('click',function(){});
let input = myForm.querySelector('input[name=content]')
input.addEventListener('keypress',function(e){
    if(e.keyCode === 13){
        
    }
});
```


[MVC版简历message模块为mvc分离的](https://sltrust.github.io/cv/028mycv.html)