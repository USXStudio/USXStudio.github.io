---
title: 小型node服务器
date: 2018-04-26 22:31:49
tags: node
---

### node增删改查

#### localhost:8200/user 获取所有用户(GET)
#### localhost:8200/user 创建一个用户(POST)
#### localhost:8200/user/aaa 修改一个用户(PATHC)
#### localhost:8200/user/bbb 删除一个用户(DELETE)

> Content-Type:application/json

```
{"name":"aaa","age":118}
```


```
const http = require('http');
const server = http.createServer();
const url = require('url')

let users = [
    {name:"aaa",age:18},
    {name:"bbb",age:34},
    {name:"ccc",age:22},
    {name:"ddd",age:13},
    {name:"eee",age:19},
    {name:"fff",age:28}
];
server.on('request',(req,res)=>{
    console.log('请求方式==>'+req.method)
    console.log('请求的路径==>'+req.url)

    //获取请求的路径
    const parseUrl = url.parse(req.url)

    if(parseUrl.path.indexOf('/user')===-1){
        res.statusCode = 403;
        res.end(`${res.statusCode} not allowed!`)
        return;
    }
    console.log(req.method)
    switch(req.method){
        case 'GET':
            console.log('get users')
            res.statusCode = 200;
            res.end(JSON.stringify(users))
            break;
        case 'POST':
            console.log('create user')
            let user = '';
            req.on('data',(buffer)=>{
                const userStr = buffer.toString();
                let CT = req.headers['content-type'];
                if(CT === 'application/json'){
                    user = JSON.parse(userStr);
                    users.push(user);
                }
            })
            req.on('end',()=>{
                res.statusCode = 201;
                res.end('user has created!')
            })
            break;
        case 'PATCH':
            console.log('update users')
            //   /user/aaa
            let userName = parseUrl.path.substring(6)
            console.log(userName)
            req.on('data',(buffer)=>{
                const userStr = buffer.toString();
                let CT = req.headers['content-type'];
                if(CT === 'application/json'){
                    let update = JSON.parse(userStr)
                    let user = users.find(u=>u.name === userName)
                    user.age = update.age;
                }
            })
            req.on('end',()=>{
                res.statusCode = 201;
                res.end('user has updated!')
            })
            break;
        case 'DELETE':
            console.log('DELETE user')
            if(parseUrl.path.indexOf('/user/')>-1){
                let userName = parseUrl.path.substring(6)
                let index = users.findIndex(u=>u.name === userName);
                users.splice(index,1);
                res.statusCode = 200;
                res.end(JSON.stringify(users))
            }
            break;
    }
})

server.listen(8200)
```
