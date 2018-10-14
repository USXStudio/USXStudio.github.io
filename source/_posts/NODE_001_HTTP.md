---
title: node从零开始之HTTP
date: 2018-04-24 23:25:13
tags: node
---

### HTTP(HyperText Transfer Protocol) 超文本传输协议

[维基百科中文](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)

### HTTP格式

文本

### HTTP的组成部分

> 详情请看我写的 HTTP入门文章

1. 动词 路径 协议/版本   第一部分路径随便写 一定要以"/"开头
2.   第二部分 键值对
    - Key1: value1     
    - Key2: value2
    - Key3: value3
    - Content-Type: application/x-www-form-urlencoded
    - Host: www.baidu.com
    - User-Agent: curl/7.54.0
3.   回车     就一个目的区分 第二部分和第四部分 
4. 要上传的数据

### 推荐使用的测试工具 postman

> 方便调试http请求的工具

[download](https://www.getpostman.com/)

![postman](https://raw.githubusercontent.com/slTrust/note/master/node/node_001_01.png)

- get 获取数据
- post 新增数据
- patch 更新资源
- delete 删除数据
- head 不用实际请求资源的时候,获取文件的相关信息(大小/长度/权限)
    > 对一个资源发一个head请求就会返回它的头文件(里面包含CDN 上的资源/图片/视频的相关信息)
- options 向服务器询问我能通过那些方法去操作这个资源(区分管理员/游客信息)

> 具体风格请去看 restful 相关的约定

### 响应头

[维基百科HTTP状态码](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)

- 200 OK 响应成功啦
- 201 Created 创建资源的时候做一个返回
- 202 服务器已接受但是没有处理(最终请求可能会也可能不会被执行)
- 206 断点续传。下载时（文件比较大100MB）先返回10M 你下载好后下次再从10M的节点去要后面的10M
- 302 Found 重定向(资源被放到了其他地方)
- 304 Not modified 表示资源未修改返回上一次请求的内容(但是仍然触发了请求)
- 400 参数不对 返回请求失败的一些原因
- 401 服务器向客户端询问权限的一个响应头
- 403 服务器已经收到，但是拒绝执行。 通常告诉用户权限不够
- 404 未找到页面
- 500 
- 502 网关错误
- 504 超时

### 推荐书籍

- 图解HTTP(简单)推荐这本
- HTTP权威指南(工具书)

### node 模块化

> 最简单的模块点定义

module_a.js

```
function haha(){console.log(1)}

exports.haha = haha;
```

main.js 使用module_a.js

```
var mod = require('./module_a')
mod.haha()
```

- 系统模块用绝对路径如 requier('http')
- 自有模块要用相对路径 requier('./module_a')  

> 同一模块导出多个变量或方法

module_a.js

```
function haha(){console.log(1)}

function lala(){console.log(2)}
```

> 每个都指定一下

```
module.exports.haha = haha;
module.exports.lala = lala;
```

> 多个导出

```
module.exports = {
    haha,
    lala
}
```

### node 之字里行间创建一个服务器

node可以很简短的几句话就可以创建一个服务器

```
const http = require('http');
let server = http.createServer();

server.on('request',(request,response)=>{

})

server.listen(8200)
```