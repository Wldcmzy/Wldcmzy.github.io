---
title: python进度条库tqdm
date: 2022-09-07 09:20:20
categories:
  - [基础知识, python, tqdm]
tags:
  -	进度条
  - tqdm
---



```python
import tqdm

N = 10000000

for each in tqdm.tqdm(range(N)):
    pass

for i in tqdm.trange(N):
    pass
```

