---
title: iview引入问题
date: 2018-06-06 16:35:42
tags: vue
---

> #### 本文针对vue-cli@2.9.3版本

> 我使用的iview版本为2.14.1

### 引入iview触发的问题

#### 本地运行正常，打包后报错提示没有某个字体文件

webpack.prod.conf.js

```
module: {
    rules: utils.styleLoaders({
        sourceMap: config.build.productionSourceMap,
        extract: false
        // extract: true
    })
}

//里边的 extract改为false,默认是true
```

[详情请参考issues](https://github.com/iview/iview/issues/515)


#### iview全局引入后，打包体积过大

> 我说的是vendor.js(打包后的1M)

##### 然后依照官网的api进行按需引用的改造

借助插件 babel-plugin-import可以实现按需加载组件，减少文件体积。首先安装，并在文件 .babelrc 中配置：

```
npm install babel-plugin-import --save-dev

// .babelrc
{
  "plugins": [["import", {
    "libraryName": "iview",
    "libraryDirectory": "src/components"
  }]]
}
```

然后这样按需引入组件，就可以减小体积了：

```
import { Button, Table } from 'iview';
Vue.component('Button', Button);
Vue.component('Table', Table);
```

> 但是这样会报UglifyJs的错误，因为截止2018年6月6日为止，iview里的内容使用了es6语法，然而uglifyJs是不支持的！

```
UglifyJs do not support ES6+, you can also use babel-minify for better treeshaking: https://github.com/babel/minify
```

于是我就又各种搜索报错终于找到答案

[原文链接](https://blog.csdn.net/nongweiyilady/article/details/78893504)


> #### 解决方案(参考以上链接)

1. 在webpack.base.conf.js中，我们先在js编译的时候添加如下：

```
{
    test: /\.js$/,
    loader: 'babel-loader',
    include: [resolve('src'),
    resolve('test'),resolve('/node_modules/iview/src'),resolve('/node_modules/iview/packages')]
}
```

2. 先让iview的es6语法经过babel来转换，然后在build/webpack.prod.conf.js中，注释掉原来的uglifyJs，引入外部的uglifyJs对js进行压缩混淆，代码如下：

```
//改动 你需要下载依赖 
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

// 注释掉原来的 UglifyJs 因为 它不支持ES6
// new webpack.optimize.UglifyJsPlugin({
//   compress: {
//     warnings: false
//   },
//   sourceMap: true
// }),
new UglifyJsPlugin({
    // 使用外部引入的新版本的js压缩工具
    parallel: true,
    sourceMap: true,
    uglifyOptions: {
    ie8: false,
    ecma: 6,
    warnings: false,
    mangle: true,
    // debug false
    output: {
        comments: false,
        beautify: false,
        // debug true
    },
    compress: {
        // 在UglifyJs删除没有用到的代码时不输出警告
        warnings: false,
        // 删除所有的 `console` 语句
        // 还可以兼容ie浏览器
        drop_console: true,
        // 内嵌定义了但是只用到一次的变量
        collapse_vars: true,
        // 提取出出现多次但是没有定义成变量去引用的静态值
        reduce_vars:true
    }
    }
}),
```

3. 不要忘了安装依赖

```
cnpm install uglifyjs-webpack-plugin --save-dev
```

4. 详情配置请参考官网

[uglifyjs-webpack-plugin](http://www.css88.com/doc/webpack/plugins/uglifyjs-webpack-plugin/)

> ##### 经历如上步骤后，我的vendor.js打包后体积为 300K(我只用了iview 的 DatePicker)
> ##### 但是iview即使按需引入css还是挺大的大约300K还需要在 main.js里引入 依然很大。。。。。。



