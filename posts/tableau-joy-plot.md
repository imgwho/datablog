---
date: 2020-09-09
title: 使用tableau画欢乐图(joy plot)
tags:
  - tableau
  - dataviz-tips
description: 摘要：如何用tableau画joy plot？
---


# 原文翻译

主要翻译自 [https://www.flerlagetwins.com/2018/05/joy-plot.html#](https://www.flerlagetwins.com/2018/05/joy-plot.html#)<br />（感谢西南小组tableau交流问答群刘瑞协助）<br />

## 模板

我不会深入研究计算等细节，而是提供一个创建这些图表的模板。这个模板包括两个组件，即一个Excel电子表格和一个Tableau工作簿。我开发这些模板的目的是为了让你尽可能轻松地插入自己的数据。也就是说，它确实做了某些假设。首先，它假设你已经聚合了你的数据。

因为你的欢乐图的轴可以是一个连续的日期或连续的数字（就像我的27个俱乐部的欢乐图一样），我实际上创建了两个版本的Excel和Tableau模板，但它们的基本结构是一样的。

Excel电子表格（你可以在这里找到它们：数字轴模板和日期轴模板）有三个表。数据、排序和模型。

模型处理所需的数据脚手架，使多边形以零开始和结束。它将在Tableau数据模型中加入我们的实际数据。

Sort表为您提供了一种简单的方法来为您正在绘制的维度的成员排序。每个成员都需要在这个表里有一行，有一个Key，它将定义排序顺序。

最后，数据表将用于填充您的预聚合数据。它只包含四列。如果需要，你可以添加更多，但这四列是Tableau模板所需要的。这些列如下：<br />![](https://2.bp.blogspot.com/--0GbTojp77Q/WtK9AKo7TbI/AAAAAAAAKAg/Llrl42iiauAGz4Mgz3vGhI386bSctjzeQCLcBGAs/s1600/Sample%2BData.PNG#align=left&display=inline&height=688&originHeight=688&originWidth=462&status=done&style=none&width=462)

## 字段解释

辅助列（Join） - 这一列的目的只是将Model表中的每一行连接到Data工作表中的每一行。但不用太担心这个问题。你只需要确保每一行在这一列中都有一个 "连接 "的值。注意：严格来说，我们可以在Tableau中使用连接计算将这些表连接在一起，但为了简单起见，我经常喜欢在我的数据集中包含一个单独的列。

维度（dimension）-你要绘制的维度。

时间（time）-将控制你的轴的连续值。这将是一个数字或日期，取决于你使用的模板。

值（value）-您的汇总值。

每个绘图点将有一条记录。下面是一些样本数据的工作表的样子。<br />

![图 2](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/c9e28b5f077d5638a2529063e18a9ef49fb69f5acfe54e2577f4cb7048358839.png)  





## 其他说明

一旦你已经填充了数据表， 然后你需要将其连接到Tableau。首先下载模板Tableau工作簿，这些工作簿在我的Tableau公共页面上。数字模板和日期模板。然后编辑数据源并将其连接到你的Excel模板。工作簿应该自动更新以反映你的数据。

根据你的数据中的维数以及你希望在区域图之间的间距，你会想要调整Spacing参数。只要稍微调整一下数值，直到它看起来恰到好处。另外，根据你想要的标签对齐方式，你还需要对标签表进行一些微调。调整顶部和底部的填充，直到它们与你的图表很好地对齐。

从这里开始，你可以对图表做任何你想做的事情--改变颜色、添加过滤器、更新工具提示等，就像你平时做的那样。下面是上面显示的样本数据的效果。

这就是它的全部内容。如果你使用这个模板来创建你自己的joy plot，我很乐意看到它。但请不要仅仅因为你能创建一个。一定要仔细检查你的用例，并首先确保它是正确的图表类型。


# 理解与应用

光有上面的材料其实是完全不知道原理的，所以我尝试研究作者的tableau文件的各个字段和函数，以便理解。

## 数据准备

需要data、sort、model三个表。<br />[Joy Plot Template - Dates.xlsx](https://www.yuque.com/attachments/yuque/0/2020/xlsx/93504/1598968661692-aa5106c7-c2f3-4401-961e-224dcdf344eb.xlsx)<br />[Joy Plot Template - Dates.xlsx](https://www.yuque.com/attachments/yuque/0/2020/xlsx/93504/1598968628017-8fd8143f-a4d5-4e05-a014-6f09eedbfed0.xlsx?_lake_card=%7B%22uid%22%3A%221598968629448-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fxlsx%2F93504%2F1598968628017-8fd8143f-a4d5-4e05-a014-6f09eedbfed0.xlsx%22%2C%22name%22%3A%22Joy%20Plot%20Template%20-%20Dates.xlsx%22%2C%22size%22%3A23262%2C%22type%22%3A%22application%2Fvnd.openxmlformats-officedocument.spreadsheetml.sheet%22%2C%22ext%22%3A%22xlsx%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22qmeDZ%22%2C%22card%22%3A%22file%22%7D)<br />进行连接后如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1598967804294-780dd78f-65a8-4c91-bbab-6137ced5f3cb.png#align=left&display=inline&height=751&originHeight=751&originWidth=1457&size=151734&status=done&style=none&width=1457)

## 图形解构

观察需要最少字段形成的图。<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1598970022982-9cddd00c-cd6c-4c90-9205-422dd5fdaaab.png#align=left&display=inline&height=966&originHeight=966&originWidth=1451&size=149981&status=done&style=none&width=1451)<br />可以发现主要有4个字段：time、Value Adjusted for Dimension、Key、Path。<br />重要的字段如下：

### Path

关键就是这个计算字段，勾勒出图形的形状线段。

```css
//绘制曲线路径
//当曲线分类为"Primary"时，使用日期为绘制路径
IF [曲线分类Purpose]="Primary" THEN [日期Time]
//当曲线分类为"Zeros"时，按最大日期计算返回的日期，按天增加，即下面的直线
ELSE DATE((INT([Max Value])*2) - INT([日期Time]) + 1)
END
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1599572138702-963c0ddd-dd00-477e-8772-7688a98b01b6.png#align=left&display=inline&height=969&originHeight=969&originWidth=1702&size=168587&status=done&style=none&width=1702)<br />再将标记的形状改为多边形即可<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1599573453100-75febf84-607d-4d0b-9b1e-6f30e313b4e3.png#align=left&display=inline&height=822&originHeight=822&originWidth=2281&size=92788&status=done&style=none&width=2281)<br />最后勾选所有dimension，再按降序排序key即可<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1599574213167-b21e3936-b5c3-46dd-a13c-23a16324313c.png#align=left&display=inline&height=1250&originHeight=1250&originWidth=1686&size=155732&status=done&style=none&width=1686)

### Max Value

```css
//按分类维度取最大日期，并转换为浮点数值，以便计算
FLOAT({FIXED [分类维度Dimension]: MAX([日期Time])})
```


### Value Adjusted for Dimension

决定填充的曲线

```css
// This is the y axis position, adjusted to place each plot on a different spot.
// 这是y轴位置，调整后将每个地块放在不同的位置。

//MAX([Value Adjusted]) + (MAX([Number of Values])-[Dimension Index]+1)*[Spacing]

//折线计算结果
[Value Adjusted] 

+ 
//不同分类维度的起点位置。
([Number of Values]-[维度序号Key]+1)*[Spacing分类维度间距]
```


### Value Adjusted

```css
//当曲线分类为"Primary"时，使用实际值，生成折线
IF [曲线分类Purpose]="Primary" THEN [Value]


//当曲线分类为"Zeros"时，使用0，生成底部直线
ELSE 0
END
```


### Number of Values

```css
// Number of values in the dimension.
//分类维度的总个数
{FIXED : COUNTD([分类维度Dimension])}
```


### Spacing

调整间距<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1599471647568-94a5bd1a-851c-4980-940a-47f609e5aaf6.png#align=left&display=inline&height=292&originHeight=292&originWidth=572&size=17785&status=done&style=none&width=572)


<Comment />