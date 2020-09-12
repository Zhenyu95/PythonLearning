# PyTorch

## Basics

### Initiate

```python
# torch.FloatTensor ：用于生成数据类型为浮点型的Tensor，传递给torch.FloatTensor的参数
# 可以是一个列表，也可以是一个维度值。
a = torch.FloatTensor(2, 3) # random float of dimension 2, 3
b = torch.FloatTensor([2, 3, 4, 5])

# torch.IntTensor ：用于生成数据类型为整型的 Tensor，传递给 torch.IntTensor的参数
# 可以是一个列表，也可以是一个维度值。
a = torch.IntTensor(2, 3) # random int of dimension 2, 3
b = torch.IntTensor([2, 3, 4, 5])

# torch.rand ：用于生成数据类型为浮点型且维度指定的随机Tensor，
# 随机生成的浮点数据在0～1区间均匀分布。
a = torch.rand(2, 3)

# torch.randn ：用于生成数据类型为浮点型且维度指定的随机Tensor，
# 随机生成的浮点数的取值满足均值为0、方差为1的正太分布。
a = torch.randn(2, 3)

# torch.range ：用于生成数据类型为浮点型且自定义起始范围和结束范围的Tensor，
# 传递的参数有三个，起始值、结束值和步长
a = torch.range(1, 10, 1)

# torch.zeros ：用于生成数据类型为浮点型且维度指定的Tensor，
# 浮点型的Tensor中的元素值全部为0。
a = torch.zeros(2, 3)
```

### Tensor Calculation

```python
# torch.abs ：将参数传递到torch.abs后返回输入参数的绝对值作为输出，
# 输入参数必须是一个Tensor数据类型的变量。
a = torch.abs(torch.randn(2,3))

# torch.add ：将参数传递到 torch.add后返回输入参数的求和结果作为输出，
# 输入参数既可以全部是Tensor数据类型的变量，也可以一个是Tensor数据类型的变量，另一个是标量。
a = torch.randn(2, 3)
b = torch.randn(2, 3)
c = torch.add(a, b)
d = torch.add(a, 10)

# torch.clamp ：对输入参数按照自定义的范围进行裁剪，最后将参数裁剪的结果作为输出。
a = torch.randn(2, 3)
b = torch.clamp(a, -0.1, 1)

# torch.div ：将参数传递到torch.div后返回输入参数的求商结果作为输出，
# 参与运算的参数可以全部是Tensor数据类型的变量，也可以是Tensor数据类型的变量和标量的组合。
a = torch.randn(2, 3)
b = torch.randn(2, 3)
c = torch.div(a, b)
d = torch.div(a, 10)

# torch.mul ：将参数传递到 torch.mul后返回输入参数求积的结果作为输出，
# 参与运算的参数可以全部是Tensor数据类型的变量，也可以是Tensor数据类型的变量和标量的组合。
a = torch.randn(2, 3)
b = torch.randn(2, 3)
c = torch.mul(a, b)
d = torch.mul(a, 10)

#torch.pow ：将参数传递到torch.pow后返回输入参数的求幂结果作为输出，
# 参与运算的参数可以全部是Tensor数据类型的变量，也可以是Tensor数据类型的变量和标量的组合。
a = torch.randn(2, 3)
b = torch.randn(2, 3)
c = torch.pow(a, b)
d = torch.pow(a, 10)

# torch.mm ：将参数传递到 torch.mm后返回输入参数的求积结果作为输出. 
# torch.mm运用矩阵之间的乘法规则进行计算,参数的维度要满足矩阵乘法的前提条件。
a = torch.randn(2, 3)
b = torch.randn(3, 2)
c = torch.mm(a, b)

# torch.mv ：将参数传递到torch.mv后返回输入参数的求积结果作为输出，
# torch.mv运用矩阵与向量之间的乘法规则进行计算，被传入的参数中: 
# 第1个参数代表矩阵，第2个参数代表向量，顺序不能颠倒。
a = torch.randn(2, 3)
b = torch.randn(3)
c = torch.mv(a, b)
```

### Transpose: torch.t\(\)

```python
x = torch.randn(2, 3)
>>> x
tensor([[ 0.4875,  0.9158, -0.5872],
        [ 0.3938, -0.6929,  0.6932]])
>>> torch.t(x)
tensor([[ 0.4875,  0.3938],
        [ 0.9158, -0.6929],
        [-0.5872,  0.6932]])
```

