---
date: 2020-07-29
title: 使用tableau画上下甘特图
tags:
  - tableau
  - dataviz-tips
description: 摘要：本文主要介绍Tableau 画上下甘特图的关键步骤和注意事项等。
---


# ![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1596027330254-04a3795a-c384-476a-b7d9-53280a460e03.png#align=left&display=inline&height=1190&originHeight=1190&originWidth=1253&size=82352&status=done&style=none&width=1253)
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
![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1596027417148-9186d6ff-4b03-4ed0-a347-d3811dffe15e.png#align=left&display=inline&height=338&originHeight=338&originWidth=566&size=20368&status=done&style=none&width=566)
即使用window_avg函数![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1596027432218-53a1def6-d532-4ced-9597-9ee638db80a7.png#align=left&display=inline&height=853&originHeight=853&originWidth=1098&size=102028&status=done&style=none&width=1098)
然后是高出平均线、低于平均线的值用两种颜色
![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1596027490107-c0a653e1-c5b6-41c9-838c-b477ac51236f.png#align=left&display=inline&height=331&originHeight=331&originWidth=565&size=14982&status=done&style=none&width=565)

附文件：
[鄱阳湖历年平均水位.zip](https://www.yuque.com/attachments/yuque/0/2020/zip/93504/1596027662884-08a4e7fe-f6e9-437a-be97-5918122947d4.zip?_lake_card=%7B%22uid%22%3A%221596027663472-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fzip%2F93504%2F1596027662884-08a4e7fe-f6e9-437a-be97-5918122947d4.zip%22%2C%22name%22%3A%22%E9%84%B1%E9%98%B3%E6%B9%96%E5%8E%86%E5%B9%B4%E5%B9%B3%E5%9D%87%E6%B0%B4%E4%BD%8D.zip%22%2C%22size%22%3A31156%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22LBfYe%22%2C%22card%22%3A%22file%22%7D)


<Comment />