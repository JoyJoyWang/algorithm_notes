# algorithm_notes
This is used to do learning notes and summaries for algorithm and data structure.
# Algorithm Design 基本算法的证明过程

[TOC]

## Proof: EFT in interval scheduling is optimal

**证明EFT算法在Interval Scheduling中是最优的**

- 先证明一个引理，eft贪心算法结果中的第n个事件的结束时间一定不晚于最优算法结果中第n个时间的结束时间。
- 引理用归纳法容易证明，因为如果第n-1个结束时间不晚于的话，下一个事件的选择中，eft算法就选择了最早完成的。
- 在引理基础上再用归纳法（induction）证明eft算法能实现的任务数量是最多的，可用反证法（contradiction）：
- 有两个集合A,B分别是eft选择的事件和最优结果事件，那假设A里面结果的数量i小于B集合中事件数量j。由引理知，Ai完成的时间不晚于Bi完成的时间，所以Bi+1实际上也是可以加入到A集合中的。由此可知A得到的是最优结果。

## Minimum Spanning Tree

证明**Prim 算法**和**Kruskal 算法**在加权图中可以找到最小生成树

- 假设任一算法都生成一棵树 T。设存在另一棵总权重较小的生成树 S。令 e 为位于 T 但不在 S 中的最小权重边。
- 如果我们将 e 添加到 S，我们就会从树的等效定义中获得一个循环。
- 该环包含一条边 e'，该边在 S 中但不在 T 中，否则 T 不会是树。
- 因此，我们用 T 中的 e 替换 S 中的 e'，并获得新的生成树 S'。
- 从 T 的构造方法可知，e 的权重不能超过 e' 的权重。
- 因此，S' 的总权重不会超过 T 的总权重。而且，S' 比 S 多一条与 T 相同的边。我们重复上述过程，反复将S的边换成T的边，每次中间图的权重不超过T的权重，因此T的权重不超过S的权重，与S的定义矛盾.因此T一定是最小生成树。

## Proof: Huffman Codes is optimal

**证明哈夫曼树是最优的**

- **引理1**：给定*W* = {*w*1, *w*2, *w*3...,*wn*} (*n* >= 2), 以此集合构建相应的哈夫曼树。
- 令wi, wj 是W中权重最小的两个元素。则这两个数相应的结点是兄弟结点，且这两结点在二叉树中的深度不小于其他不论什么一个叶结点的深度。
- **using induction 归纳法证明:**
- 现在有w1,w2,…wn是各个节点，w1是出现频率最小的，wn是出现频率最大的
- base case：当n=2的时候，必然是一个最小二叉树，是最优的（就是有最小的外部路径长度）
- assume：假设有n-1个节点{w2，w3，…,wn}，用huffman codes构造出来的树是最优的。
- then， 现在有一个n个节点（n>=2）的哈夫曼树T，最小权重的两个节点是w1，w2，他们是一对兄弟节点同时有父节点V。由哈夫曼算法，w1和w2肯定是最深的节点之一。于是我们构造一个新的哈夫曼树T‘{w1+w2,w3,w4,….wn}是一个n-1个节点的树，有假设可知这个T‘是最优的。那么把w1和w2接到T’上，根据引理这两个节点选了最深的位置，那么接完这两个节点的树也是最优的。

## MRP(multi-level regression with poststratification)

MRP是什么算法？

多层线性回归在这篇文章中讲得很清楚：https://zhuanlan.zhihu.com/p/150878441

后分层是什么？wiki百科中：

> The **poststratification** refers to the process of adjusting the estimates, essentially a weighted average of estimates from all possible combinations of attributes (in this example age and sex, though there were more). Each combination is sometimes called a "cell." The **multilevel regression** is used to smooth noisy estimates in the cells with too little data by using overall or nearby averages

后分层就是根据已经被调查的受众数据进行调整，刻意增加某些群体的数量等，让结果更具有代表性。
