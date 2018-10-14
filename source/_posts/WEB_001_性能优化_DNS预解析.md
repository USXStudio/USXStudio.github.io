---
title: web性能优化之DNS预解析
date: 2018-05-17 23:11:34
tags: web性能优化
---

### DNS预解析

> 去参考淘宝的移动端页面你可能会看到这样一段代码

请自行选择

[淘宝](https://h5.m.taobao.com)

```
<title>淘宝网触屏版</title>
<meta name=data-spm content=a215s>
<link rel="dns-prefetch" href="//g.alicdn.com">
<link rel="dns-prefetch" href="//gw.alicdn.com">
<link rel="dns-prefetch" href="//log.mmstat.com">
<link rel="dns-prefetch" href="//api.m.taobao.com">
<link rel="dns-prefetch" href="//cdn.tanx.com">
<link rel="dns-prefetch" href="//ecpm.tanx.com">
```

- 淘宝页面有很多的图片资源

> 预解析就是指「加载后面图片的时候，不会对域名再次进行解析了(因为已经解析过了)」

作用

- 加快后面图片的解析时间

### 待更新


