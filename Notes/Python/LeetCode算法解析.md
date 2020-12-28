# LeetCode算法解析
## 图
+ [克隆图](https://leetcode-cn.com/problems/clone-graph/solution/dfs-he-bfs-by-powcai/)
	DFS或者BFS，创建哈希表存储已经复制过的节点。
+ [二维路径矩阵中找到体力消耗最小的路径](https://leetcode-cn.com/problems/path-with-minimum-effort/solution/dfs-er-fen-cha-zhao-mo-ban-ti-by-kobe24o/)
	转化为，在限制体力的情况下能不能找到一条符合条件的路径。
	二分法查找合适路径，DFS遍历矩阵
+ [求能构成最小高度树的节点](https://leetcode-cn.com/problems/minimum-height-trees/)
	逐步对出入度为1的节点进行删除，队列存储待删除节点
+ [判断二分图](https://leetcode-cn.com/problems/is-graph-bipartite/solution/pan-duan-er-fen-tu-by-leetcode-solution/)
	状态标记+DFS/BFS
+ [机票行程排序](https://leetcode-cn.com/problems/reconstruct-itinerary/solution/zhong-xin-an-pai-xing-cheng-by-leetcode-solution/)
	欧拉图/半欧拉图的欧拉回路/欧拉通路遍历
	[Hierholzer算法](https://zhuanlan.zhihu.com/p/108411618)

+ [求概率最大的路径](https://leetcode-cn.com/problems/path-with-maximum-probability/solution/gai-lu-zui-da-de-lu-jing-by-leetcode-solution/)
	[Dijkstra算法—算最短连通路径](https://www.zhihu.com/question/20630094/answer/758191548)
	求任意一个起点到剩余每个节点的最短路径
	维护一个集合，每次遍历找出距离集合中各节点距离最短的下一个节点，更新距离
	又在每次在剩下没遍历过的元素中找最小的元素，等于需要多一次的排序过程。

+ [构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/solution/liang-tang-bian-li-by-z1m/)
	左右遍历各构造数组，对应位置的数值乘积则为相应位置的结果。

+ [表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/mian-shi-ti-20-biao-shi-shu-zhi-de-zi-fu-chuan-y-2/)
	字符串中含有e，+/-， .等元素，使用有限自动状态机

+ [字符串转整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/solution/mian-shi-ti-67-ba-zi-fu-chuan-zhuan-huan-cheng-z-4/)
	边界处理 2的31次方：2147483647
	
+ [连接所有点的最小费用](https://leetcode-cn.com/problems/min-cost-to-connect-all-points/solution/primjian-duan-de-pythondai-ma-by-whiterobe/)
	[Prim—求最小生成树]()
	求待访问节点中距离现节点集合的最短边，并加入。
	[python优先队列]()`from queue import PriorityQueue`

+ [去除冗余连接得到无环图](https://leetcode-cn.com/problems/redundant-connection/solution/dfs-bfs-bing-cha-ji-xiang-xi-quan-mian-d-j050/)
	DFS/BFS/并查集
+ [连同整个网络的最小操作次数](https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/solution/lian-tong-wang-luo-de-cao-zuo-ci-shu-by-leetcode-s/)
	DFS/并查集
	反向考虑，不要丢弃看似多余的信息

+ [21点-获胜概率](https://leetcode-cn.com/problems/new-21-game/solution/huan-you-bi-zhe-geng-jian-dan-de-ti-jie-ma-tian-ge/) 
	逆向动态规划，已知最后一批状态，向前更新状态，得到问题的所在的状态。

+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
+ []()
