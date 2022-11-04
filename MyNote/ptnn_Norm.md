# LayerNorm

```python
import torch

import torch.nn as nn

x = torch.arange(24, dtype= torch.float32).reshape(2, 3, 2, 2)
ln1 = nn.LayerNorm((3, 2, 2))
ln2 = nn.LayerNorm((2, 2))
ln3 = nn.LayerNorm(2)

y1, y2, y3 = ln1(x), ln2(x), ln3(x)
y1, y2, y3


```

```
(tensor([[[[-1.5933, -1.3036],
           [-1.0139, -0.7242]],
 
          [[-0.4345, -0.1448],
           [ 0.1448,  0.4345]],
 
          [[ 0.7242,  1.0139],
           [ 1.3036,  1.5933]]],
 
 
         [[[-1.5933, -1.3036],
           [-1.0139, -0.7242]],
 
          [[-0.4345, -0.1448],
           [ 0.1448,  0.4345]],
 
          [[ 0.7242,  1.0139],
           [ 1.3036,  1.5933]]]], grad_fn=<NativeLayerNormBackward>),
 tensor([[[[-1.3416, -0.4472],
           [ 0.4472,  1.3416]],
 
          [[-1.3416, -0.4472],
           [ 0.4472,  1.3416]],
 
          [[-1.3416, -0.4472],
           [ 0.4472,  1.3416]]],
 
 
         [[[-1.3416, -0.4472],
           [ 0.4472,  1.3416]],
 
          [[-1.3416, -0.4472],
           [ 0.4472,  1.3416]],
 
          [[-1.3416, -0.4472],
           [ 0.4472,  1.3416]]]], grad_fn=<NativeLayerNormBackward>),
 tensor([[[[-1.0000,  1.0000],
           [-1.0000,  1.0000]],
 
          [[-1.0000,  1.0000],
           [-1.0000,  1.0000]],
 
          [[-1.0000,  1.0000],
           [-1.0000,  1.0000]]],
 
 
         [[[-1.0000,  1.0000],
           [-1.0000,  1.0000]],
 
          [[-1.0000,  1.0000],
           [-1.0000,  1.0000]],
 
          [[-1.0000,  1.0000],
           [-1.0000,  1.0000]]]], grad_fn=<NativeLayerNormBackward>))
```

可见LayerNorm对于参数指定Shape作Norm,从最低维度开始 依次匹配（不匹配则报错), 不进行LayerNorm的维度 彼此是独立 平行的

LayerNorm只由输入参数的shape决定

# BatchNorm

BatchNorm对除了第二维的其它维度作Norm,输入参数的维度也是第二维的shape，与LayerNorm不同(输入的shape是作Norm的维度的shape)

### 1.nn.BatchNorm1d(num_features)

```python
    1.对小批量(mini-batch)的2d或3d输入进行批标准化(Batch Normalization)操作
    2.num_features：
            来自期望输入的特征数，该期望输入的大小为'batch_size x num_features [x width]'
            意思即输入大小的形状可以是'batch_size x num_features' 和 'batch_size x num_features x width' 都可以。
            （输入输出相同）
            输入Shape：（N, C）或者(N, C, L)
            输出Shape：（N, C）或者（N，C，L）
 
      eps：为保证数值稳定性（分母不能趋近或取0）,给分母加上的值。默认为1e-5。
      momentum：动态均值和动态方差所使用的动量。默认为0.1。
      affine：一个布尔值，当设为true，给该层添加可学习的仿射变换参数。
    3.在每一个小批量（mini-batch）数据中，计算输入各个维度的均值和标准差。gamma与beta是可学习的大小为C的参数向量（C为输入大小）
      在训练时，该层计算每次输入的均值与方差，并进行移动平均。移动平均默认的动量值为0.1。
      在验证时，训练求得的均值/方差将用于标准化验证数据。 
    4.例子
            >>> # With Learnable Parameters
            >>> m = nn.BatchNorm1d(100) #num_features指的是randn(20, 100)中（N, C）的第二维C
            >>> # Without Learnable Parameters
            >>> m = nn.BatchNorm1d(100, affine=False)
            >>> input = autograd.Variable(torch.randn(20, 100)) #输入Shape：（N, C）
            >>> output = m(input)  #输出Shape：（N, C）
```

### 2.nn.BatchNorm2d(num_features)

```python
    1.对小批量(mini-batch)3d数据组成的4d输入进行批标准化(Batch Normalization)操作
    2.num_features： 
            来自期望输入的特征数，该期望输入的大小为'batch_size x num_features x height x width'
            （输入输出相同）
                输入Shape：（N, C，H, W)
                输出Shape：（N, C, H, W）
      eps： 为保证数值稳定性（分母不能趋近或取0）,给分母加上的值。默认为1e-5。
      momentum： 动态均值和动态方差所使用的动量。默认为0.1。
      affine： 一个布尔值，当设为true，给该层添加可学习的仿射变换参数。
    3.在每一个小批量（mini-batch）数据中，计算输入各个维度的均值和标准差。gamma与beta是可学习的大小为C的参数向量（C为输入大小）
      在训练时，该层计算每次输入的均值与方差，并进行移动平均。移动平均默认的动量值为0.1。
      在验证时，训练求得的均值/方差将用于标准化验证数据。
    4.例子
        >>> # With Learnable Parameters
        >>> m = nn.BatchNorm2d(100) #num_features指的是randn(20, 100, 35, 45)中（N, C，H, W)的第二维C
        >>> # Without Learnable Parameters
        >>> m = nn.BatchNorm2d(100, affine=False)
        >>> input = autograd.Variable(torch.randn(20, 100, 35, 45))  #输入Shape：（N, C，H, W)
        >>> output = m(input)
```

batchNorm参考[here](https://blog.csdn.net/qq_41105401/article/details/114339269)

test code: [here](../code/testpt_bn_ln.ipynb)

