# Contact-Lib代码前四个Model无法运行

## 参考链接

1. [关于 pytorch inplace operation](https://zhuanlan.zhihu.com/p/38475183)
2. [「Bug」问题分析 RuntimeError：which is output 0 of ReluBackward0](https://blog.csdn.net/ViatorSun/article/details/125149874)

## Error
*RuntimeError: one of the variables needed for gradient computation has been modified by an inplace operation: [torch.cuda.FloatTensor [16, 512, 3, 3]], which is output 0 of ReluBackward0, is at version 3; expected version 0 instead. Hint: enable anomaly detection to find the operation that failed to compute its gradient, with torch.autograd.set_detect_anomaly(True).*

## 主要原因

*主要是pytorch版本问题 nn.relu()内部实现可能引入了内置操作修改了某些需要计算梯度的变量*







​	