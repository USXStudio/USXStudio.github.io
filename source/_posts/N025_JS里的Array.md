---
title: JS里的Array
date: 2017-11-23 17:15:16
tags: 前端知识点
---

### 基本类型 && 复杂类型
> 复杂类型的对象使用不使用new都会返回对象
```
Objcet(1)  // 返回一个Number对象
Object('sss') //返回一个String对象
Object(true)  //返回一个 Boolean对象
Object() // 返回一个对象 {}
new Object() //返回一个对象{}
```
```
String(1) //  '1'
String(true)  // 'true'
String(null)  //  'null'

new String(1)  // String对象
new String('sss')  //String对象

Number(1)  //  数值类型  1
Number('sss')  // 数值类型  NaN

new Number(1)  // Number对象

Boolean(1)  // true
new Boolean(1)  //Boolean对象
new Boolean('   ') //Boolean对象
```
>![](https://sltrust.github.io/note/img/note025_2.png)

```
//不一致性的一个恶心的问题
var arr = new Array(3); //这个数组的length为3
//arr[0]未指定 arr[1]未指定 arr[2] 未指定
console.log(0 in arr) //false
console.log(1 in arr) //false
console.log(2 in arr) //false
```

> 
#### JS七种数据类型
> number   string   boolean   symbol   null   undefined   object
#### 五个falsy值
> 0 NaN 空字符 null undefined

#### Array 
> 用法
##### 声明
```
var a1 = ['a','b'];   //推荐使用
var a2 = new Array('a','b')
// 两种方式没区别
```
#### 不一致性 Array(3) 和 Array(3,3) (坑爹之处)
```
var a1 = Array(3) //单个参数  声明了length = 3 的数组 每个值是undefined
var a2 = Array(2,3) // 声明了 length = 2 的数组  [2,3]
```
>![](https://sltrust.github.io/note/img/note025_01.png)

#### function && Function
```
//function 是关键字  
//Function是全局对象

//声明方式  
//具名函数
function fn (){ }

// 匿名函数
function(){ }   

var f2 = new Function('a','b','return a+b');   //闲的蛋疼才用   很极限的情况下才会应用
```

```
var f = function (){return 'a';}
// var a = 1的变体  只不过  function存在堆里 f 存的是引用
```
#### 继续研究数组
>![](https://sltrust.github.io/note/img/note025_03.png)
#### Array && 伪数组
```
var arr = [1,2,3];
for(var i=0;i<arr.length;i++){
  console.log(arr[i])
}

//伪数组
var obj = {0:1,1:2,2:3,length:3}
for(var i=0;i<obj.length;i++){
  console.log(obj[i])
}
```
> 数组与伪数组的区别就是  伪数组的原型链里没有Array.prototype这一层
>![](https://sltrust.github.io/note/img/note025_04.png)

#### forEach
```
var a =[1,2,3]
a.forEach(function(val,idx,arr){ 
  //遍历方法要接受一个函数
  //函数有三个参数  val是值 idx是索引  arr就是这个数组对象
  console.log(val,idx,arr)
})
```
```
//接受一个函数并执行
function myExe( fn){
  fn(666);
}
myExe(function(xxx){
                console.log(xxx)
            })
// 666
```
> 简化版forEach
```
function forEach(arr,fn){
    for(var i=0;i<arr.length;i++){
        fn(arr[i],i);
    }
}

var arr = ['a','b','c']
forEach(arr,function(v,i){console.log(v,i)})


//数组的forEach
arr.forEach(function(v,i){
                     console.log(v,i)
            })
```
疑惑 为什么a.forEach() 没有像我们的forEach一样传递 arr呢？
```
arr.forEach(function(){}) 
//等价于下面的形式
arr.forEach.call(arr,function(){})
```
>再看
```
var obj = {0:'a',1:'b',2:'c',length:2};
obj.forEach = function(fn){
    for(var i=0;i<this.length;i++){
        fn(this[i],i);
    }
}
//forEach实际上接收了 obj 就是用this来接收
// js蛋疼之处
```

##### sort() 
它内置的排序一般是快排
```
var a = [5,3,2,1,4];
a.sort(); // 1,2,3,4,5 默认升序
//你也可以传递一个函数 函数的返回值为正数、0或负数
a.sort(function(x,y){return x-y;})  //1,2,3,4,5
a.sort(function(x,y){function y-x};) //5,4,3,2,1
```
### 复杂的排序
```
hash = {
    '马云':167.22,
    '马化腾':379,
    '李彦宏':229.11
};
//按资产排序 倒序
var a = ['马云','马化腾','李彦宏']
a.sort(function(x,y){
    return hash[y] - hash[x]
})
```
##### join 
```
var a = [1,2,3];
a.join() ; //1,2,3
a.join('--') //1--2--3
```
##### concat()
>concat会返回一个新的数组
```
var a = [1,2,3]
var b = [4,5,6]
var c = a.concat(b);
// 返回一个新的数组  [1,2,3,4,5,6]  

// 非常厉害的用法快速复制一个数组
var d =a.concat([]) // 新数组 [1,2,3] 
```
##### map() 
> 跟forEach的区别
- forEach没有返回值
- map有返回值
>map的含义 是映射 
>![](https://sltrust.github.io/note/img/note025_5.png)
```
var a = [1,2,3]
a.map(function(){return value*2}) // [2,3,6]
//箭头函数的写法
a.map(value=>value*2)
```
##### filter
> 过滤
```
var a = [1,2,3,4,5,6,7,8,9,10]
a.filter(function(value){
    return value%2===0;
})
// [2,4,6,8,10]
```
##### reduce
> 减少 降
```
var a = [1,2,3,4,5,6,7,8,9,10]
//第一个参数是一个函数 函数有两个参数 第一个参数 之前的结果 当前的值
//第二个参数代表初始值
a.reduce(function(sum,n){
  return sum+n
},0) 
// 55

//箭头函数
a.reduce((sum,n)=>sum+n,0);
```
##### map用reduce代替
```
var a  = [1,2,3];
a.reduce(function(arr,n){
  arr.push(n*2);
  return arr
},[])  //[2,4,6]
```

##### filter用reduce代替
```
var a  = [1,2,3,4,5,6,7,8,9,10];
a.reduce(function(arr,n){
  if(n%2===0){
    arr.push(n)
  }
  return arr
},[])  //[2,4,6,8,10]
```









