算法框架
```python
def BFS(start: Node, target: Node):# 计算从起点start到终点target的最近距离
    q = Queue()# 队列
    visited = set()# 避免走回头路
    q.append(start)# 将起点加入队列
    step = 0
    while(not q.empty()):
        size = q.qsize()
        for i in range(size):# 将当前队列中的节点向四周扩散
            current = q.pop()
            if(current == target):# 判断是否到达终点
                return step
            for(x in current.adj()):# 将current的相邻节点加入队列
                if(x not in visited):
                    q.append(x)
                    visited.add(x)
        step += 1# 更新步数
```