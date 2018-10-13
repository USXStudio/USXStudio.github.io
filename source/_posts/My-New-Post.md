---
title: 使用Geoserver，MapboxGL搭建离线地图
date: 2018-09-18 22:03:41
tags: mapboxgl,geoserver,gis
---

 最近一直在开发基于mapboxgl的项目，由于某些众所周知的原因，导致最近mapboxgl的后端服务一直不稳定。

与此同时 mapbox中国版后端服务对于小型To B 项目来说又比较贵，同时有些项目需要离线部署。在这个背景下我调研了一些方案 比如 [tiles-serve](https://openmaptiles.com/server/)和[geoserver](http://blog.geoserver.org/) 因为懒得使用docker 就放弃了这个方案，直接使用 geoserver 来搭建。

乘着这个机会把自己最近的工作内容分享给大家，因为涉及到的技术栈，概念比较多，所以主要讲实践。

## 1. GeoServer 环境搭建
GeoServer 是 OpenGIS Web 服务器规范的 J2EE 实现，使用它可以非常方便的发布地图数据。

简而言之使用它可以非常快速的搭建自己的地图后端服务器。然后只要你使用类似Openlayer，leaflet，mapboxgl等前端工具，你就可以非常迅速的搭建属于自己的离线地图应用。

因为我们的项目主要是可视化，所以本文的内容主要是搭建底图服务，展示地图，不涉及定位，导航，搜索等其他服务。

但是目前Geoserver 主要用来发布栅格瓦片也就是图片，所幸现在有矢量瓦片的插件，我们可以使用Geoserver 发布瓦片数据

 1. 下载 安装 [Geoserver](http://geoserver.org/release/2.12.4/) 
 Geoserver 支持Tomcat，windows，OSX，Platform Independent Binary安装。但是最新版本已经不能OSX了,鉴于为大家提供尽可能多的选择的原则，我们使用Geoserver 2.12.4 这个版本

 2. 安装 Geoserver
    若是采用Tomcat这种方式的话，应该不要要说，毕竟没几个前端会安装这个玩意，方式的话，基本一路install 即可。出问题的地方可能就是JDK没有安装或者配置好吧。

 3. 下载矢量切片插件[ Vector Tiles](http://geoserver.org/release/2.12.4/)
 插件也在Geoserver  这个页面上



## 2. 导入地图数据

## 3. mapboxgl配置