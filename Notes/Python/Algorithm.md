# 算法
## 编辑距离算法—比较两个字符串的相似度
[jianshu](https://www.jianshu.com/p/46ff18e8d636)
动态规划
算法解析
相似度计算

##  各类距离公式
+ https://www.jb51.net/article/164673.htm
+ https://www.cnblogs.com/liujinhong/p/6001997.html

## BM25 tf-idf的优化
[公式详解](https://zhuanlan.zhihu.com/p/113224707)
[BM25F改进](https://zhuanlan.zhihu.com/p/31009310)
增加三个协调因子，并考虑查询句长度、目标词词频和文档的长度、平均长度的关系。



## [Kruskal—基于并查集的贪心算法—最小生成树](https://blog.csdn.net/luomingjun12315/article/details/47700237)

## prim—另一种最小生成树算法

## [并查集](https://blog.csdn.net/luomingjun12315/article/details/47373345)
需要实现的三个接口：
+ 建立新的集合

+ 将两个不相交集合进行合并，相交则不合并

+ 找到每个节点的代表节点，也可以判断两个节点是不是在一个集合内。
  关键概念：节点的代表节点（即该节点的父节点），集合的代表节点（即该集合的根节点）

## [Dijkstra—正权图单源最短路径](https://www.zhihu.com/question/20630094/answer/758191548)
+ 特点：使用了广度优先搜索解决正值赋权有向图或者无向图的单源最短路径问题，算法最终得到一个最短路径树。该算法常用于路由算法或者作为其他图算法的一个子模块，但是路径中有负权值的时候失效。
+ 一次算法的执行维护一个数组，表示从一个起点到剩余各点的最短距离
