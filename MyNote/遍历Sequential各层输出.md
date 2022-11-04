# 遍历Squential各层输出：

## 代码框架：

```python
import torch

import torch.nn as nn

kernel_List = [12, 4, 4, 4, 4, 4]

channel_List = [128, 256, 512, 512, 512, 400]



class ConvBlock(nn.Module):

  def __init__(self, in_channel, out_channel, kernel_sz, padding, stride = 2) -> None:

​    super().__init__()

​    self.conv = nn.Conv2d(in_channel, out_channel, kernel_sz, stride, padding)

​    self.bn = nn.BatchNorm2d(out_channel)

​    self.relu = nn.ReLU()

​    self.dropout = nn.Dropout()

  def forward(self, x):

​    x = self.relu(self.bn(self.conv(x)))

​    x = self.dropout(x)

​    return x



def get_convBlocks(in_channel):

  layerNum = len(kernel_List)

  blocks = []

  blocks.append(ConvBlock(in_channel, channel_List[0], kernel_List[0], int(kernel_List[0] / 2 - 1)))

  for i in range(1, layerNum):

​    blocks.append(ConvBlock(channel_List[i-1], channel_List[i], kernel_List[i], int(kernel_List[i] / 2 - 1)))

  return blocks



class DeepFold(nn.Module):

  def __init__(self, in_channel) -> None:

​    super().__init__()

​    self.convLayer = nn.Sequential(*get_convBlocks(in_channel))

  

  def forward(self, x):

​    return self.convLayer(x)
```



## 1.迭代遍历：

```python
print(x.shape)

for layer in model.convLayer:

  x = layer(x)

  print(x.shape)

  print('-'*60)
```

## 2.hook

```python
outputList = []
#定义钩子函数

def hook(self, layer: nn.Module,  output: torch.tensor):

  outputList.append(output)

x = torch.rand(64, 3, 256, 256)

model = DeepFold(3)
#为各层添加钩子，保留各层输出

for layer in model.convLayer:

  layer.register_forward_hook(hook)
#forward后，outputList就保留了各层输出

y = model(x)

for ele in outputList:

  print(ele.shape)
```

