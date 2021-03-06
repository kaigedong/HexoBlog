---
title: pytorch01
date: 2017-09-22 18:37:46
tags:
---


# pytorch 学习笔记

<!-- more -->


## 1. 安装

[pytorch主页](http://pytorch.org/)有详细说明.

`os: osx`-`package mamnager: conda`-`python 3.6`-`no CPU`

```shell
conda install pytorch torchvision -c soumith 
python
>>> import torch 出现错误。
解决：pip install numpy --upgrade
```

## 2.Torch 或 Numpy 

```python
import torch
import numpy as np

np_data = np.arange(6).reshape((2,3))
torch_data = torch.from_numpy(np_data)
tensor2array = torch_data.numpy()
print(
    '\nnumpy array:', np_data,          # [[0 1 2], [3 4 5]]
    '\ntorch tensor:', torch_data,      #  0  1  2 \n 3  4  5    [torch.LongTensor of size 2x3]
    '\ntensor to array:', tensor2array, # [[0 1 2], [3 4 5]]
)
```

>**`np.arange`与`range`**
>
>使用方式相似，`np.arange([start,] stop[, step,], dtype=None)`;`range(start, stop[, step])`
>
>但`np.arange`产生的是`array `类型的数据。
>
>**torch data对应的numpy array类型**: 可通过函数互换。
>
>```python
>In [14]: torch_data
>Out[14]:
>
> 0  1  2
> 3  4  5
>[torch.LongTensor of size 2x3]
>
>In [15]: np_data
>Out[15]:
>array([[0, 1, 2],
>       [3, 4, 5]])
>```

## 3. Torch 中的数学运算

### 简单计算

`Tensor` 和`numy`一样，且能互相转换，转换前后没有发生内存的复制，所以修改其中某一方的值，也会引起另一方改变。

```
# np.array 转为Tensor
import numpy as np
a = np.ones(5)
# 使用torch.from_numpy即可实现转换
b = torch.from_numpy(a)  # out: [torch.DoubleTensor of size 5]
np.add(a, 1, out=a)
print(a)
print(b)            # a b的值都变为2
```



> `Tensor`，即`numpy`中的多维数组。上面已经提到过，PyTorch对其加入了GPU支持。同时，PyTorch中的`Tensor`可以与`numpy`中的`array`很方便地进行互相转换。
>
> ```python
> x = torch.Tensor(5, 3)  # construct a 5x3 matrix, uninitialized
> # 或者随机填充
> y = torch.rand(5, 3)    # construct a randomly initialized matrix
> # 使用size方法可以获得tensor的shape信息，torch.Size 可以看做 tuple
> x.size()                # out: torch.Size([5, 3])
> ```

```python
data = [-1, -2, 1, 2]
tensor = torch.FloatTensor(data)  # 转换成32位浮点 tensor

np.abs(data)
torch.abs(tensor)  

np.sin(data)
torch.sin(tensor)

np.mean(data)
torch.mean(tensor)
```

> ```shell
> abs 
> numpy:  [1 2 1 2] 
> torch:  
>  1
>  2
>  1
>  2
> [torch.FloatTensor of size 4]
> ```
>
> numpy 是横着排列，tensor 是纵向排列的(`\n`)。

### 矩阵计算

```python
# matrix multiplication 矩阵点乘
data = [[1,2], [3,4]]
tensor = torch.FloatTensor(data)  # 转换成32位浮点 tensor

np.matmul(data, data)
torch.mm(tensor, tensor) 
[[7,10]，[15,22]]
```

### GPU计算

PyTorch中使用GPU计算很简单，通过调用`.cuda()`方法，很容易实现GPU支持。

```
# let us run this cell only if CUDA is available
if torch.cuda.is_available():
    print('cuda is avaliable')
    x = x.cuda()
    y = y.cuda()
    x + y          # 在GPU上进行计算
```


>  感谢*[莫烦python](https://morvanzhou.github.io/tutorials/)*，[xmfbit](https://xmfbit.github.io/2017/02/25/pytorch-tutor-01/)
