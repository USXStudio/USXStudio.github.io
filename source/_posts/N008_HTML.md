---
title: HTML
date: 2017-10-17 21:02:23
tags: 前端知识点
---



### HTML Hypertext Markup Language 
[维基百科HTML](https://en.wikipedia.org/wiki/HTML)

#### HTML 的版本（W3C 组织制定规范）

- HTML 4.01
- XHTML
- HTML 5 √(我们从这里开始学)
- HTML 5.1

#### 规范文档（Specifications）

> 你可以google 搜  html spec [这个网址](https://www.w3.org/TR/html5/)

1. 由 W3C 写文档（李爵士）
2. W3C 根据浏览器的实际情况总结文档，并不是凭空想象
#### DOCTYPE 文档头

- 用来选择文档类型
- 除了 HTML 5 的 DOCTYPE，其他都很难记：https://www.w3.org/QA/2002/04/valid-dtd-list.html
- 如果你没写 DOCTYPE，那你就惨了

#### html / head / body

head可省略吗？ body可省略吗？ 可以

#### 省略标签

#### 常见标签：a、form、input、button、h1、p、ul、ol、small、strong、div、span、kbd、video、audio、svg

- 基本上，你知道标签对应单词的意思，就知道这个标签怎么用了（语义化）
- 除了 div 和 span，其他标签都有默认样式
- MDN 上有所有标签的文档

#### 如何查看 MDN 文档

Google：关键词 + MDN

#### HTML 中 b 标签和 strong 标签有什么区别

b 表示样式上的加粗，strong 是逻辑状态表示内容重要。strong表示语气很强烈含有感情成分

#### p 标签的英文全称是什么？

paragraph

#### a 标签的英文全称是什么

anchor

#### 什么是空元素

```
input/img/meta/link/hr/br/col....
```
[空元素](https://developer.mozilla.org/zh-CN/docs/Glossary/%E7%A9%BA%E5%85%83%E7%B4%A0)

#### HTML5推荐写法

```
<-- 更加严格的写法要自闭合 -->
<input type="text"/>

<-- H5推荐的用法 -->
<input type="text">
```

#### div span 可以写在head里吗？

答案是不可以，但是你写了HTML会自动纠错将你写的div放到body里

#### 可替换元素

[可替换元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)

#### w3cschool html测试题

[测试题](https://www.w3schools.com/quiztest/quiztest.asp?qtest=HTML)

#### HTML究竟要不要管元素是不是块级元素还是内联元素

- HTML是不管块级元素和内联元素的区别的
- HTML只定义这是什么，不管样式
- 样式由CSS控制     

所以写HTML时候一定不要想样式问题 

#### 写页面千万别自己设计

> 你可以去 dribbble.com 你可以看到外国优秀设计师的作品

#### 初学HTML时写页面你可能会这样想

![想着上面两个一左一右下面是一个盒子](https://raw.githubusercontent.com/slTrust/note/master/img/note008_01.png)

> 实际上这样的布局是很难实现的因为目前
CSS只支持横向布局和纵向布局

![实际应该这样](https://raw.githubusercontent.com/slTrust/note/master/img/note008_02.png)

> 实际上应该上面两个左右的盒子外面包一个div当做一个整体