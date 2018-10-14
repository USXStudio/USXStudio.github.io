---
title: 自己实现AJAX
date: 2017-12-07 15:43:46
tags: 前端知识点
---

# AJAX

```
let request = new XMLHttpRequest();

request.open('get','/xxx');

request.send();

request.onreadystatechange = function(){
    if(request.readyState === 4){
        if(request.status >= 200 && request.status <300){
            console.log(request.responseText);
        }else{
            console.log(request.status);
        }
    }
}
```

### request.open('请求方式',url,[可选参数]);

> #### get
```
request.open('get','/xxx');
```

> #### post
```
request.open('post','/xxx');
```

### request.setHeaders(key,value);

> #### 自己设置请求第二部分
```
let request = new XMLHttpRequest();

request.open('get','/xxx');

request.setRequestHeader('name','hjx');
request.setRequestHeader('Content-Type','x-www-form-urlencoded');

request.send();
...
```

![](https://raw.githubusercontent.com/slTrust/note/master/img/note035_1.png)

### request.send('你设置的内容');

> #### 设置请求的第四部分

> #### 注意:如果是get方式  第四部分是不可见的

```
let request = new XMLHttpRequest();
            
request.open('post','/xxx');
request.setRequestHeader('name','hjx');
request.setRequestHeader('Content-Type','x-www-form-urlencoded');

request.send('我偏要设置请求的第四部分');
```

![](https://raw.githubusercontent.com/slTrust/note/master/img/note035_2.png)

### 客户端获取服务器的响应部分
```
request.onreadystatechange = function(){
    if(request.readyState === 4){
        if(request.status >= 200 && request.status <300){
            console.log(request.status);
            console.log(request.statusText);
            console.log(request.getAllResponseHeaders());
            console.log(request.responseText);
        }else{
            console.log(request.status);
        }
    }
}
```

### 问题
1. JS可以任意设置请求 Header 吗？
    - 第一部分 request.open('get','/xxx')
    - 第二部分 request.setRequestHeader('name','hjx');
    - 第四部分 request.send('a=1&b=2');
2. JS可以获取响应的所有部分 吗？
    - 第一部分 request.status/request.statusText
    - 第二部分 request.getResponseHeader()/request.getAllResponseHeaders()
    - 第四部分 request.responseText
    
#### jQuery.ajax()

```
window.jQyery = function(nodeOrSelector){
    let nodes = {}; 
    /************省略非本次知识的重点*************/
    }
    return nodes;
}

window.jQuery.ajax = function(){
    
}

window.$ = window.jQuery;

```

![](https://raw.githubusercontent.com/slTrust/note/master/img/note035_3.png)

### 先搞出ajax的函数体

```
window.jQuery.ajax = function(url,method,body,successFn,failFn){

    let request = new XMLHttpRequest();

    request.open(method,url);
    
    request.send(body);

    request.onreadystatechange = function(){
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <300){
                successFn.call(undefined,request.responseText)
            }else{
                failFn.call(undefined,request)
            }
        }
    }
}

btn.addEventListener('click',function(){
    window.jQuery.ajax(
        '/xxx',
        'get',
        'a=1&b=2',
        (res)=>{console.log(res)},
        (err)=>{console.log()}
    );
})
```

### 你睡了一晚上，第二天基本ajax里的参数你基本就忘光了

> 优化一下

#### 提取参数

```
window.jQuery.ajax = function(options){
    let url = options.url;
    let method = options.method;
    let body = options.body;
    let successFn = options.successFn;
    let failFn = options.failFn;
    let request = new XMLHttpRequest();
    request.open(method,url);
    request.send(body);
    request.onreadystatechange = function(){
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <300){
                successFn.call(undefined,request.responseText)
            }else{
                failFn.call(undefined,request.status)
            }
        }
    }
}

btn.addEventListener('click',function(){
    window.jQuery.ajax({
        url:'/xxx',
        method:'get',
        body:'a=1&b=2',
        successFn:(res)=>{console.log(res)},
        failFn:(err)=>{console.log()}
    });
})
```

#### 如果我想设置请求头咋办？ 

> 继续添加往options上挂载key就好了

```
window.jQuery.ajax = function(options){
    let url = options.url;
    let method = options.method;
    //继续设置参数
    let headers = options.headers;
    let body = options.body;
    let successFn = options.successFn;
    let failFn = options.failFn;
    let request = new XMLHttpRequest();
    request.open(method,url);
    for(let key in headers){
        let value = headers[key];
        request.setRequestHeader(key,value);
    }
    request.send(body);
    request.onreadystatechange = function(){
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <300){
                successFn.call(undefined,request.responseText)
            }else{
                failFn.call(undefined,request.status)
            }
        }
    }
}

btn.addEventListener('click',function(){
    window.jQuery.ajax({
        url:'/xxx',
        method:'get',
        headers:{
            'Content-Type':'application/x-www-form-urlencoded',
            'name':'hjx'
        },
        body:'a=1&b=2',
        successFn:(res)=>{console.log(res)},
        failFn:(err)=>{console.log()}
    });
})

```

#### 借鉴jQuery.ajax(url[,settings])　接受一个或者两个参数

```
window.jQuery.ajax = function(options){
    let url;
    if(arguments.length===1){
        url = options.url;
       
    }else if(arguments.length===2){
        url = arguments[0];
        options = arguments[1];
    }
    let method = options.method;
    let headers = options.headers;
    let body = options.body;
    let successFn = options.successFn;
    let failFn = options.failFn;
    ......
}

btn.addEventListener('click',function(){
    window.jQuery.ajax(
        '/xxx',
        {
            method:'get',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded',
                'name':'hjx'
            },
            body:'a=1&b=2',
            successFn:(res)=>{console.log(res)},
            failFn:(err)=>{console.log()}
        }
    );
})
```

#### ES6的方式写以上代码

> 恢复只接收一个参数

```
window.jQuery.ajax = function(options){
    //ES6解构赋值
    let {url,method,body,successFn,failFn,headers} = options;

    let request = new XMLHttpRequest();
    request.open(method,url);
    for(let key in headers){
        let value = headers[key];
        request.setRequestHeader(key,value);
    }
    request.send(body);
    request.onreadystatechange = function(){
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <300){
                successFn.call(undefined,request.responseText)
            }else{
                failFn.call(undefined,request.status)
            }
        }
    }
}
```

#### 更进一步的优化

```
    //ES6解构赋值
window.jQuery.ajax = function( {url,method,body,successFn,failFn,headers}){
    let request = new XMLHttpRequest();
    request.open(method,url);
    for(let key in headers){
        let value = headers[key];
        request.setRequestHeader(key,value);
    }
    request.send(body);
    request.onreadystatechange = function(){
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <300){
                successFn.call(undefined,request.responseText)
            }else{
                failFn.call(undefined,request.status)
            }
        }
    }
}
```

#### ES6方式交换a和b的值
```
//以前的方式
var a = 1;
var b = 2;
var temp = a;
a = b;
b = temp;
//ES6方式
var x = 1;
var y = 2;
[x,y] = [y,x];

```

#### ES6 把x的值当作key

```
//以前
var x = '???';
var o = {};
o[x]= true;

//ES6
var x = '???';
var o = {
    [x]:true
}
```

### Promise规范

> jQuery已经实现的方式

```
$.ajax({
    url:'/xxx',
    method:'get'
}).then(
        (res)=>{console.log(res) return '成功';},
        (err)=>{console.log(err)} return '失败';})
  .then(
        (res)=>{console.log(res)}, //这里的res就是 如果上一个成功了 它的返回值
        (err)=>{console.log(err)}) // 同上
```

### Promise版本的ajax

```
window.jQuery.ajax = function({url,method,body,headers}){
    return new Promise(function(resolve,reject){
        //ES6解构赋值
        let request = new XMLHttpRequest();
        request.open(method,url);
        for(let key in headers){
            let value = headers[key];
            request.setRequestHeader(key,value);
        }
        request.send(body);
        request.onreadystatechange = function(){
            if(request.readyState === 4){
                if(request.status >= 200 && request.status <300){
                    resolve.call(undefined,request.responseText)
                }else{
                    reject.call(undefined,request.status)
                }
            }
        }
    });
}

btn.addEventListener('click',function(){
    window.jQuery.ajax({
            url:'/xxx',
            method:'get',
            headers:{
                'Content-Type':'application/x-www-form-urlencoded',
                'name':'hjx'
            },
            body:'a=1&b=2'
        }).then(
                (res)=>{console.log(res)},
                (err)=>{console.log(err)}
            )
})
```



