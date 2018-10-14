---
title: hexo换主题
date: 2017-10-05 18:30:09
tags: 前端知识点
---

## 主题合集

> [https://github.com/hexojs/hexo/wiki/Themes ](https://github.com/hexojs/hexo/wiki/Themes )

### 随便找一个主题，进入主题的 GitHub 首页，比如我找的是  [Jacman](https://github.com/wuchong/jacman.git)

> 复制它的 SSH 地址或 HTTPS 地址，假设地址为 https://github.com/wuchong/jacman.git

> 比如我现在在自己的hexo blog根目录  myBlog  

### 一 进入themes目录
``` bash
$ cd themes
```

### 二 克隆该风格到你本地的主题
``` bash
$ git clone https://github.com/wuchong/jacman.git
```
### 三 切换回项目目录 
``` bash
$ cd ..
```

### 四 将 _config.yml 的第 75 行改为 theme: jacman，保存

### 五 重新初始化你的博客
``` bash
$ hexo generate
```
### 六 部署
``` bash
$ hexo deploy
```
### 七 等一分钟，然后刷新你的博客页面
[jacman这个主题的Blog](https://sltrust.github.io/index.html)


## 上传源代码

> 注意「你的用户名.github.io」上保存的知识博客，并没有保存 hexo 目录，你需要再创建一个名为 generate_hexoBlog 的空仓库，用来保存 myBlog 里面的代码。

### 1.在 GitHub 创建 generate_hexoBlog 空仓库
> 顺序执行第一个段落里的命令 …or create a new repository on the command line

``` bash
echo "# ddd" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:slTrust/generate_hexoBlog.git
git push -u origin master
```
 
### 2.命令执行后，记住，千万别用 HTTPS 地址。

这样备份之后，你就不会因为误删 myBlog 目录而痛哭了。以后每次 hexo deploy 完之后，都要 push 一下代码，以备份。

这个 generate_hexoBlog 就是用来生成博客的程序，而「你的用户名.github.io」仓库就是你的博客页面。

### 3.注意如果你 hexo generate 会在public里生成文章页面 具体的就是index.html 如果内容为空

> 那是因为你设置的主题依赖没安装  

``` bash
切换到 themes目录
git clone 你想要的风格仓库地址
cd ..   
hexo generate
hexo deploy
```

