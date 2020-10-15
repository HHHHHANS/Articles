# BBN Structure 双线网络结构
[ Bilateral-Branch Network with Cumulative Learning for Long-Tailed Visual Recognition]()
[Github源码]( https://github.com/Megvii-Nanjing/BBN)
![总体网络结构][pic3]

__两条线各自对不同采样方式的样本进行特征学习和分类学习，最后汇总到累积学习结构中__

>&emsp;_Concretely, we design two branches for representation learning and classifier learning, termed “conventional learning branch” and “re-balancing branch”,respectively. Both branches use the same residual network structure and share all the weights except for the last residual block. 
>&emsp;Furthermore, we also design a specific cumulative learning strategy for shifting the learning “attention” between two branches in the training phase._




## 双边网络层
### 数据采样-Data samplers
+ #### conventional learning branch
对所有样本数据进行均匀采样，得出一个样本
+ #### re-balancing branch
1. 计算各类别的数据采样概率
2. 通过采样概率，随机选定某类样本
3. 通过选定的某类（标签），均匀采样出一个样本

> ![类别概率][pic1]
>
> 
>
> ![类别权重][pic2]



​                             									**Ni为某类（标签）的样本数**
​														**Nmax为规模最大的类（标签）的样本数**

两条线每次进入的x为一个常规采样和一个re-sampling的一个样本，
多次反复该步骤形成了mini-batch。

### 权重共享-Weight sharing
双线的网络结构除了最后一个residual block，其余都是使用相同的网络结构和权重（例如ResNet-32和ResNet-50）
>&emsp; _On the one hand, the well-learned representation by the conventional learning branch can benefit the learning of the re-balancing branch. On the other hand, sharing weights will largely reduce computational complexity in the inference phase._

分析：先使用conventional learning对常规采样样本进行前向传播，得出常规特征向量fc，使用相同的网络对re-balancing learning的re-sampling样本进行前向传播。
### GAP
> *Global average pooling*

## 积学习层-Cumulative learning

>![汇总权重][pic4]

&emsp; 在积学习层，设置一个会动态调整大小的α参数，表示不同epoch时候对两边的特征向量分配的权重大小，即分配不同的注意力。**因为α是递减的，因此模型的整体训练重心会从head-info转向tail-info。**

## 推理阶段
  训练完成后的在推理阶段，**α会被设置成0.5**，表示在此阶段，常规线与Re线的特征矩阵应该分配相同的注意力，得到两个预测logits，最后通过矩阵元素逐个相加得到分类预测结果。



[pic1]:https://github.com/HHHHHANS/Articles/blob/main/Resource/BNN-P%20of%20classes.png?raw=true	"类别概率"
[pic2]:https://github.com/HHHHHANS/Articles/blob/main/Resource/BNN-w%20of%20classes.png?raw=true	"类别权重"
[pic3]:https://github.com/HHHHHANS/Articles/blob/main/Resource/BNN-structure.png?raw=Ture	"总体网络结构"
[pic4]:https://github.com/HHHHHANS/Articles/blob/main/Resource/BNN-%CE%B1%20of%20branch.png?raw=true	"汇总权重"