# 反向传播图与设置可跟踪梯度的Tensor

参考博客: https://ginsoda.github.io/deep_learning/2019/07/29/computing-graph-of-pytorch/

​				https://blog.csdn.net/weixin_42782150/article/details/106854349

小实验：

```python
import torch
import torch.nn as nn

class model(nn.Module):
    def __init__(self):
        super().__init__()
        self.w = torch.tensor(2, dtype = torch.float, requires_grad=True)
        self.b = torch.tensor(1, dtype =torch.float, requires_grad=True)
    def forward(self, x):
        return self.w * x + self.b
    # def parameters(self, recurse: bool = True) -> Iterator[Parameter]:
    #     return super().parameters(recurse)
    def param(self):
        return self.w, self.b

x1 = torch.tensor([[1,1],[1,5]], dtype=torch.float)
themodel = model()
optimizer = torch.optim.SGD(themodel.param(), lr=1e-3, momentum=0.9)
themodel.train()
y1 = themodel(x1)

y = 0
# y = torch.tensor(0, dtype=torch.float, requires_grad=True) 
# Error A leaf Variable that requires grad is being used in an in-place operation
y += min(torch.tensor(0, dtype= torch.float, requires_grad = False), y1[0][0] + y1[1][1])
# y += min(0, y1[0][0] + y1[1][1]) # Error 'int' object has no attribute 'backward'
y += max(torch.tensor(0, dtype=torch.float,
         requires_grad=True), y1[0][0] + y1[1][1])
# y = x1 * x2
optimizer.zero_grad()
y.backward()
print(themodel.w.grad)
print(themodel.b.grad)
optimizer.step()

```


错误一:

```python
# y 初始化
y = torch.tensor(0, dtype=torch.float, requires_grad=True) 
# Error A leaf Variable that requires grad is being used in an in-place operation
```
错误二:

```python
y = 0
y += min(0, y1[0][0] + y1[1][1])
# y设置为不能跟踪梯度的0 # Error 'int' object has no attribute 'backward'
# 当然如果取到min后面的y1, 则可以跟踪到梯度 忽略了前面的0
```
错误三:

```python
y = 0
y += min(torch.tensor(0, dtype= torch.float, requires_grad = False), y1[0][0] + y1[1][1])
# y设置为不能跟踪梯度的tensor(0) # Error element 0 of tensors does not require grad and does not have a grad_fn
# 当然如果取到min后面的y1, 则可以跟踪到梯度 忽略了前面的tensor 0
```

改正：

初始化常数0，将错误三里的requires_grad设置为True。

如果后面的数小于0，则min取到后面，可跟踪model梯度，如果大于0，则取到tensor 0, 此时model参数梯度为None,但是不会报错。

```python
# 使用Variable也可以， 不过现在的版本可以直接设置tensor requires_grad
from torch.autograd import Variable
loss += max(Variable(torch.Tensor([0]), requires_grad = True).to(device), nega_cos - posi_cos + self.m)
# 这是我复现论文DeepFold 计算loss的代码一句
```

