---
title: numpy常用函数和数据类型
date: 2018-04-01 16:27:34
categories: 
- Python
tags:
- numpy
---



### np.random.rand

np.random.rand(d0,d1,...,dn)表示创造一个(n+1)维的大小为d0\*d1\*...\*dn的数组，其中每一个数都是随机产生的[0, 1)内的数。

```python
>>> np.random.rand(3,2)
array([[0.91184685, 0.81463722],
       [0.9261665 , 0.960428  ],
       [0.52837831, 0.61184641]])
```

### np.float32

数据类型，32位浮点数。

### np.dot

矩阵乘法。np.dot(x, y)表示矩阵x和y相乘。

```python
>>> a = [1, 2]
>>> b = [[3], [4]]
>>> np.dot(a,b)
array([11])
```



