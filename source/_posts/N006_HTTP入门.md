---
title: HTTP入门
date: 2017-10-12 12:51:02
tags: 前端知识点
---

### www（World Wide Web）
> 主要有三个概念
- URI 俗称网址
- HTTP 两台电脑之间的传输协议
- HTML 超文本标记语言，用于页面跳转
> URL的作用是让你访问一个网页，HTTP的作用是让你能下载这个页面，HTML是让你能看懂这个网页。
### URI 是什么（统一资源标识符）
URI分为两种URL（统一资源定位符）和URN（统一资源名称）
#### URN
> ISBN:9934123123123 书号，这就是一个URN 可以确定一个"唯一的"资源
#### URL
> https://www.baidu.com/s?wd=hello&rev_spt=1#5

- https代表--协议
- www.baidu.com 代表--域名
- /s代表--路径
- wd=hello&rev_spt=1代表--查询参数
- “#5” 代表--锚点 

> .com代表一级(顶级)域名 baidu代表二级域名 www是三级域名

> 但是一般的工程师都会把.com忽略掉,把baidu叫做一级域名 此时要脑补他说的是错的

### URL的常见组成

#### DNS 域名解析器

> 命令行里输入 nslookup www.baidu.com就可以看见这个网址对应服务器相关信息

```

Server: 192.168.5.1   
这个是我的路由(他去问的电信，电信帮你解析网址对应的ip电信为什么知道你花钱啦)
Address:  192.168.5.1#53

Name:  www.baidu.com 
Address:  119.75.213.61 这个服务器告诉你百度的ip 每个人返回的IP不一样

```
> dns会根据一个域名返回一个ip 
那我可以不可以指定一个ip（可以）这就是如何绕过dns自己指定一个ip
非常时期可以修改host文件(window系统)自己指定google的ip
因为电信返回的ip是错的。


![示意图](https://sltrust.github.io/note/img/note006_1.png)

#### http

![示意图](https://sltrust.github.io/note/img/note006_2.png)

#### curl 命令

```
 curl -s -v -H "hjx:xxx" -- "https://www.baidu.com"
```

- -s代表不显示进度
- -v代表显示请求和响应
- -H "hjx:xxx" 添加一个请求头
- -- 'https://www.baidu.com ' 代表要请求的网址

```
* Rebuilt URL to: https://www.baidu.com/
*   Trying 119.75.216.20...
* TCP_NODELAY set
...
*  SSL certificate verify ok.

> GET / HTTP/1.1    "/"代表根目录 HTTP/1.1代表协议和版本号
> Host: www.baidu.com   就是访问的域名
> User-Agent: curl/7.55.1  我用的什么软件发起的响应
> Accept: */*   我接受你返回给我的任何内容
> hjx:xxx   可以删掉没有任何意义  删除 - H参数就可以
>    

< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
< Connection: Keep-Alive
< Content-Length: 2443
< Content-Type: text/html
< Date: Tue, 02 Jan 2018 15:44:59 GMT
< Etag: "588603ec-98b"
< Last-Modified: Mon, 23 Jan 2017 13:23:56 GMT
< Pragma: no-cache
< Server: bfe/1.0.8.18
< Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
<
* Connection #0 to host www.baidu.com left intact
```

-  *开头的代表注释
- “>” 代表请求内容
- ”<“ 代表响应内容

> 重新修改请求
```
curl -X POST -s -v -H "hjx:xxx" -- "https://www.baidu.com" 
```
请求的内容为
```
POST / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */*
hjx: xxx
```
继续修改
```
curl -X POST -d "1234567890" -s -v -H "hjx: xxx" -- "https://www.baidu.com"
```
```
POST / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */*
hjx: xxx
Content-Length: 10  代表上传数据的字节
Content-Type: application/x-www-form-urlencoded 上传的格式

1234567890
```
#### get和post的区别
- get就是获取内容 
- post就是上传内容
> 疑问 
```
curl -s -v -H "host:www.qq.com" -- "https://www.baidu.com" 修改访问的域名
相当于你去麦当劳问有没有老北京鸡肉卷

```
### 请求的格式

>
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

#### 请求包含的内容

1. 请求最多包含四部分，最少包含三部分。（也就是说第四部分可以为空）
2. 第三部分永远都是一个回车（\n）
3. 动词有 GET POST PUT PATCH DELETE HEAD OPTIONS 等
    - PUT 代表 五行话替换成一行 整体更新
    - PATCH 代表 五行话里其中一行替换 一行 局部更新
    - DELETE 删除
    
4. 这里的路径包括「查询参数」，但不包括「锚点」
5. 如果你没有写路径，那么路径默认为 /
6. 第 2 部分中的 Content-Type 标注了第 4 部分的格式
> x-www-form-urlencoded 
- x 代表没有被写入规范的格式
- www 万维网
- urlencoded 解析中文


#### 用 Chrome 查看响应

1. 打开 Network
2. 输入网址
3. 选中第一个响应
4. 查看 Response Headers，点击「view source」，点击「view source」，点击「view source」
5. 你会看到响应的前两部分
6. 查看 Response 或者 Preview，你会看到响应的第 4 部分

#### http 和https
> http是明文传输 就是你的登录密码，别人可以看到不是加密的

> https 就是 密码这些东西是经过加密的 无法直接看到内容

### 响应

请求了之后，应该都能得到一个响应，除非断网了，或者服务器宕机了。

响应示例
上面三个请求示例，前两个请求对应的响应分别为
```
HTTP/1.1 200 OK
Accept-Ranges: bytes
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Connection: Keep-Alive
Content-Length: 2443
Content-Type: text/html
Date: Tue, 10 Oct 2017 09:14:05 GMT
Etag: "5886041d-98b"
Last-Modified: Mon, 23 Jan 2017 13:24:45 GMT
Pragma: no-cache
Server: bfe/1.0.8.18
Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/

<!DOCTYPE html>
<!--STATUS OK--><html> <head> 后面太长，省略了……
HTTP/1.1 302 Found
Connection: Keep-Alive
Content-Length: 17931
Content-Type: text/html
Date: Tue, 10 Oct 2017 09:19:47 GMT
Etag: "54d9749e-460b"
Server: bfe/1.0.8.18

<html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8"> 后面太长，省略了……
```
GET 请求和 POST 请求对应的响应可以一样，也可以不一样
响应的第四部分可以很长很长很长
响应的格式
>
```
1. 协议/版本号 状态码 状态解释
2. Key1: value1
2. Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html 
3
4 要下载的内容
```
#### 状态码要背，是服务器对浏览器说的话

```
1xx 不常用
2xx 表示成功  
3xx 表示滚吧
4xx 表示你丫错了
5xx 表示好吧，我错了
200 一般的成功  
204 创建成功 一般post返回204
301 表示你访问的资源已经不在了 迁移到其他域名返还给你新的地址
302 表示你访问的资源只是临时不在了 敏感信息封禁了一段时间
304 表示你访问的内容和上一次一样,返回上一次的内容
404 未找到页面  网址错了
502 服务器挂了
```
```
状态解释没什么用
第 2 部分中的 Content-Type 标注了第 4 部分的格式
第 2 部分中的 Content-Type 遵循 MIME 规范
用 Chrome 查看响应
打开 Network
输入网址
选中第一个响应
查看 Response Headers，点击「view source」，点击「view source」，点击「view source」
你会看到响应的前两部分
查看 Response 或者 Preview，你会看到响应的第 4 部分
```
