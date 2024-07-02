---
date: 2024-07-02
title: Tableau实现去向省份分布的地图可视化
tags:
  - Tableau
  - Tableau教程
description: 摘要：本文以肿瘤患儿省外就医数据为实例，演示从数据处理到用Tableau进行流向可视化的过程。
---

# Tableau实现去向省份分布的地图可视化

## 处理数据-二维转一维

首先这份数据是2019-2020年全国各省份肿瘤患儿省外就医的省份分布百分比，呈现形式是二维交叉表，需要先转为一维表。  
原数据横向的是收治的省份，纵向的是常住地省份，非常难读，
使用excel中的powerquery可以轻松处理这种情况，仅需3步，后面有详细过程：  
1、选择表格数据，载入powerquery  
2、整理成常规二维表  
3、选择第一列，然后逆透视其他列

![图 0](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/eeaaf4f5554d0d93c542f418451f9428bdb4b46add5785c4fd5da136c431638f.webp)  


## 处理数据-获取省经纬度
1、利用Tableau识别省份经纬度  
2、下载数据成文件

![图 1](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/8b7e034c6d1f18a59a275993eddeacf580d613ea291315c95ef8fc9314c9bac1.webp)  


## 处理数据-合并这两个数据
1、powerquery打开这两个数据  
2、合并查询

![图 2](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/5dfc60eb94b3dcfe26698df8f13952f55534646fed22634bc9e05caa8f358093.webp)  


## Tableau可视化-合并这两个数据
1、创建流向图所需计算字段  
出发地：MAKEPOINT([常住地纬度], [常住地经度])  
出发地：MAKEPOINT([常住地纬度], [常住地经度])  
流向：IF [Value] > 0 THEN (MAKELINE([出发地], [目的地]))END   //当没有值的时候不显示线

2、可视化并调整

![图 3](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/cff862dd1653e19b374f92990ce51a9449a4d4c7ffde9d90d48581e7a75cda00.webp)  



<Comment />