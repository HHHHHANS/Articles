+ ### 使用pandas对dataframe进行有放回抽样，并返回被抽样的样本的序列
```python
def resample(df):

	# pd.dataframe.sample(n为需要抽取出来的数量；axis=0为抽取行；replace=Tru表示有放回抽样，无放回则为flase)
	new_df = data.sample(n=df.shape[0], axis=0, replace=True)
	selected_index = new_df.index
	
	# pd.dataframe.drop_duplicates(keep=true默认为保留重复项目中的一项，flase则不保留所有重复项)
	selected_index = selected_index.drop_duplicates()
	
	# pd.dataframe.isin()挑选符合条件的dataframe或series，加上~表示逆选择
	sampled = df.index.isin(selected_index)
	
	return new_df, sampled
```
[`np.dataframe.sample()`](https://www.cnblogs.com/webRobot/p/11484648.html)
[`np.dataframe.drop_duplicates()`](https://blog.csdn.net/Asher117/article/details/84454969)
[`np.dataframe.isin()`](https://blog.csdn.net/W_weiying/article/details/84618685)

+ ### pandas.dataframe已经将column名字作为属性获取该列数据
`df.列名`  效果等同于  `df["列名"]`



+ ### `numpy.transpose()` 改变序列的排列
[官方文档例子](https://numpy.org/doc/stable/reference/generated/numpy.transpose.html)
[解析](https://blog.csdn.net/u012762410/article/details/78912667)
不接收额外参数下，默认是转置操作
接受了一个list或者tuple作为新坐标，则转化为新坐标系的顺序。
```python
>>> x = np.arange(4).reshape((2,2))
>>>print(x)
array([[0, 1],
       [2, 3]])
```
```python
>>> new_x_1 = x.transpose()
>>> new_x_2 = np.transpose()
>>> print(new_x_1)
>>> print(new_x_2)
[[0 2]
 [1 3]]
[[0 2]
 [1 3]]
```


+ ### `matplotlib.pyplot` TSNE 数据降维

+ ### 直接对dataframe进行`if`判断报错以及python中`if`的深层理解
[解读](https://blog.csdn.net/qq_39314099/article/details/101691122)