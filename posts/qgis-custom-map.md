---
date: 2020-10-08
title: 如何用qgis生成自定义样式的地图
tags:
  - qgis
  - dataviz-tips
description: 摘要：地图软件qgis自定义地图教程。
---


# 前置条件
需要安装qgis，下载链接如下：
[https://qgis.org/downloads/QGIS-OSGeo4W-3.12.3-1-Setup-x86.exe](https://qgis.org/downloads/QGIS-OSGeo4W-3.12.3-1-Setup-x86.exe)  
![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1598096255606-8bca5722-74cd-4cd6-b2ee-899f20e94b0d.png)  
需要下载成都地图，我这里也已经上传，点击下载[planet_103.227,29.978_104.658,31.541.osm.shp.zip](https://www.yuque.com/attachments/yuque/0/2020/zip/93504/1598096812035-33be40e9-44b5-429b-9aac-0d9911a42ed8.zip?_lake_card=%7B%22uid%22%3A%221598096804931-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fzip%2F93504%2F1598096812035-33be40e9-44b5-429b-9aac-0d9911a42ed8.zip%22%2C%22name%22%3A%22planet_103.227%2C29.978_104.658%2C31.541.osm.shp.zip%22%2C%22size%22%3A12964029%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%228TscL%22%2C%22card%22%3A%22file%22%7D)  
解压后如下，我们需要用到其中的shp文件，所以可以按类型排序。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1598096861630-d462f5c1-d22c-4fd4-b0f6-01330ca5491e.png#align=left&display=inline&height=922&originHeight=922&originWidth=796&size=109256&status=done&style=none&width=796)
士佳大佬提示：
大范围的地图用 [https://download.geofabrik.de/asia.html](https://download.geofabrik.de/asia.html)
小范围的地图用 [https://extract.bbbike.org/](https://extract.bbbike.org/)
当然也可以自己找shp格式或者符合qgis软件的地理数据文件。
# 关键样式
有很多样式文件可以选择，大致就是这样  
需要导入这个样式文件
[https://gist.githubusercontent.com/tjukanovt/c0f00116f88fb0a1e102e0485e9aa6dc/raw/f7918bf264ca40045ea6c793dfcab07d880bb4c3/qlimt.xml](https://gist.githubusercontent.com/tjukanovt/c0f00116f88fb0a1e102e0485e9aa6dc/raw/f7918bf264ca40045ea6c793dfcab07d880bb4c3/qlimt.xml)  
作者gist目录是[https://gist.github.com/tjukanovt](https://gist.github.com/tjukanovt)
# 最终效果
![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1598603956895-fdc4c142-cae4-4b47-aa41-a3fc183a6b34.png)


<Comment />