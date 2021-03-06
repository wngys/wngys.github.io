# 1.常见的数据类型转换

GPU tensor 不能直接用numpy()转换，必须先转换为CPU tensor,即 tensor.cpu().numpy() 转换为numpy数据
如果tensor是标量，使用.item()方法取出数据

```python
# List转numpy.array：
temp = np.array(list)
# numpy.array转List：
arr = temp.tolist()
```

# 2. DasetFolder

```python
from torch.utils.data import DataLoader, Subset, ConcatDataset, Dataset
from torchvision.datasets import DatasetFolder
```

```python
train_dataset_label = DatasetFolder(
    r'food-11/training/labeled', lambda x: Image.open(x), extensions='.jpg',transform=train_tfm) 
```

class DatasetFolder 用于组织数据集，通常目录结构:

```
directory/
├── class_x
│   ├── xxx.ext
│   ├── xxy.ext
│   └── ...
│       └── xxz.ext
└── class_y
    ├── 123.ext
    ├── nsdf3.ext
    └── ...
    └── asd932_.ext
```

类似有ImageFolder等

除此之外 记录一下用到的
Subset, Concatset等

# 3.一般的Dataset类写法:

> ```python
> class FoodDataset(Dataset):
>     def __init__(self,path,tfm=test_tfm,files = None):
>      super(FoodDataset).__init__()
>      self.path = path
>      self.files = sorted([os.path.join(path,x) for x in os.listdir(path) if x.endswith(".jpg")])
>      if files != None:
>          self.files = files
>      print(f"One {path} sample",self.files[0])
>      self.transform = tfm
> 
>     def __len__(self):
>      return len(self.files)
> 
>     def __getitem__(self,idx):
>      fname = self.files[idx]
>      im = Image.open(fname)
>      im = self.transform(im)
>      #im = self.data[idx]
>      try:
>          label = int(fname.split("/")[-1].split("_")[0])
>      except:
>          label = -1 # test has no label
>      return im,label
> ```

#  4.筛选阈值大于一定值的tensor

```python
masks = values > threshold
# bool索引:
Subvalue = values[masks]
# 或者
indice = torch.arange(0, length)[masks]
Subvalue = values[indice]
```

# 5.tqdm



# 6.半监督训练：

预测伪标签->筛选->concat with labeled data -> train -> 预测伪标签....
伪标签transform 和 带标签训练集一样 都是 train_tfm
伪标签 DataLoader不要shuffle,否则筛选的数据集顺序和标签顺序 有问题
数据分布会混乱，结果就是训练集loss下降，验证集loss上升

# 7.Trick: Ensemble

model1 完全不使用半监督方法
model2,model3,model4 先不使用半监督训练一个模型，加载训练好的模型参数，再使用半监督方法训练
model2,3,4可以选择不同的数据增强方法或者模型结构

Esemble technique:
ensemble1: vote model1, model2, model3 预测标签取众数
ensemble2: fusion model2, model3, model4 预测概率求平均 然后取最大为标签
ensemble3: vote model1, model2, model3
ensemble4: vote ensemble1, ensemble2, ensemble3

# 8.更规范代码

训练集和验证集 每个epoch记录训练集和验证集loss和acc
保存最大的测试集acc对应的模型
每个epoch可以将结果情况写入日志
设置stale，stale过大说明 验证集模型acc很久没有提升 结束训练

# 9.这次用到的数据增强:

```
train_tfm = transforms.Compose([
    # Resize the image into a fixed shape (height = width = 128)
    # You may add some transforms here.
    transforms.RandomRotation(40),
    transforms.RandomAffine(degrees=0, translate=(0.2, 0.2), shear=0.2),
    transforms.RandomHorizontalFlip(p=0.5),
    transforms.Resize((224, 224)),
    # ToTensor() should be the last one of the transforms.
    transforms.ToTensor(),
    transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
])
```

# 10.numworkers, pinmemory确实能加速很多 

```
os.environ["CUDA_DEVICE_ORDER"] = "PCI_BUS_ID"
os.environ["CUDA_VISIBLE_DEVICES"] = "0, 1, 2, 3" #调整物理编号与逻辑编号对应 0->0 .....
device_ids = [0, 1, 2, 3]

model = nn.DataParallel(model, device_ids).to(device) # 四卡训练

watch -n 1 nvidia-smi 查看显卡实时状态
```



next to do :

cross validation

查看data augment 图片数目增加了?

