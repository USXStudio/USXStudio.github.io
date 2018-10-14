---
title: webpack
date: 2018-02-13 16:02:49
tags: 前端知识点
---

### webpack

[webpack：从入门到真实项目配置](https://zhuanlan.zhihu.com/p/30701816)

[关于 webpack 的面试题有哪些？](https://www.zhihu.com/question/266788138)

![什么是工程化](https://raw.githubusercontent.com/slTrust/note/master/img/note045_01.png)

### 回到之前的项目

[基础版本项目地址](https://sltrust.github.io/cv/cv030/030mycv.html)

1. 安装node-sass  
    
    > github 搜索 node-sass
    
    > ### cnpm install node-sass -g 
    
    > -g 这样你就可以任何目录都能执行自动化工具

    - 如果你安装node-sass出错 直接google搜索 node-sass安装失败(window通常会失败)
    
2. 安装成功
 
    - 如何使用node-sass

        1. 修改后缀。直接把你的main.css修改为  main.scss
        2. 查看文档发现如下
            
            ```
            node-sass src/style.scss dest/style.css
            ```
3. sass语法
    
    - 嵌套
    
    ```
    css写法
    .topNavBar nav {padding:5px;}
    .topNavBar nav ul{ list-style:none;margin:0;padding:0;}
    
    scss写法 是不是简洁了很多
    
    .topNavBar{
        nav {
            padding:5px;
            ul{
                list-style:none;
                margin:0;
                padding:0;
            }
        }
    }
    
    ```
4. sass和scss的区别

    > #### ruby社区写了sass规则比scss更简洁，但是前端大多数看不懂
    
    aa.sass
    ```
    body
     p
      a
       background : red
    ```
    aa.css
    ```
    body p a {
      background: red; }

    ```
    
    sass可以自动加“{}” 和“;” 但是前端小菜看不懂啊。。。那你就吃土吧！
    
    > 于是出现了 scss
    
    aa.scss
    ```
    body{
        p{
            a{
                background:red;
            }
        }
    }
    ```
    
    > 前端一看： 懂了。。。
    
5. 但是即使这样我们引用的却不能是scss文件，而是转化后的css文件

    > ### 改个背景色还要重新编译一次才能看到效果 ？ 484傻

    - google node sass watch
    
    ```
    node-sass  main.scss main.css -w main.scss
    ```

#### Babel

> #### 我们自己写项目可以大量使用ES6,而真实项目可能需要在IE上运行

1. [babel](http://babeljs.io/)

    1. 第一步
    ```
    安装官方提示的步骤
    npm install --save-dev babel-cli babel-preset-env
    ```
    
    2. 第二步 新建.babelrc
    ```
    {
      "presets": ["env"]
    }
    ```
    
    3. 第三步点击 向导 点击cli下
    
    > ![](https://raw.githubusercontent.com/slTrust/note/master/img/note045_02.png)
    
    > ![](https://raw.githubusercontent.com/slTrust/note/master/img/note045_03.png)
    
    4. 继续按照步骤
    
    ```
    npm install --save-dev babel-cli
    ```
    
    5. 由于我们没有pageage.json
    
    ```
    npm init 一直回车回车回车
    ```
    
    6. 因为 npm install --save-dev babel-cli的时候我们还没有 pageage.json
    
    7. 再次运行 
    
    ```
    npm install --save-dev babel-cli
    ```
    
    > 你会发现增加了一行
    
    ```
    {
      "devDependencies": {
    +   "babel-cli": "^6.0.0"
      }
    }
    ```
    
    8. 在pageage.json里添加如下代码 
    
    ```
      {
        "name": "my-project",
        "version": "1.0.0",
    +   "scripts": {
    +     "build": "babel src -d lib"
    +   },
        "devDependencies": {
          "babel-cli": "^6.0.0"
        }
      }
    ```
    > ![](https://raw.githubusercontent.com/slTrust/note/master/img/note045_04.png)
    
    > ![](https://raw.githubusercontent.com/slTrust/note/master/img/note045_05.png)
    
    9. 这个时候提示你可以运行 npm run build 
    
    > 此时报错了   ！ 因为命令行已经发展到了非常变态的地步，就算你是个老手也会报错
    
        - 因为npm run bulid 实际运行的是 script里的 "babel src -d lib"
        - 而"babel src -d lib"会优先寻找./node_modules/.bin/目录
        - 此时你就要知道全局安装和局部安装的区别了
        
    > ### 由于我们是局部安装 所以我们要这样运行
    
    ```
    ./node_modules/.bin/babel src -d lib
    ```
    
    10. 我们肯定也要时时修改，而不是边保存边编译
    
    ```
    ./node_modules/.bin/babel src -d lib --watch
    ```
    
### 以上叫自动化

> 但是有一个问题 CSS自动化要开一个命令行，JS也要开一个，这样无形中多了好多窗口

#### 实际开发中如下图

![](https://raw.githubusercontent.com/slTrust/note/master/img/note045_06.png)

> 改变目录解构

```
--|app
----|src
------|css
--------|a.scss
--------|b.scss
--------|c.scss
------|js
--------|a.js
--------|b.js
--------|c.js
------|img
--------|a.jpg
--------|b.jpg
--------|c.jpg
------|index.html

------|dist
//这个目录是根据src里生成的

------|node_modules 第三方包
------|vendors 第三方文件
```

1. node-sass 命令的变化监听目录
    ```
    node-sass src/css/ -o dist/css/  -w src/css/
    ```
2. babel 变化
    
    ```
    ./node_modules/.bin/babel src/js -d dist/js --watch
    ```

#### 而我们不仅仅会修改js/css还会修改 html/img文件 这个时候又怎么办呢？

#### 于是我们又发现了watch-cli一个命令行工具可以监听文件变化

[watch-cli](https://github.com/doowb/watch-cli)

> 监听html文件的变化

```
watch -p "src/index.html" -c "cp src/index.html  dist/index.html"
```
> 监听img文件的变化

```
watch -p "src/img/**/*" -c "cp src/img  dist/img"
```

### 经过如上步骤 我们每次开发一个项目就要开四个命令行工具

> 在node诞生之后 前端想尽办法代替上面的四个命令行

- grunt 不用学了 已经过时了 被gulp替代了  gulp比它快
- gulp 也不用学了 被webpack替代了

### webpack

[我的webpack示例仓库](https://github.com/slTrust/webpack-demo)

### parcel代替webpack(依然不稳定，需要时间去磨合，建议还是webpack虽然快死了)

> 尝试去试一下这个 秒杀webpack啊简直！



---


#### webpack就是干(实践webpack加入es6 加入sass)

> 不知有多少人想说 fack webpack

### google 搜索  webpack

> 进入官网 点击github

> 提示你安装

```
# 建议安装 webpack@3.10.0 因为我用的是这个 这步骤可以先跳过因为你还没有package.json
npm i --save-dev webpack 
```

继续往下看，发现毫无线索，.... 发 现get started 点击


#### step1 新建目录  

```
mkdir webpack_simple
cd webpack_simple
# 你想使用webpack安装进入你的依赖一定要先保证你有package.json文件

npm init 
...  一路回车 你有了自己的package.json

# 安装webpack依赖   --save-dev是添加到package.json里 这样别人拿到你的这个文件直接  npm i就可以直接安装依赖了
npm i webpack@3.10.0 --save-dev

```

#### step2 我们的目录结构如下

```
 webpack_simple
  |- package.json
+ |- index.html
+ |- /src
+   |- index.js
```

#### step3 webpack的命令需要一个配置文件 改变目录如下

```
  webpack-demo
  |- package.json
+ |- webpack.config.js
  |- /dist
    |- index.html
  |- /src
    |- index.js
```

#### step4 webpack.config.js的编写

```
const path = require('path');

module.exports = {
  entry: './src/index.js',   //入口文件
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')  //打包后的目录
  }
};
```

#### step5 好了  在确保你有node的情况下

```
# 注意是 webpack_simple目录下运行如下命令
webpack
```

#### step6 大功告成 你发现 多了一个dist目录 



#### 小小细节 提交的时候千万不要提交如下目录的东西

你该新建 .gitignore 文件

```
node_modules/
/dist/
```

> 你需要新建.gitignore文件 内容如上

> 现在你已经成功的实现打包js

### 现在都ES8了   我们至少得使用ES6吧！

> 目标引入 babel-loader

### github里搜索  babel-loader

#### 看文档 注意看你当前依赖的webpack的版本 它会有对应的跳转链接

#### 看文档 注意看你当前依赖的webpack的版本 它会有对应的跳转链接

#### 看文档 注意看你当前依赖的webpack的版本 它会有对应的跳转链接

> 知道为啥说三次吗？ 因为你不点这个链接估计卡你一宿都不知道为啥错了

#### 进入对应版本链接说明文档

> webpack 3.x babel-loader 7.x | babel 6.x 我选择了这个  376 于是点击了  7.x branch

#### 继续看 7.x版本对应的安装依赖文档

> ## Install

> webpack 1.x | babel-loader <= 6.x

> webpack 2.x | babel-loader >= 7.x (recommended) (^6.2.10 will also work, but with deprecation warnings)

> webpack 3.x | babel-loader >= 7.1

```
yarn add babel-loader babel-core babel-preset-env webpack --dev
We recommend using yarn, but you can also still use npm:

# 我用的npm所以用这个
npm install --save-dev babel-loader babel-core babel-preset-env 
# 安装之后你肯定心急了  运行下webpack
webpack
```

#### 你可能发现npm下载好慢 那是因为是从国外下载所以慢 

```
# 设置淘宝镜像 cnpm
 npm install -g cnpm --registry=https://registry.npm.taobao.org

以后你就可以用  cnpm i 包名
的方式下载国内的资源镜像
```

#### 根据babel-loader的文档提示 它说有个module参数  你可以试试

> 自行脑补然后把module 结构添加到你的webpack.config.js里

```
const path = require('path');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist/js/')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env']
          }
        }
      }
    ]
  }
};
```

现在你可以写一写ES6语法了


#### 我们还有scss

### github里搜索  sass-loader

#### 看文档 安装依赖

```
npm install sass-loader node-sass webpack --save-dev
```

#### 添加模块  rules是个数组  脑补添加该对象

```
// webpack.config.js
module.exports = {
	...
    module: {
        rules: [{
            test: /\.scss$/,
            use: [{
                loader: "style-loader" // creates style nodes from JS strings
            }, {
                loader: "css-loader" // translates CSS into CommonJS
            }, {
                loader: "sass-loader" // compiles Sass to CSS
            }]
        }]
    }
};
```

```
const path = require('path');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist/js/')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env']
          }
        }
      },
      {
        test: /\.scss$/,
        use: [{
            loader: "style-loader" // creates style nodes from JS strings
        }, {
            loader: "css-loader" // translates CSS into CommonJS
        }, {
            loader: "sass-loader" // compiles Sass to CSS
        }]
      }
    ]
  }
};
```

### 然后写对应的scss文件  

```
# 在index.js里引入
import '../css/main.scss'

... 命令行输入 webpack
报错  说没有style-loader


```

#### 诀窍 只要报错一般提示少什么东西  然后   你就 cnpm install 提示的文件名

```
cnpm i style-loader  --save-dev
cnpm i css-loader --save-dev
```

#### 突然性的报错  有可能是依赖包下载有问题

```
最佳实践就是  rm -rf node_modules
然后 cnpm i 重新安装依赖
```

#### 千万别参考别人的配置然后一路查

> 这样你就会陷入绝境，因为别人的配置也不知道是从哪里copy来的他能跑通你就不一定了。出了错你就哭吧！

> #### 最佳实践就是照着官方demo写，因为你发现错了，别人还可能跟你遇到同样的问题，这样你还可以参考，一旦你去用了别人的配置，webpack配置这么繁琐，有经验的前端还会翻车呢？更何况你呢？而且你英文不一定很好！














































