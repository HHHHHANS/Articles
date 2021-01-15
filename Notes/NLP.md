# NLP
[参考来源](https://www.jianshu.com/p/80436483b13b)
## Seq2Seq
+ 输入，encoder， C， decoder， 输出
+ 1 * N， N * N， N * 1
+ encoder一般都相同，三种不同的decoder
## Attention
在Seq2Seq模型中，encoder总是将源句子的所有信息编码到一个固定长度的上下文向量c中，然后在decoder解码过程中向量c总是不变，存在以下缺陷：
+ 对于长句子，很那用一个人定长的向量c完全表示其意义
+ RNN存在长序列梯度消失的问题，只是用最后一个神经元得到的向量c效果不理想
+ 无法模仿人类的注意力方式，人在阅读的时候，会把注意力放在当前的句子上

attention是一种将模型的注意力放在当前翻译单词上的一种机制，
例如翻译”I have a cat“， 翻译到“我”的时候，将注意力放在源句的“I”上。

使用了attention后，decoder的输入就不是固定的上下文向量c了，会根据当前翻译的信息计算当前的c