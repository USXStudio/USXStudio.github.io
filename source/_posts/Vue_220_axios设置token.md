---
title: axios设置token
date: 2018-04-20 11:31:37
tags: vue
---


#### axios之后台死活接不到的token参数

[https://zhuanlan.zhihu.com/p/27594936](https://zhuanlan.zhihu.com/p/27594936)

> 近期在给公司做硬件后台于是就搭了一个vue-cli项目

- 从单vue文件路由
- 从路由到路由守卫(跳转前鉴权)
- 从路由守卫到菜单鉴权
- 引入iview组件到封装基于  iview(表格、分页、加筛选、前端排序)
- 使用mixin混入对象
- 其中也踩了很多坑

> 今天就说一个就是axios设置 headers

这个问题问了好多人好多群、大致的结果就是你设置就行了  然后后台直接取


##### 问题1 登录发了两次请求

```
import axios from 'axios'
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
Vue.prototype.$http= axios;

//在你登录的时候后台返回登录信息设置全局token请求头
let params = new URLSearchParams();
                        
params.append('userAcnt',this.formSignIn.user);
params.append('nowPwd', this.formSignIn.password);
params.append('signInAvoid',0);
this.$http.post(ApiLink.URL_LOGIN,params).then((res)=>{
    //登录逻辑
    //获取token 从res里获取  我就不多说了
    this.$http.defaults.headers.token = token;
})
```

如图

![step](https://raw.githubusercontent.com/slTrust/note/master/img/vuenote_220_01.png)

![step](https://raw.githubusercontent.com/slTrust/note/master/img/vuenote_220_02.png)

![step](https://raw.githubusercontent.com/slTrust/note/master/img/vuenote_220_03.png)

1. 简单来说就是后台以为 「OPTIONS」就是你实际的请求了
2. 然后想方设法的去取 「Access-Control-Request-Headers: token 」里的内容，断点的去看请求发现 token 一直是字符串的 'token' (键名)
3. 实际上第二次的请求才是实际的请求
