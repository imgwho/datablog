---
date: 2020-07-29
title: 使用Tableau画上下甘特图
tags:
  - Tableau
  - dataviz-tips
description: 摘要：本文主要介绍Tableau 画上下甘特图的关键步骤和注意事项等。
---

# 使用Tableau画上下甘特图
先看效果
![图 0](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/e395609a9c63e47ecb4eb31eb457f4de0dcbbc9d18fe797e3c356ef75151a4c1.webp)  

# 难点：
先看以下数据源，非常简洁

| 年 | 年平均水位（m） |
| --- | --- |
| 2002 | 16.00271607 |
| 2003 | 15.75254231 |
| 2004 | 15.36662571 |
| 2005 | 15.89260357 |
| 2006 | 15.44266279 |
| 2007 | 15.3341966 |
| 2008 | 15.51047561 |
| 2009 | 14.78714882 |
| 2010 | 16.19155746 |
| 2011 | 14.5368649 |
| 2012 | 15.63157219 |
| 2013 | 14.70402016 |
| 2014 | 15.25017935 |
| 2015 | 15.13395162 |

再看以下如何计算2002年到2015年的平均水位线：
![图 1](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/e1944042844e78a9e3fe1f8b7bb10408dacf148b134c56f1d228d70f990a598e.webp)  



即使用window_avg函数
![图 2](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/dfb1bf677f87c6525bebb1cb0b78f33622907821711310c2e35f0393025ce595.webp)  



然后是高出平均线、低于平均线的值用两种颜色
![图 3](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/241efc1ab469d500e9407607bf133121e68d7db8fdf60623a8a2df8f6a2b3321.webp)  


附文件：
[鄱阳湖历年平均水位.zip](https://www.yuque.com/attachments/yuque/0/2020/zip/93504/1596027662884-08a4e7fe-f6e9-437a-be97-5918122947d4.zip?_lake_card=%7B%22uid%22%3A%221596027663472-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fzip%2F93504%2F1596027662884-08a4e7fe-f6e9-437a-be97-5918122947d4.zip%22%2C%22name%22%3A%22%E9%84%B1%E9%98%B3%E6%B9%96%E5%8E%86%E5%B9%B4%E5%B9%B3%E5%9D%87%E6%B0%B4%E4%BD%8D.zip%22%2C%22size%22%3A31156%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22LBfYe%22%2C%22card%22%3A%22file%22%7D)


<Comment />