## 维特比算法

维特比算法是用动态规划求概率最大路径的一种方法。根据动态规划原理，最优路径具有这样的特性：如果最优路径在时刻t通过节点$i_t^*$，那么该最优路径经过的节点$i_0^*$到节点$i_t^*$的路径值一定大于其他从节点$i_0^*$到节点$i_t^*$的路径值。根据这一原理，对于时刻t的每个节点，我们都要计算到该节点时的最大值，这样保证了到该节点都只保留了唯一的一条路径，从而实现了算法复杂度的降低。一个简单的说明可以[参照该例子](https://www.cnblogs.com/always-fight/p/12669298.html)。
