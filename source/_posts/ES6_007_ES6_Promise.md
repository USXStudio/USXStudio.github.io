---
title: ES6_Promise(七)
date: 2017-03-15 17:42:37
tags: ES6速学
---

### 回调函数

```
function myAsync(callback){
    setTimeout(function(){
        console.log('执行完成');
        callback('data');
    }, 2000);
}

myAsync(function(data){
    console.log(data);
});
```

### Promise 解决了什么痛点 

> 回调地狱   代码如下

```
myAsync(function(data){
    //第一个请求成功接收到数据  然后根据数据触发第二次请求
    console.log('第一个请求完成！')
    myAsync2(function(data){
        console.log('第二个请求完成！')
        myAsync3(function(data){
            console.log('第三个请求完成！')
             //一直一直的回调  排错是很困难的 代码也恶心
        })
    })
})
```

### 我的第一个Promise使用

```
let myPromise = new Promise(function(resolve, reject){
    //当异步代码执行成功时，我们才会调用resolve(...),
    // 当异步代码失败时就会调用reject(...)
    //使用setTimeout(...)来模拟异步代码  实际可以是Ajax请求
    console.log('异步请求中');
    setTimeout(function(){
        resolve("成功!"); //2秒后代码执行成功  调用resolve
    }, 2000);
});

myPromise.then(function(successMessage){
    //successMessage的值是上面调用resolve(...)方法传入的值.
    console.log("success! " + successMessage);
});
```

### 解决回调地狱之链式操作

> Promise的优势在于:
- 可以在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。 是不是有种jquery的感觉！
        
> 链式操作的用法

- Promise只是能够简化层层回调的写法，比传递callback的方式简单

```
function myAsync1(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步1完成');
            resolve('data1');
        }, 1000);
    });
    return p;            
}
function myAsync2(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步2完成');
            resolve('data2');
        }, 2000);
    });
    return p;            
}
function myAsync3(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步3完成');
            resolve('data3');
        }, 2000);
    });
    return p;            
}
//依次的异步操作
myAsync1()
    .then(function(data){
        console.log(data);
        return myAsync2();
    })
    .then(function(data){
        console.log(data);
        return myAsync3();
    })
    .then(function(data){
        console.log(data);
    });
```
### reject( )是什么鬼？

> 就是异步操作通常有两种结果一个是成功一个是失败

```
function myAsync(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步1完成');
            var num = Math.random()*100;
            if(num>60){
                 resolve('运气不错通过啦！'+num);
            }else{
                 reject('太差了，分不够！'+num)
            }
           
        }, 1000);
    });
    return p;            
}
myAsync()
    .then(
        function(data){
            console.log('resolved');
            console.log(data);//成功了  你手气不错！
        }, 
        function(reason, data){
            console.log('rejected');
            console.log(reason);//哎呦 挺倒霉啊！  再来一次吧！！
        }
    );
```

### catch( ) 你肯定听过 try ...catch...

```
function myAsync(){
    var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    console.log('异步任务完成');
                    resolve('data');
                }, 1000);
            });
    return p;    
}
myAsync()
    .then(function(data){
        console.log('resolved');
        console.log(data);
        console.log(xxxx.xxxx); //此处的xxxx.xxxx未定义  报错
    })
    .catch(function(err){
        console.log('rejected');
        console.log(err);
    });
```

### all( ) 

> Promise的all方法提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调。

```
Promise
.all([myAsync1(), myAsync2(), myAsync3()])
.then(function(results){
    console.log(results);
});
```

- 用Promise.all来执行，all接收一个数组参数，里面的值最终都算返回Promise对象。
- 这样，三个异步操作是并行执行的，等到它们都执行完后才会进到then里面。
- 那么，三个异步操作返回的数据哪里去了呢？
- 都在then里面呢，all会把所有异步操作的结果放进一个数组中传给then，就是上面的results。

### race( )

> all( ) 的效果实际上是 ==>以"执行时间最慢的"为准执行回调

> race( ) 以最快的为准
race的用法与all一样，把上面myAsync3的延时改为0.5秒运行

```
Promise
.race([myAsync1(), myAsync2(), myAsync3()])
.then(function(results){
    //返回三个异步请求最快的那个
    console.log(results);
});
```












