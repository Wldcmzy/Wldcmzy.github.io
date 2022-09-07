---
title: pyecharts画极简单图
date: 2022-09-07 09:10:56
categories:
  - [基础知识, python, pyecharts]
tags:
  -	画图
---



## 建议

建议去文档搬砖

## 例子

```python
from pyecharts import options as opts
from pyecharts.charts import Pie
from pyecharts.faker import Faker

list1 = [list(z) for z in zip(Faker.choose(), Faker.values())]
print(list1)
c = (
    Pie()
    .add("",list1 )
    .set_colors(["blue", "green", "yellow", "red", "pink", "orange", "purple"])
    .set_global_opts(title_opts=opts.TitleOpts(title="Pie-设置颜色"))
    .set_series_opts(label_opts=opts.LabelOpts(formatter="{b}: {c} （{d}%）"))
    .render("饼图的绘制.html")
)
```

