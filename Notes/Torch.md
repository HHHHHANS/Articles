# Torch
## 创建Tensor
+ 从numpy导入
+ 从list导入
+ 注意```torch.tensor```接收现成数据，```.Tensor```接收维度&现成数据


## 数据增强 Data argumentation（图片样本）
+ GAN生成更多样本
+ Flip翻转：水平/上下
+ Rotate旋转：不同角度
+ Scale缩放
+ Crop Part裁剪
+ Noise增加噪声

## RNN
[Seq_len, feature_len]
+ 在CNN中把一个batch送入模型结构为[batch_num, word_num, word_vec]
+ RNN中是[word_num, batch_num, word_vec]
+ ```nn.RNN()```
	+ input_size: feature_num of input ***x***
	+ hidden_size: feature_num of hidden state ***h***
	+ num_layers

## 维度变换
#### ```view``` & ```reshape```
+ 功能接近一样
+ 各维度的转换
#### ```squeeze``` & ```unsqueeze```
+ 压缩维度 & 增加维度
+ ```unsqueeze```接受[-x-1，x]，表示插入后的tensor对应第x维是新的
+ ```squeeze```只会删除空维度（该维是1）
	+ 不接受参数，表示删除所有空维度
	+ 接收[-x, x-1]，表示删除现在的 第x 维（为空才删除）
#### ```expand``` & ```repeat```
+ ```expand```广播机制（推荐），接收预期各个维度数，只对原来维度为1的地方变换，对不想做处理的维度直接输入-1自动不做变换。
+ ```repeat```内存复制机制
#### ```transpose``` & ```t``` & ```permute```
+ ```transopose```单次，接收不同维度进行交换
+ ```permute```多次
+ ```t```2d tensor的转置
