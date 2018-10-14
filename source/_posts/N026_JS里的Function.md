---
title: JS里的Function
date: 2017-11-25 17:44:34
tags: 前端知识点
---

#### 函数的基本结构
```
function fn(参数1，参数2){
  //不写return  它会自动返回 undefined  
  return undefined
}
```
#### 函数的五种声明方式
1.第一种  具名函数
```
function x(a,b){
  return a+b;
}
```
2.第二种 声明一个匿名函数 但是不能单独使用，会报错
需要赋值给一个变量
```
// function (a,b){return a+b} 报错
var fn = function(a,b){return a+b}
```
3.第三种 具名的函数赋值给一个变量
```
var x = function y(a,b){return a+b;}
//console.log(y) 报错 y没有定义
//但是单独定义一个函数  function x(){}  
//然后console.log(x) 不会报错
//js又一垃圾之处  不一致性  语法没有错误   原因在于作用域 
// function x(){} 全局作用域
// var x = function y(){ //作用域在函数里面}

```
4.第四种 window.Function 全局对象
```
var f = new Function('a','b','return a+b');

//面试题  假定n=1
var f = new Function('a','b','return a+'+n+'b')
f(1,2)  //4
```
5.第五种 箭头函数
```
var f = (x,y)=>{return x+y}
var f2 = (x,y) =>x+y;
var f3 = x=>x*2;
var f4 = (x,y)=>{var a = 1; var b =2; return a+b+x+y;}
```
#### 函数的name属性
```
function f(){}
f.name // "f"
var f2 = function(){}
f2.name // "f2"
var f3 =function f4(){}
f3.name // "f4"
f5 = new Function('x','y','return x+y');
f5.name //"anonymous"
```
#### 函数的本质
```
function sum(x,y){
    return x+y;
}
```
>  sum(1,2)      <==>     sum.call(undefined,1,2)

>  小白都这样用 sum(1,2)   sum(2,5)  这个是语法糖 但是对你深入没有任何帮助
 硬核玩家这样用  sum.call(undefined,1,2)  

>#### 只有sum.call()调用的方式  你才能真正理解  this

### this && arguments
- f.call(undefined,1,2) 的第一个参数是 this
- f.call(undefined,1,2) 第一个后面的参数是 arguments
>![](https://sltrust.github.io/note/img/note026_1.png)
```
var f = function(){
    console.log(this)
}
f.call(1)    //1
f.call('sss') //'sss'


// 潜规则   普通模式下 如果this是undefined 浏览器会自动把this变成window
f.call(undefined)   // window

//如果是严格模式  this就是原本的值
var f2 = function(){
    'use strict'
    console.log(this)
}
f2.call(undefined);  // undefined
```
###### arguments
```
var f = function(){
    console.log(arguments);
}
f.call(undefined,1,2,3)    
/*
{
0:1,
1:2,
2:3,
length:3
callee:f(){}
}
*/
```
> arguments 不是数组 它的原型链中没有Array.prototype

#### call stack 调用栈
```

```

#### 作用域面试题
```
var a = 1;
function f1(){
    var a  = 2;
    f2.call();
    console.log(a);
    
    function f2(){
        var a =3;
        console.log(a);
    }
}
f1.call();
console.log(a);
// 3
// 2
// 1
```

```
var a = 1;
function f1(){
    f2.call();
    console.log(a);
    var a= 2 ; //变量提升
    
    function f2(){
        var a =3;
        console.log(a);
    }
}
f1.call();
console.log(a);
// 3
// undefined
// 1
```
> 注意分析时要把所有变量提升

```
var a = 1;
function f1(){
    console.log(a);
    var a= 2 ; //变量提升
    f4.call();
}

function f4(){
    console.log(a);
}
f1.call();
console.log(a);
//   undefined
//   1
//   1
```

#### 闭包
```
var a  = 1;
function f4(){
  console.log(a);
}
//如果一个函数，使用了它范围外的变量，那么这个就叫闭包
```

