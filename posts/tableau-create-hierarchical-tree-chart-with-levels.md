---
date: 2024-07-02
title: Tableau实现带分级的单级树状图
tags:
  - Tableau
  - Tableau教程
description: 摘要：本文以肿瘤患儿省外就医数据为实例，演示从数据处理到用Tableau进行流向可视化的过程。
---

# Tableau实现带分级的单级树状图

## 最终效果

点击归经切换，下面的种类自动展开

![图 0](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/973308500eb3b8e392a323cb27625a3e3cd56f6cd4b8468f616124e2a0d84af7.webp)  


## 处理数据-按照“、”拆分列，然后合并

需要拆分里面的字段，我用python直接处理，最后得到想要的数据，原数据有163行，处理后有2000多行

![图 1](https://pub-833348ee5761457dbfac749bcd651384.r2.dev/datablog/1e1a6bf7cce55292309001766d89d6f2de5711e7fac2c9f5bfe6315a0b8b2117.webp)  


```
import pandas as pd

# 从文件读取数据（假设文件名为data.csv）
# 请确保文件的编码格式正确，通常为UTF-8
df = pd.read_csv('data.csv')

# 去除不必要的空格
df = df.apply(lambda x: x.str.strip() if x.dtype == "object" else x)

# 检查列名是否正确
print(df.columns)

# 拆分分布省份字段
df['分布省份'] = df['分布省份'].str.split('、')

# 展开分布省份字段
df_exploded = df.explode('分布省份').reset_index(drop=True)

# 拆分归经字段
df_exploded['归经'] = df_exploded['归经'].str.split('、')

# 展开归经字段
df_final = df_exploded.explode('归经').reset_index(drop=True)

# 保存清洗后的数据到新文件（假设文件名为cleaned_data.csv）
df_final.to_excel('cleaned_data1.xlsx', index=False)

# 显示清洗后的数据
print(df_final)


```

## 使用模板

在这里下载模板，<https://www.datavizcanvas.com/2020/04/01/creating-tree-chart-in-tableau/>
粘贴以下代码，与原数据进行关联，然后重命名模板中的字段，最后替换数据源即可

```
Path
0
200
```

<Comment />
