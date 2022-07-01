# 划分数据集
## 方法一<br>
数据的feature和label是数组
```
perm = torch.randperm(len(x))
train_indices = perm[:int(len(perm)*0.8)]
valid_indices = perm[int(len(perm)*0.8):]
t_x = [x[i] for i in train_indices]
t_y = [y[i] for i in train_indices]
v_x = [x[i] for i in valid_indices]
v_y = [y[i] for i in valid_indices]
```
## 方法二  
Dataset类数据集  
使用pytorch封装方法 `torch.utils.data.random_split()`
### 描述:
随机将一个数据集分割成给定长度的不重叠的新数据集。可选择固定生成器以获得可复现的结果（效果同设置随机种子）。  
### 参数
![image](https://user-images.githubusercontent.com/73680840/176926098-8da226ac-3f3a-402b-a41b-0758e21b149e.png)

### 示例代码
![image](https://user-images.githubusercontent.com/73680840/176926223-a34e9d71-f330-4a64-a13b-2827d09cc6d7.png)




