---
title: node ten Modules(初始)
date: 2018-05-04 14:42:35
tags: node每日精进
---

### 给迷茫的你学习Node.js最好的方法

[原文连接](http://cnodejs.org/topic/59c75a3dd7cbefc511964688)

### 每天10个 npm 模块

[仓库地址](https://github.com/parro-it/awesome-micro-npm-packages)

### Array

#### is-sorted

> A small module to check if an Array is sorted.

核心代码   index.js

```
function defaultComparator (a, b) {
  return a - b
}

module.exports = function checksort (array, comparator) {
  comparator = comparator || defaultComparator

  for (var i = 1, length = array.length; i < length; ++i) {
    if (comparator(array[i - 1], array[i]) > 0) return false
  }

  return true
}
```

test.js

```
var sorted = require('./index')

console.log(sorted([1, 2, 3]))
// => true

console.log(sorted([3, 1, 2]))
// => false

// supports custom comparators
console.log(sorted([3, 2, 1], function (a, b) { return b - a }))
```



