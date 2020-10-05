---
description: Learnings about Numpy
---

# Numpy

## Basics

### Create a Numpy array

```python
# create a i, j matrix with elements as 1
np.ones([i, j])

# create a i, j matrix with random elements
np.empty([i, j])

# create a i, j matrix with elements as 0
np.zeros([i, j])
```

### Numpy array attributes

```python
# get dimension of array
array.ndim

# get size of array
array.size

# get shape of array
array.shape

# get data type
array.dtype

# get element byte size
array.itemsize
```

### Random

```python
# 设定随机序列，seed确定后 结果一定 以下示例 a == b
numpy.random.seed(40)
a = numpy.random.rand(2,3)
numpy.random.seed(40)
b = numpy.random.rand(2,3)

# generate a random array(i, j) in [0,1) with equal distribution
a = numpy.random.rand(i, j)

# randn ：生成一个满足平均值为0且方差为1的正太分布随机数组(i,j)。
a = numpy.random.randn(i, j)

# randint ：在给定的范围内(i,j)生成类型为整数的随机样本数。
a = numpy.random.randint(i, j)

# binomial ：生成一个维度指定且满足二项分布的随机样本数。 n: 试验次数， p:成功概率
a = numpy.random.binomial(n, p)

# beta ：生成一个指定维度且满足beta分布的随机样本数。
a = numpy.random.beta(alpha, beta)

# normal ：生成一个指定维度且满足高斯正太分布的随机样本数。
a = numpy.random.beta(miu, delta)
```

### 切片

```python
a = np.array([[1,2,3],[3,4,5],[4,5,6]])  
print (a[...,1])   # 第2列元素
print (a[:,1])   # 第2列元素
print (a[1,:])   # 第2行元素
print (a[1,...])   # 第2行元素
print (a[:,1:])  # 第2列及剩下的所有元素
print (a[...,1:])  # 第2列及剩下的所有元素
```

### `np.pad`

```python
# pad = 1 for the 2nd dimension, pad = 3 for the 4th dimension 
# and pad = 0 for the rest
a = np.pad(a, ((0,0), (1,1), (0,0), (3,3), (0,0)), 
        mode='constant', constant_values = (0,0))

# zero pad for all dimensions, pad=pad
a = np.pad(a, ((pad,pad)), mode='constant', constant_values = (0,0))
```

## Compare 2 nparray and merge into 1

```python
import numpy as np
fg = np.random.randint(0,2,(8,8,3))
bg = np.ones((8,8,3),dtype = np.uint8)
mask = np.all(fg == 0, axis = 2)

# if mask is True then the element is from fg, otherwise from bg
result = [
    bg_pix if mask_pix else fg_pix
    for (bg_pix, mask_pix, fg_pix) in zip(
    (p for row in bg for p in row),
    (p for row in mask for p in row),
    (p for row in fg for p in row)
    )]
```

