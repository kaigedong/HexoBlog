---
title: pytorch02
date: 2017-09-22 18:37:54
tags:
---


## pytorch的变量

<!-- more -->

同样感谢[xmfbit](https://xmfbit.github.io/2017/02/25/pytorch-tutor-01/)，讲的很清楚搬过来。

说完了数据类型`Tensor`，下一步便是如何实现一个神经网络。首先，对[自动求导](http://pytorch.org/docs/autograd.html)做一说明。

我们需要关注的是`autograd.Variable`。这个东西包装了`Tensor`。一旦你完成了计算，就可以使用`.backward()`方法自动得到（以该`Variable`为叶子节点的那个）网络中参数的梯度。`Variable`有一个名叫`data`的字段，可以通过它获得被包装起来的那个原始的`Tensor`数据。同时，使用`grad`字段，可以获取梯度（也是一个`Variable`）。

`Variable`是计算图的节点，同时`Function`实现了变量之间的变换。它们互相联系，构成了用于计算的无环图。每个`Variable`有一个`creator`的字段，表明了它是由哪个`Function`创建的（除了用户自己显式创建的那些，这时候`creator`是`None`）。

当进行反向传播计算梯度时，如果`Variable`是标量（比如最终的`loss`是欧氏距离或者交叉熵），那么`backward()`函数不需要参数。然而如果`Variable`有不止一个元素的时候，需要为其中的每个元素指明其（由上层传导来的）梯度（也就是一个和`Variable`shape匹配的`Tensor`）。看下面的说明代码。

```python
from torch.autograd import Variable
x = Variable(torch.ones(2, 2), requires_grad = True)
x     # x 包装了一个2x2的Tensor
"""
Variable containing:
 1  1
 1  1
[torch.FloatTensor of size 2x2]
"""
# Variable进行计算
# y was created as a result of an operation,
# so it has a creator
y = x + 2
y.creator    # out: <torch.autograd._functions.basic_ops.AddConstant at 0x7fa1cc158c08>
z = y * y * 3  
out = z.mean()   # out: Variable containing: 27 [torch.FloatTensor of size 1]
# let's backprop now
out.backward()  # 其实相当于 out.backward(torch.Tensor([1.0]))
# print gradients d(out)/dx
x.grad
"""
Variable containing:
 4.5000  4.5000
 4.5000  4.5000
[torch.FloatTensor of size 2x2]
"""
```

下面的代码就是结果不是标量，而是普通的`Tensor`的例子。

```python
# 也可以通过Tensor显式地创建Variable
x = torch.randn(3)
x = Variable(x, requires_grad = True)
# 一个更复杂的 op例子
y = x * 2
while y.data.norm() < 1000:
    y = y * 2
# 计算 dy/dx
gradients = torch.FloatTensor([0.1, 1.0, 0.0001])
y.backward(gradients)
x.grad
"""
Variable containing:
  204.8000
 2048.0000
    0.2048
[torch.FloatTensor of size 3]
"""
```

## 获取Variable 里面的数据：

上面已经说过，`Variable`包装了`data`的字段,

```python
print(variable.data)# 获取包装的数据信息。
```

