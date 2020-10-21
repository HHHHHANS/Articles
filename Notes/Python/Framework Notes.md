## 使用pandas对dataframe进行有放回抽样						    		并返回被抽样的样本的序列
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

