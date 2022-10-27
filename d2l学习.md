# 0.辅助知识

Pandas中的DataFrame和Series结构属性

https://blog.csdn.net/weixin_45760274/article/details/123380834



# 1.前言

## 1.1 机器学习的关键组件

1. 用于模型学习的数据
2. 如何转换数据模型
3. 目标函数，用来量化模型的有效性
4. 优化算法，用来调整模型参数

## 1.2 各类机器学习问题

### 1.2.1 监督学习

监督学习(supervised learning) 在给定的输入特征下预测标签

#### 1.2.1.1 回归

回归（regression）根据输入的特征预测一个**值**，任何有关**多少**的问题都是回归问题。回归问题常用的损失函数是**最小化平方误差**

#### 1.2.1.2 分类

分类（classfication）根据输入特征预测样本输入**哪一类**，模型如何判断是哪一类，一般是通过概率语言来理解，预测类别的概率的大小传达了一种模型的不确定性。分类问题常用的损失函数为**交叉熵（cross-entropy)**。

分类问题非常复杂，可以分为二元分类、多元分类、层次分类、多标签分类（学习预测不相互排斥的类别的问题）

### 1.2.1.3 搜索

搜索是在信息检索领域，对一组项目进行排序。

### 1.2.1.4 推荐系统

和搜索类似，推荐系统为“给定的用户和物品"的匹配性打分，这个分数可以说估计的评级或者购买的概率。但推荐系统可能存在反馈循环的问题（推荐的东西被更频繁的推荐）

### 1.2.1.5 序列学习

输入的样本之间是前后联系的

## 1.2.2 无监督学习

+ 聚类
+ 主成分分析
+ 因果关系
+ 生成对抗网络（generative adversarial networks）

## 1.3 dl特点

1. 端到端训练：即把特征工程的过程和建立机器学习模型的过程整合







# 2.预备知识

## 2.1 数据操作

张量(tensor)：一个n维数组

### 2.1.1 入门

改变一个张量的形状而不改变元素数量和元素值

```python
x = torch.arange(12)
x.shape
=>torch.Size([12])
x.reshape(3,4)
=>torch.Size([3,4])
```



初始化张量的一些方法

```python
torch.zeros((2,3,4))
torch.ones((2,3,4))
#初始化每个元素都从均值为0、标准差为1的标准高斯分布（正态分布）中随机采样。
torch.randn(3,4)
```

### 2.1.2 运算符

按元素运算（elementwise）

```python
x + y, x - y, x * y, x / y, x ** y  # **运算符是求幂运算
# 自然数e的x次幂
torch.exp(x)
```

拼接操作

```python
X=torch.arange(12,dtype=torch.float32).reshape((3,4))
Y=torch.tensor([[2,1,4,3],[1,1,1,1],[2,2,2,2]])
#dim=0:沿行（轴-0，形状的第一个元素）
#dim=1:沿列（轴-1，形状的第二个元素）
Z=torch.cat((X,Y),dim=1)
Z.shape
=>tensor.Size([3,8])
```

对张量所有元素进行加和，得到一个单元素张量

```python
X=tensor([1,2,3])
X.sum()
=>tensor(6.)
```

### 2.1.3 广播机制

两个形状部相同的张量在进行按元素操作时，可以通过**广播机制**：通过适当的元素复制来扩展成相同的形状，在进行元素操作。

```python
a = torch.arange(3).reshape((3, 1))
b = torch.arange(2).reshape((1, 2))
a + b
=>tensor([[0,1],[1,2],[2,3]])
```

如何判断tensor间是否可以广播：

抓住“**右对齐**”来理解广播机制是非常有好处的，判断任意tensor间是否可以广播，只需按照以下步骤就绝对不会出错了：

1. 将两操作对象的shape做右对齐
2. 空缺的位置假想为1
3. 比较同一位置处各操作对象的维数，若相同或有一个为1，则可以广播，否则无法广播

例如两个tensor的shape分别为(8, 1, 6, 5)和 (7, 1, 5)，那么是否可以广播呢？
做右对齐, 空缺的位置假想为1:
8, 1, 6, 5
1, 7, 1, 5
按照以上规则得出是可以广播的，操作结果的shape应为(8, 7, 6, 5)



### 2.1.4 索引和切片

和其他python数组一样，张量中的元素可以通过索引访问：第一个元素是0，最后一个元素是-1

```python
#把张量前两行的所有元素的值置为12
X[0:2,:]=12
```

### 2.1.5 节省内存

运行一些操作可能会导致为新结果分配内存。 例如，如果我们用`Y = X + Y`，我们将取消引用`Y`指向的张量，而是指向新分配的内存处的张量。

```python
#查看x在内存中的地址
id(x)
```

尽量原地更新

```python
Z = torch.zeros_like(Y)
print('id(Z):', id(Z))
Z[:] = X + Y
print('id(Z):', id(Z))
```

### 2.1.6 转换为其他python对象

numpy和tensor互转

```python
#tensor->ndarray
Y=X.numpy()
#ndarray->tensor
X=torch.tensor(Y)
```

大小为1的张量转标量

```python
a=torch.tensor([1])
a.item,int(a),float(1)
```

## 2.2 数据预处理

读取csv

```python
import pandas as pd

data = pd.read_csv(data_file)
print(data)
```

处理缺失值

一般用**插值法**和**删除法**

通过位置索引iloc，将data分为inputs和outputs

```python
inputs,outputs = data.iloc[:,0:2],data.iloc[:,-1]
```

对于数值型属性，使用平均值进行填充

```python
print(id(inputs))
#通过平均值填充缺失
inputs = inputs.fillna(inputs.mean())
print(id(inputs))
print(inputs)
```

对于离散值，先进行onehot编码，再NaN作为一种类型

```python
#get_dummies把离散类别转化为one-hot编码的形式
#get_dummies(data,colums=["color"])把指定的列转化为one-hot编码的形式
inputs = pd.get_dummies(inputs,dummy_na=True)
print(inputs)
```

把所有条目都转化为张量格式

```python
import torch
X,y = torch.tensor(inputs.values),torch.tensor(outputs.values),
X,y
```



作业：

1.删除缺失值最多的列

> data.drop(data, axis=0, inplace=False)
>
> axis=1删除列， inplace=1在原数据集上进行操作

```python
last=0
dc=''
for i in data.columns:
    tem = data[i].isna().sum()
    if tem>last:
        dc=i
    last = last if last>tem else tem

data1 = data.drop(dc,axis=1,inplace=False)
data1
```



2.将预处理后的数据集转换为张量格式

```python
inputs,outputs = data.iloc[:,0:2],data.iloc[:,-1]
X,y = torch.tensor(inputs.values),torch.tensor(outputs.values)
```



## 2.3 线性代数

只包含一个数值的叫标量（scalar)

向量或者轴的维度用来表示向量或轴的长度，即向量或轴的元素数量。张量的维度表示张量具有的轴数。

矩阵转置`A.T`

### 2.3.2 降维

可以对任意张量进行一个有用的操作是计算其元素的和

```python
A = torch.arange(12,dtype=torch.float32),reshape(2,3,2)
A, A.sum()
```

默认情况下，调用求和函数会沿所有的轴降低张量的维度，使它变为一个标量。还可以指定张量沿哪一个轴来通过求和降低维度。

```python
A.sum(axis=1),A.sum(axis=[0,1])#对轴0和轴1的所有元素进行求和
```

另外一个是求平均值

```python
A.mean() # A.sum() / A.numel()
```



### 2.3.3 非降维求和

调用函数来计算总和或均值时保持轴数不变

```python
B = torch.arange(6).reshape(2,3)
B,B.sum(axis=1,keepdims=True)
```

### 2.3.4 **点积**

```python
torch.dot(x,y)
```

### 2.3.5 **矩阵-向量积**

```python
# A为nxn矩阵 x 为n维向量 结果是n维向量
torch.mv(A, x)
```

### 2.3.6 矩阵-矩阵乘法

矩阵乘法：C=AB，A中行乘上B中列作为C中对应行列的元素的值

![image-20220511230540902](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20220511230540902.png)

```python
torch.mm(A,B)
```

### 2.3.7 范数

向量的范数表示一个一个向量有多大

欧几里得距离就是一个L2范数

```python
torch.norm(u)
```

## 2.4 微积分

在深度学习中，将拟合模型的任务分解为两个关键的问题：

1. 优化(optimization)：用模型拟合观测数据的过程；
2. 泛化(generalization)：生成出有效性超出用于训练的数据集本身的模型

深度学习通常选择对于模型参数可微的损失函数



**偏导数**：将微分的思想从一元函数推广到多元函数，偏导数是指y关于某个参数xi的偏导

**梯度**：连结一个多元函数对所有变量的偏导数，以得到该函数的梯度（gradient）向量。![image-20220512144456218](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20220512144456218.png)



## 2.5 自动微分

深度学习框架通过自动计算导数，即自动微分（automatic differentiation）。实际中，根据我们设计的模型，系统会构建一个*计算图*（computational graph）， 来跟踪计算是哪些数据通过哪些操作组合起来产生输出。

在我们计算y关于x的梯度之前，我们需要一个地方来存储梯度。 重要的是，我们不会在每次对一个参数求导时都分配新的内存。 因为我们经常会成千上万次地更新相同的参数，每次都分配新的内存可能很快就会将内存耗尽。 注意，一个**标量函数**关于**向量x的梯度**是向量，并且**与x具有相同的形状。**



### 2.5.1 一个简单的例子

作为一个演示例子，假设我们想对函数y=2x⊤x关于列向量x求导。 首先，我们创建变量`x`并为其分配一个初始值。

```python
import torch

x = torch.arange(4.0)
x
=>tensor([0., 1., 2., 3.])
```

在我们计算y关于x的梯度之前，我们需要一个地方来存储梯度。 重要的是，我们不会在每次对一个参数求导时都分配新的内存。 因为我们经常会成千上万次地更新相同的参数，每次都分配新的内存可能很快就会将内存耗尽。

```python
x.requires_grad_(True)  # 等价于x=torch.arange(4.0,requires_grad=True)
x.grad  # 默认值是None
```

计算y并计算梯度

```python
y = 2 * torch.dot(x, x)
y
=>tensor(28., grad_fn=<MulBackward0>)

y.backward()
x.grad
=>tensor([ 0.,  4.,  8., 12.])
```

模型情况下,pytorch会累积梯度，因此每次迭代需要清楚之前的值

```python
x.grad.zero_()
y = x.sum()
y.backward()
x.grad
```

### 2.5.2 非表现向量的反向传播

当`y`不是标量时，向量`y`关于向量`x`的导数的最自然解释是一个矩阵。 对于高阶和高维的`y`和`x`，求导的结果可以是一个高阶张量。

然而，虽然这些更奇特的对象确实出现在高级机器学习中（包括深度学习中）， 但当我们调用向量的反向计算时，我们通常会试图计算一批训练样本中每个组成部分的损失函数的导数。 这里，我们的目的不是计算微分矩阵，而是单独计算批量中每个样本的偏导数之和。

```python
x.grad.zero_()
y = x * x
# 等价于y.backward(torch.ones(len(x)))
y.sum().backward()
x.grad
```



## 2.6 概率

在统计学中，我们把从**概率分布**中**抽取样本**的过程称为***抽样*（sampling）**。 笼统来说，可以把***分布*（distribution）**看作是对事件的概率分配， 稍后我们将给出的更正式定义。 将概率分配给一些离散选择的分布称为*多项分布*（multinomial distribution）。

为了抽取一个样本，即掷骰子，我们只需传入一个概率向量。 输出是另一个相同长度的向量：它在索引i处的值是采样结果中i出现的次数。

```python
import torch
from torch.distributions import multinomial
from d2l import torch as d2l

fair_probs = torch.ones([6]) / 6
multinomial.Multinomial(1, fair_probs).sample()
=>tensor([0., 0., 0., 0., 0., 1.])
```

我们也可以看到这些概率如何随着时间的推移收敛到真实概率。 让我们进行500组实验，每组抽取10个样本。

```python
counts = multinomial.Multinomial(10, fair_probs).sample((500,))
#cumsum(dim=0) 对tensor沿着第0维进行累加，不改变tensor的形状
cum_counts = counts.cumsum(dim=0)
estimates = cum_counts / cum_counts.sum(dim=1, keepdims=True)
```

**概率论公理**

在处理骰子掷出时，我们将集合S={1,2,3,4,5,6} 称为***样本空间*（sample space）**或*结果空间*（outcome space）， 其中每个元素都是***结果*（outcome）**。 ***事件*（event）**是一组给定样本空间的随机结果。 例如，“看到5”（{5}）和“看到奇数”（{1,3,5}）都是掷出骰子的有效事件。 注意，如果一个随机实验的结果在A中，则事件A已经发生。 也就是说，如果投掷出3点，因为3∈{1,3,5}，我们可以说，“看到奇数”的事件发生了。

***概率*（probability）**可以被认为是将集合映射到真实值的函数。 在给定的样本空间S中，事件A的概率， 表示为P(A)，满足以下属性：



**随机变量**

在我们掷骰子的随机实验中，我们引入了*随机变量*（random variable）的概念。 随机变量几乎可以是任何数量，并且它可以在随机实验的一组可能性中取一个值。 考虑一个随机变量X，其值在掷骰子的样本空间S={1,2,3,4,5,6}中。 我们可以将事件“看到一个5”表示为{X=5}或X=5， 其概率表示为P({X=5})或P(X=5)。 通过P(X=a)，我们区分了随机变量X和X可以采取的值（例如a）。 然而，这可能会导致繁琐的表示。 为了简化符号，一方面，我们可以将P(X)表示为随机变量X上的*分布*（distribution）： 分布告诉我们X获得某一值的概率。 另一方面，我们可以简单用P(a)表示随机变量取值a的概率。 由于概率论中的事件是来自样本空间的一组结果，因此我们可以为随机变量指定值的可取范围。 例如，P(1≤X≤3)表示事件{1≤X≤3}， 即{X=1,2,or,3}的概率。 等价地，P(1≤X≤3)表示随机变量X从{1,2,3}中取值的概率。

请注意，*离散*（discrete）随机变量（如骰子的每一面） 和*连续*（continuous）随机变量（如人的体重和身高）之间存在微妙的区别。 现实生活中，测量两个人是否具有完全相同的身高没有太大意义。 如果我们进行足够精确的测量，你会发现这个星球上没有两个人具有完全相同的身高。 在这种情况下，**询问某人的身高是否落入给定的区间**，比如是否在1.79米和1.81米之间更有意义。 在这些情况下，我们将这个看到某个数值的可能性量化为*密度*（density）。 高度恰好为1.80米的概率为0，但密度不是0。 在任何两个不同高度之间的区间，我们都有非零的概率。



## 2.7 查阅文档

为了知道模块中可以调用哪些**函数和类**，我们调用`dir`函数。 例如，我们可以查询随机数生成模块中的所有属性：

```python
import torch

print(dir(torch.distributions))
```

忽略以“`__`”（双下划线）开始和结束的函数（它们是Python中的特殊对象）， 或以单个“`_`”（单下划线）开始的函数（它们通常是内部函数）。

有关如何使用给定函数或类的更具体说明，我们可以调用`help`函数。 例如，我们来查看张量`ones`函数的用法。

```python
help(torch.ones) #或者用torch.ones?
# torch.ones??显示代码
```

# 3.线性神经网络

## 3.1 线性回归

经典统计学习技术中的线性回归和softmax回归可以视为线性神经网络

偏置（bias）用来表示当所有的特征值都取0时，预测值应该为多少

在我们开始考虑如何用模型*拟合*（fit）数据之前，我们需要确定一个拟合程度的度量。 *损失函数*（loss function）能够量化目标的*实际*值与*预测*值之间的差距。 通常我们会选择非负数作为损失，且数值越小表示损失越小，完美预测时的损失为0。 回归问题中最常用的损失函数是平方误差函数。 当样本i的预测值为y^(i)，其相应的真实标签为y(i)时， 平方误差可以定义为以下公式：

## 3.2 从零开始实现线性回归



### 3.2.1 生成数据集

我们使用线性模型参数w=[2,−3.4]⊤、b=4.2 和服从正态分布的噪声项ϵ生成数据集及其标签。

```python
%matplotlib inline
import random
import torch
from d2l import torch as d2l

#生成人工数据集
def synthetic_data(w, b, num_examples):  #@save
    """生成y=Xw+b+噪声"""
    X = torch.normal(0, 1, (num_examples, len(w)))
    y = torch.matmul(X, w) + b
    y += torch.normal(0, 0.01, y.shape)
    return X, y.reshape((-1, 1))

true_w = torch.tensor([2, -3.4])
true_b = 4.2
features, labels = synthetic_data(true_w, true_b, 1000)
print('features:', features[0],'\nlabel:', labels[0])
=>features: tensor([-1.6092, -0.5917])
label: tensor([2.9942])
#查看第二个特征和label之间的散点图
d2l.set_figsize()
d2l.plt.scatter(features[:, (1)].detach().numpy(), labels.detach().numpy(), 1);
```



### 3.2.2 读取数据集

训练模型时要对数据集进行遍历，每次抽取一小批量样本，并使用它们来更新我们的模型。 由于这个过程是训练机器学习算法的基础，所以有必要定义一个函数， 该函数能打乱数据集中的样本并以小批量方式获取数据。

在下面的代码中，我们定义一个`data_iter`函数， 该函数接收批量大小、特征矩阵和标签向量作为输入，生成大小为`batch_size`的小批量。 每个小批量包含一组特征和标签。

```python
def data_iter(batch_size, features, labels):
    num_examples = len(features)
    indices = list(range(num_examples))
    # 这些样本是随机读取的，没有特定的顺序
    random.shuffle(indices)
    for i in range(0, num_examples, batch_size):
        batch_indices = torch.tensor(
            indices[i: min(i + batch_size, num_examples)])
        yield features[batch_indices], labels[batch_indices]

batch_size = 10
for X, y in data_iter(batch_size, features, labels):
    print(X, '\n', y)
    break
```



当我们运行迭代时，我们会连续地获得不同的小批量，直至遍历完整个数据集。 上面实现的迭代对于教学来说很好，但它的执行效率很低，可能会在实际问题上陷入麻烦。 例如，它要求我们将所有数据加载到内存中，并执行大量的随机内存访问。 在深度学习框架中实现的内置迭代器效率要高得多， 它可以处理存储在文件中的数据和数据流提供的数据。



### 3.2.3 定义模型相关及初始化

```python
#初始化参数
w = torch.normal(0, 0.01, size=(2,1), requires_grad=True)
b = torch.zeros(1, requires_grad=True)


#定义模型
def linreg(X, w, b):  #@save
    """线性回归模型"""
    return np.dot(X, w) + b

#定义损失函数
def squared_loss(y_hat, y):  #@save
    """均方损失"""
    return (y_hat - y.reshape(y_hat.shape)) ** 2 / 2

#定义优化算法
def sgd(params, lr, batch_size):  #@save
    """小批量随机梯度下降"""
    with torch.no_grad():
        for param in params:
            param -= lr * param.grad / batch_size
            param.grad.zero_()


#训练
lr = 0.03
num_epochs = 3
net = linreg
loss = squared_loss

for epoch in range(num_epochs):
    for X, y in data_iter(batch_size, features, labels):
        l = loss(net(X, w, b), y)  # X和y的小批量损失
        # 因为l形状是(batch_size,1)，而不是一个标量。l中的所有元素被加到一起，
        # 并以此计算关于[w,b]的梯度
        l.sum().backward()
        sgd([w, b], lr, batch_size)  # 使用参数的梯度更新参数
    with torch.no_grad():
        train_l = loss(net(features, w, b), labels)
        print(f'epoch {epoch + 1}, loss {float(train_l.mean()):f}')
```



## 3.3 线性回归的简介实现

在3.2中，我们只运用了： （1）通过张量来进行数据存储和线性代数； （2）通过自动微分来计算梯度。 实际上，由于数据迭代器、损失函数、优化器和神经网络层很常用， 现代深度学习库也为我们实现了这些组件。

```python
import numpy as np
import torch
from torch.utils import data
from d2l import torch as d2l

true_w = torch.tensor([2, -3.4])
true_b = 4.2
features, labels = d2l.synthetic_data(true_w, true_b, 1000)

#读取数据集
def load_array(data_arrays,batch_size,is_train=True):
    """构造一个PyTorch数据迭代器"""
    dataset = data.TensorDataset(*data_arrays)
    return data.DataLoader(dataset, batch_size, shuffle=is_train)

batch_size = 10
data_iter = load_array((features,labels),batch_size)

# 定义模型
from torch import nn
net = nn.Sequential(nn.Linear(2,1))


#初始化参数,其实已经默认初始化好了
net[0].weight.data.normal_(0,0.01)
net[0].bias.data.fill_(0)

#定义损失函数,平方L2范数
loss = nn.MSELoss()

#定义优化算法
trainer = torch.optim.SGD(net.parameters(),lr=0.03)


#训练
nums_epochs = 10
for epoch in range(nums_epochs):
    for X,y in data_iter:
        #前向传播
        l = loss(net(X),y)
        trainer.zero_grad()
        #反向传播来计算梯度
        l.backward()
        #通过优化器来更新模型
        trainer.step() 
    #计算每一个迭代周期的损失
    l = loss(net(features),labels)
    print(f'epoch{epoch+1},loss {l}')
    #作业:如何访问线性回归的梯度
    with torch.no_grad():
        print(net[0].weight.grad)
```



## 3.4 softmax回归

通常，机器学习实践者用*分类*这个词来描述两个有微妙差别的问题： 1. 我们只对样本的“硬性”类别感兴趣，即属于哪个类别； 2. 我们希望得到“软性”类别，即得到属于每个类别的概率。 这两者的界限往往很模糊。其中的一个原因是：即使我们只关心硬类别，我们仍然使用软类别的模型。

## 3.5 softmax实现



# 4. 多层感知机

## 4.1 多层感知机

## 4.2 多层感知机的简洁实现

## 4.3 模型选择、拟合和过拟合

模型的目标是发现模式（pattern）。将模型在训练数据上拟合的比在潜在分布中更接近的现象称为*过拟合*（overfitting）， 用于对抗过拟合的技术称为*正则化*（regularization）。

影响模型泛化的因素：

1. 可调整参数的数量。当可调整参数的数量（有时称为*自由度*）很大时，模型往往更容易过拟合。
2. 参数采用的值。当权重的取值范围较大时，模型可能更容易过拟合。
3. 训练样本的数量。即使你的模型很简单，也很容易过拟合只包含一两个样本的数据集。而过拟合一个有数百万个样本的数据集则需要一个极其灵活的模型。

### 4.4.2 模型选择

**使用验证集：**在实际应用中，情况变得更加复杂。 虽然理想情况下我们只会使用测试数据一次， 以评估最好的模型或比较一些模型效果，但现实是测试数据很少在使用一次后被丢弃。 我们很少能有充足的数据来对每一轮实验采用全新测试集。

解决此问题的常见做法是将我们的数据分成三份， 除了训练和测试数据集之外，还增加一个*验证数据集*（validation dataset）， 也叫*验证集*（validation set）。

**K折交叉验证：**当**训练数据稀缺**时，我们甚至可能无法提供足够的数据来构成一个合适的验证集。 这个问题的一个流行的解决方案是采用K*折交叉验证*。 这里，**原始训练数据**被分成K个不重叠的子集。 然后执行K次模型训练和验证，每次在K−1个子集上进行训练， 并在剩余的一个子集（在该轮中没有用于训练的子集）上进行验证。 最后，通过对K次实验的结果取平均来估计训练和验证误差。

### 4.4.3 模型欠拟合还是过拟合

是否过拟合或欠拟合可能取决于**模型复杂性**和**可用训练数据集的大小**

**模型复杂性**

高阶多项式函数比低阶多项式函数复杂得多。 高阶多项式的参数较多，模型函数的选择范围较广。 因此在固定训练数据集的情况下， 高阶多项式函数相对于低阶多项式的训练误差应该始终更低（最坏也是相等）。

**数据集大小**

另一个重要因素是数据集的大小。 训练数据集中的样本越少，我们就越有可能（且更严重地）过拟合。 随着训练数据量的增加，泛化误差通常会减小。



## 4.5 权重衰减



## 4.6 暂退法（Dropout）



## 4.7 数值稳定和模型初始化

**选择哪个激活函数**以及**如何初始化参数**可以决定优化算法**收敛的速度**有多快，不适合的选择会导致梯度爆炸或梯度消失的问题

解决或减轻上述问题的一种方法是进行参数初始化

+ 默认初始化
+ Xavier初始化



# 5.深度学习计算

在本章中，我们将深入探索深度学习计算的关键组件， 即模型构建、参数访问与初始化、设计自定义层和块、将模型读写到磁盘， 以及利用GPU实现显著的加速。 

## 5.1 层和块



### 5.1.1 自定义块

每个块必须提供的基本功能：

1. 将输入数据作为其前向传播函数的参数。
2. 通过前向传播函数来生成输出。请注意，输出的形状可能与输入的形状不同。例如，我们上面模型中的第一个全连接的层接收一个20维的输入，但是返回一个维度为256的输出。
3. 计算其输出关于输入的梯度，可通过其反向传播函数进行访问。通常这是自动发生的。
4. 存储和访问前向传播计算所需的参数。
5. 根据需要初始化模型参数。

即输入的数据、传播函数、前向传播过程中用到的辅助参数、初始化的模型参数



## 5.2 参数管理

参数管理主要包括以下内容：

- 访问参数，用于调试、诊断和可视化。
- 参数初始化。
- 在不同模型组件间共享参数。

```python
net[2].state_dict()
net[2].weight
net[2].bias
```

# 6.卷积神经网络

## 6.1 从全连接到卷积

多层感知机十分适合处理表格数据，其中行对应样本，列对应特征。然而对于高维感知数据，这种缺少结构的网络可能会变得不实用。

*卷积神经网络*（convolutional neural networks，CNN）是机器学习利用自然图像中一些已知结构的创造性方法。

适合于计算机视觉的神经网络架构：

1. *平移不变性*（translation invariance）：不管检测对象出现在图像中的哪个位置，神经网络的前面几层应该对相同的图像区域具有相似的反应，即为“平移不变性”。
2. *局部性*（locality）：神经网络的前面几层应该只探索输入图像中的局部区域，而不过度在意图像中相隔较远区域的关系，这就是“局部性”原则。最终，可以聚合这些局部特征，以在整个图像级别进行预测。

卷积层对输入和卷积核权重进行互相关运算，并在添加标量偏置之后产生输出。 所以，卷积层中的两个被训练的参数是卷积核权重和标量偏置。 就像我们之前随机初始化全连接层一样，在训练基于卷积层的模型时，我们也随机初始化**卷积核**权重。



## 6.3 填充和步幅

**填充：**解决经过多个卷积层后，原始图像丢失很多有用信息的问题，一般填充0。

卷积神经网络中卷积核的高度和宽度通常为奇数，例如1、3、5或7。

**步幅：**大幅降低图像的宽度和高度。解决原始图像输入分辨率过于冗余的问题。

在计算互相关时，卷积窗口从输入张量的左上角开始，向下、向右滑动。 在前面的例子中，我们默认每次滑动一个元素。 但是，有时候为了高效计算或是缩减采样次数，卷积窗口可以跳过中间位置，每次滑动多个元素。

**在实践中，我们很少使用不一致的步幅或填充**
