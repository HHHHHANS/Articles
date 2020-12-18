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
	[Dijkstra算法](https://www.zhihu.com/question/20630094/answer/758191548)
	维护一个集合，每次遍历找出距离集合中各节点距离最短的下一个节点，更新距离