
# Feature Transfer Learning - 人脸识别中对尾部数据的特征迁移学习框架

Paper:[Feature Transfer Learning for Face Recognition with Under-Represented Data](https://github.com/HHHHHANS/Articles/blob/main/Resource/Paper/Long-Tail%20classification/%E8%BF%81%E7%A7%BB%E5%AD%A6%E4%B9%A0Feature%20Transfer%20Learning%20For%20Face%20Recognition%20with%20Under-Represented%20Data.pdf)

## 摘要分析：

+ 背景：使用代表性不足的数据（尾部数据）进行传统的神经网络训练会得出有偏差的分类器

+ 推出了基于中心的特征迁移框架，通过头部样本来增加尾部样本的特征空间

+ 假设所有样本的方差为高斯先验，并将头部样本的方差转移到尾部样本上，使得尾部样本的分布更接近头部样本

+ 使用该特征转移成功呈现出平滑的视觉插值，该插值可以进行解缠以保留类的标识，同时通过不带标识变化（例如姿势和光照）来扩大类的特征空间
	
	> _Moreover, our feature transfer successfully presents smooth visual interpolation, which conducts disentanglement to preserve identity of a class while augmenting its feature space with non identity variations such as pose and lighting._

## 涉及相关领域
+ **Imbalanced data classification**
+ **One-shot & low-shot learning: 单样本 & 少样本学习**
+ **Feature transfer learning**

## 解析
### 1. The Proposed Approach
+ 使用UR数据（尾部）进行训练的局限性
	![头部和尾部样本分布][pic1]
	+ classifier weight norms：在最后一层全连接层中，由于头部数据的样本量充足，在BP阶段有更多的机会被更新。常用对策：
		+ re-sampling 重采样
		+ weight normalization 权重标准化
		**但是这两个方法都无法从本质上解决UR样本数不足和缺乏多样性的问题。**
	+ intra-class variances：尽管使用了wight norm regularization， 尾部样本和头部样本的weight norm已经非常相似，但由于数量不足，类内方差intra-class variances相比于头部数据还是大很多。
### 2. The Proposed Framework
> ***Enlarging the intra-class variance for UR classes is the key to alleviate these imbalance issues.***

即需要增大尾部数据的类内variances，进而扩大了尾部数据的feature space。
![][pic2]
有很多框架使用的是新颖的losses和正则化来增加模型的泛化能力。
本文FTL框架使用了从头部数据的迁移学习来增加尾部数据的类内方差

![总体结构][pic3]
#### 总体结构解析
+ 输入 **x**
	![输入x][pic7]
+ 特征提取 **Enc** - **g**
	![编码式][pic5]
	**Enc**对输入的图片进行计算得到**g**：rich features space博特征空间
+ 解码并重建 **Enc**-**Dec**-**Reconstruction** 
	逐像素重建损失函数![重建损失][pic4]	其中**x'**为![解码式][pic6]
	>*The reconstruction loss allows g to contain diverse nonidentity variations such as pose, expression, and lighting. Therefore, we denote g as the rich feature space.* 
	>*该损失函数可以使**g**获得丰富的非标识性的变化分布例如姿势，表情和光线，因此称**g**为博特征空间*
+ 滤网器 **R** filtering network
	生成判别性标识特征**f**， 包括线性层\*1+去卷积层\*2+卷积层\*2+线性层\*1
	![滤网器][pic8]

+ FC层分类
	FC层损失函数 ![FC层损失函数][pic9]
	FC层权重![FC-weight][pic13]![FC-weights' domain][pic14]
	> *Note that softmax loss is scale-dependent where the loss can be made arbitrarily small by scaling the norm of the weights wj or features f.*
	>
	
	+ 可以单独对weight norm 或 features 进行正则化，或同时对两者进行标准化[refer1]
#### 训练流程
1. 给定一个训练数据集，包含头部样本和尾部样本，使用![总损失函数][pic12]并预先训练所有模块：Enc，Dec，R，FC
2. 开始在每个iteration中交替进行stage1和stage2
	+ stage1：Decision Boundary Reshape（依次对三个batch 进行训练）
		固定博特征提取器Enc和解码器Dec的权重，计算头样本的协方差矩阵**Cov-M**，**对头部样本batch训练**，再**对尾部样本batch进行训练**，将**Cov-M**应用到尾部数据中进行transfer操作，获得尾部样本的新特征空间，**对尾部样本的新特征向量训练**。这个过程目的是修正分类器的决策边界，即**让分类器学习尾部样本经过迁移的新特征向量**。
	+ stage2：Compact Feature Learning
		固定全连接FC层权重信息，在BP过程中直接更新R和Enc的权重，即**博特征提起器和精特征提取器识别imput中更加紧凑的特征**
[pic1]: "头部和尾部样本分布"
[pic2]:"扩大特征空间"
[pic3]: https://github.com/HHHHHANS/Articles/blob/main/Resource/Images/FTL-%20overview.png?raw=true "总体结构"
[pic4]:"重建损失"
[pic5]: "编码式"
[pic6]: "解码式"
[pic7]: "输入x"
[pic8]: "滤网器"
[pic9]: "fc损失"
[pic10]:"损失函数正则项"
[pic11]:"损失函数各项系数"
[pic12]:"总损失函数"
[pic13]:"FC-weights"
[pic14]: "FC-weights'domain"
[pic15]:"gik-of-flipped-imput-x"
[pic16]:"Q"
[pic17]:"V"
[pic18]:"algorithm"
[pic19]:"two-stages"
[pic20]:"ci"
[refer1]: F. Wang, X. Xiang, J. Cheng, and A. L. Yuille. Normface: l 2 hypersphere embedding for face verification. arXiv preprint arXiv:1704.06369, 2017.