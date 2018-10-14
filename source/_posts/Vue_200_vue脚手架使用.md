---
title: vue-cli使用
date: 2018-04-11 09:11:01
tags: vue
---

#### 如果你从零开始构建vue-cli项目

1. 你需要安装node环境

[node官网](https://nodejs.org/en/download/) 下载对应版本(建议下载LTS版本相对稳定)

> 一路next千万别手欠把add to path 默认选中的取消了

2. windows用户请打开cmd 输入

```
# 查看node 版本 我的是8.9.4
node -v 

# 查看npm版本 我的是5.6.0 
npm -v
```

3. 终于你有了node的命令行环境这样你就可以安装前端相关依赖了

```
npm install 包名

# eg 这些只是例子
npm install jquery
npm install vue
npm install ...
```

#### 安装vue-cli

1. [vue官网](https://cn.vuejs.org/)
2. 点击「起步」如果是英文 「get stared」
3. 左侧菜单栏点击 安装
4. 找到命令行工具(CLI) 

    ```
    # 全局安装 vue-cli
    $ npm install --global vue-cli
    # 创建一个基于 webpack 模板的新项目
    $ vue init webpack my-project
    # 安装依赖，走你
    $ cd my-project
    $ npm run dev
    ```
5. 以上命令你就可以本地初始化一个 vue的官方模板了 

#### 如果是别人构建好的项目

给你一个项目目录(注意给别人项目的时候是不要连带node_modules的)

```
cd 项目目录
npm install
```

- 注意下载的时候由于是国外的包可能会出现丢包的情况
- 一旦丢包或者报错，就把整个node_modules目录删除
- 再次运行 npm install

#### 其实你可以使用淘宝镜像

使用说明 [淘宝镜像如何设置](https://npm.taobao.org/)

> 你可以使用 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```






