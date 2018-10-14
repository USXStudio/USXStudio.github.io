---
title: vue多页面配置问题汇总
date: 2018-05-22 16:17:20
tags:  vue
---

> #### 注意：本项目基于vue-cli 2.9.3

### 基于上一篇vue多页面的配置方式

#### 很多时候我们需要引用同一模块

> 当你定义api接口的时候,或者加载去除默认样式的reset.css文件

> 我们应把复用的模块放在同一目录下,如图我这里把复用的文件放在「modules」里

![](https://raw.githubusercontent.com/slTrust/note/master/img/vuenote_202_01.png)

找到webpack.base.conf.js文件,添加如下代码

```
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
      // 公共文件路径加载
      'css':'@/modules/css',
      'js':'@/modules/js',
      'components':'@/components'
    }
}
```

当你的页面里需要使用该模块的时候就可以像这样引入

```
import 'css/reset.css';
import 'js/flexible.js';
```

#### 引入淘宝移动端适配方案后flexible.js 与引入 mint-ui的冲突

> 当你做了移动端适配后，也就是配置了postcss-px2rem后,引入mint-ui框架会把UI框架里组件像素单位「px」转为「rem」


我使用的是postcss依赖  所以.postcssrc.js配置如下

```
module.exports = {
  "plugins": {
    // to edit target browsers: use "browserslist" field in package.json
    "autoprefixer": {},
    "postcss-px2rem": {
       remUnit: 75
    }
  }
}
```

> 移动端如果使用阿里的flexible适配方案，现有的移动端UI库大部分都不适用了，移动端的UI库默认的initial-scale=1，在高清屏上flexible会调整initial-scale=1/dpr，UI的样式被整体缩小了

> ##### 这样就导致UI变形

- ##### 解决方式一：就是你要用样式覆盖,这样就更恶心了

- ##### 解决方式二：依然使用flexible适配方案，但是不使用px2rem

> 注掉使用postcss-px2rem的功能，采用手写rem的方式，这样引入的ui组件样式就不会变小

```
module.exports = {
  "plugins": {
    // to edit target browsers: use "browserslist" field in package.json
    "autoprefixer": {},
    //"postcss-px2rem": {
    //   remUnit: 75
    //}
  }
}
```

#### 手写rem问题来了，又回到原始的方式了

我们可以使用强大的IDE插件来帮我们解决问题

> 我用的是VScode，设计稿750px,如果每个需要适配的单位都去计算rem是很麻烦的，推荐px to rem这个插件，然后将16设置为75（设计稿为750px）

![](https://raw.githubusercontent.com/slTrust/note/master/img/vuenote_202_02.png)

这个时候我们直接写px，选中后alt+z就直接转换为rem了

- 先选中如下文本 「16px」
- 再按住「alt」
- 最后按「z」

#### npm run build问题

> 打包之后的文件为绝对路径,需在服务环境下才能打开

不方便我们本地调试，我们需要相对路径

1. 打开config文件夹下的index.js文件
2. 修改build中assetsPublicPath为’’（原本为’/‘）

#### 静态资源路径问题

```
-|src
-|static
---|img
------|aa.png
```

> ##### 在html或者css里请这样书写，打包后这个路径也是对的

```
<img src="static/img/aa.png">
```

> 注意这样是错的

```
../../static/img/aa.png
```
