# Project Experience



## 闲置虚拟机预测
#### 项目状况
目标：基于虚拟机的指标数据的建模并分类。
建模样本数量：200—>600台
测试样本数量：5k台
随机森林分类使用默认参数
分类精确率在90%
召回率在95%（目标在于提高召回率）

#### 遇到的困难

+ tsFresh库内存泄漏问题：
	+ 数据清洗阶段发现电脑内存释放不及时的问题：线性增长；
	抽取出单次任务的代码放进单独进程内，进行time.sleep，强制释放内存。
		
	+ 使用内存监测插件，找出内存泄漏模块、泄漏代码：构建序列特征时候某些特征含有的子特征过大，优化不好，重新评估特征重要性，跳过无用特征构建，问题解决。



## 案例库匹配

#### 遇到的困难
+ demo期间使用不同的序列化框架：
	`eval()`最方便，但较为轻量，单次使用空间有限。&不安全 -> `ast.eval()`保证安全
	`pickle`性能中等，单次使用空间中等。&不安全
	`joblib`性能最差，单次使用空间最大。
	`json`限制多。
	`yaml.safe_load`安全性最好
+ 自学习模块的维护，考量用户输入可以作为专家知识。
+ 维护特征表的特征顺序
+ 维护指纹库中每条案例所包含的信息构建
+ 匹配算法的选择
+ demo期间选用one-hot编码，特征矩阵产生了明显稀疏性的后续选定编码方式以及在扩展特征后优化编码方式。
+ jieba分词框架在pyinstaller执行打包后，字典文件无法打进去。
	[解决方法](https://blog.csdn.net/sinat_34200786/article/details/79715236)
+ 


## AIOps 托管云告警数据智能关联
#### 项目状况
+ 对zabbix告警数据一个月产生10w+告警数据进行压缩，同源关联分析（地理位置，所处主机集群等）
+ 后续需要对夜莺nightingale数据进行分析。

#### 遇到的困难
+ 数据无标签，难以使用有监督学习的算法
+ 转而考虑使用无监督学习，告警内特征/可构建的特征少，且业务规则抽象，没有一套统一可供参考的标准。


## Elasticsearch 与head插件
[1](https://blog.csdn.net/lh2420124680/article/details/74277380)
[支持win7的nodejs版本](https://nodejs.org/dist/v12.16.2/)
[windows-es环境搭建](https://blog.csdn.net/u014236259/article/details/53696133)

+ 安装ik分词版本需要跟ES版本对齐
+ 安装IK分词的目录不能有空格或中文—java.io.filepermission—acess denied



## Tkinter
[1](https://www.cnblogs.com/lili414/p/8954798.html)
[滚动条添加方式](https://blog.csdn.net/qq_28123095/article/details/79331756)
[tkinter窗口视窗教程大集合](https://blog.csdn.net/ahilll/article/details/81531587)
[字体格式汇总](https://blog.csdn.net/oh5w6hinug43jvrhhb/article/details/106726048)
[各类弹出框](https://blog.csdn.net/sinat_39369871/article/details/107184453)
[radiobutton汇总](https://blog.csdn.net/qq_38970783/article/details/88803931)

## python打包成exe
[1](https://www.cnblogs.com/mini-monkey/p/11195309.html)
[2](https://blog.csdn.net/CholenMine/article/details/80964272)

