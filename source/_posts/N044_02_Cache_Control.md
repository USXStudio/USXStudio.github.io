---
title: Cache_Control
date: 2018-02-10 19:02:49
tags: 前端知识点
---

### Cache Control

稍微有点相关web性能优化

```
node.js后台
//缓存30秒
response.setHeader('Cache-Control','max-age=30')

//这样你访问对应资源的时候就会多了一个响应头
Cache-Control：max-age=30
//意思是30秒内不要再次请求

30秒内下载资源的时间为 0ms 是从内存里或者磁盘上读取

30秒后再次请求就会重新下载资源
```

### 更新缓存

如main.js你缓存了10年但是，你更新后修改了main.js的内容

> 缓存的策略就是url一致，如果你想更新缓存那就更改url

```
<script src="main.js?v=1"></script>

//更新版本
<script src="main.js?v=2"></script>
```

```
// 文件上加一个随机字符串，这个如果你是webpack会帮你自动生成
<script src="main.fdaslj312ljkklj321.js"></script>
```


这样就实现了更新缓存

### 什么是Expire 

- 它与 Cache Control 效果一样
- 出现在 Cache Control之前
- 现在被Cache Control替代了

区别就是：

- Cache Control 是多长时间过期
- Expire 是 某年某月某日几时几分几秒过期
- Expire 的时间指的是本地时间

> 如果你系统时间是2050年，而缓存时间是2020年,你就没法缓存了


### 什么是 ETag

> 这样不得不提 MD5(摘要算法)

假如你下载一个linux系统400MB 下载过程中如何检测你下载的对不对呢？

- MD5就是为了这个场景出现的

```
如果网上的 linux系统 400MB  MD5值是 XXXXXXXX
自己下载的 linux系统 400MB  你的MD5值和网上的一致才说明下载的是正确的
```

每个文件对应一个 MD5值
文件改动越小MD5差异越大


1. 应用ETag

```
引入MD5模块算出 文件对应的MD5值

let fileMd5 = md5(文件资源字符串)
response.setHeader('ETag',fileMd5)
```

2. 这样你访问资源的时候响应头里就有了一个ETag头

```
形如
ETag:440c53453212aac08c2321sd
```

3. 以后你在访问同一资源的时候，请求头里会有一个「if-None-Match」头

「if-None-Match」就是如果不匹配，就是如果你的MD5跟我一样就不需要下载

```
if-None-Match:440c53453212aac08c2321sd
```

### ETag 和 Cache Control的区别

- 用 Cache Control是直接不请求，从盘符里读取资源
- 用 ETag 是直接不下载，但是仍然会发请求

#### 结论就是使用  Cache Control

