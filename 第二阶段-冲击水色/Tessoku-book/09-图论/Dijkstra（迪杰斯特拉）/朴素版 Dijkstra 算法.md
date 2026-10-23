[【算法动画】Dijkstra 为什么要用优先队列？从朴素扫描到最小堆实现_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1RjK26zEvy/?spm_id_from=333.1387.search.video_card.click&vd_source=79ca3da41a540ffd5bee721903882381)

### Code TLE
``` python
import heapq  
import sys  
import math  
from collections import deque  
  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    graph = [ [ ] for _ in range(n + 1)]  
    for _ in range(m):  
        a, b, c = map(int, sys.stdin.readline().strip().split())  
  
        graph[a].append((b, c))  
        graph[b].append((a, c))  
  
    dist = [math.inf] * (n + 1)  
    visited = [False] * (n + 1)  
  
    dist[1] = 0  
    for _ in range(n):  
        u = -1  
        min_d = math.inf  
        for i in range(1, n + 1):  
            if not visited[i] and dist[i] < min_d:  
                min_d = dist[i]  
                u = i  
  
        if u == -1:  
            break  
  
        visited[u] = True  
  
        for v, weight in graph[u]:  
            if not visited[v] and dist[u] + weight < dist[v]:  
                dist[v] = dist[u] + weight  
  
    outs = []  
    for i in range(1, n + 1):  
        if dist[i] == math.inf:  
            outs.append(-1)  
        else:  
            outs.append(dist[i])  
  
    sys.stdout.write('\n'.join(map(str, outs)))  
  
  
if __name__ == "__main__":  
    main()
```

这段代码实现的是**朴素版（原始版）的 Dijkstra 算法**，它的整体逻辑可以分为四个阶段：**建图与初始化**、**贪心选择最近点**、**松弛更新邻居**、**整理输出**。

我们一步步拆解它的内部逻辑：

---

### 第一阶段：建图与初始化

```python
    graph = [ [ ] for _ in range(n + 1)]
    for _ in range(m):
        a, b, c = map(int, sys.stdin.readline().strip().split())
        graph[a].append((b, c))
        graph[b].append((a, c))

    dist = [math.inf] * (n + 1)
    visited = [False] * (n + 1)
    dist[1] = 0
```

1. **邻接表建图**：
   * 因为顶点编号是 $1 \sim N$，所以开一个大小为 $N + 1$ 的列表 `graph`。
   * 题目是无向图，所以输入一条边 $(a, b, c)$，两端都要存：从 $a$ 能到 $b$ 代价为 $c$；从 $b$ 也能到 $a$ 代价为 $c$。
2. **状态数组初始化**：
   * `dist`：记录从起点 $1$ 到各个顶点的已知最短距离。除了起点自己距离为 `0`（`dist[1] = 0`），其余点一开始都是未知，设为正无穷 `math.inf`。
   * `visited`：标记数组。记录这个点的最短距离是否已经**彻底敲定（确定不可再更改）**，初始全为 `False`。

---

### 第二阶段：外层循环确定 $N$ 个点（核心主循环）

```python
    for _ in range(n):
```
* 图里一共有 $N$ 个顶点。Dijkstra 算法的规则是：**外层循环每执行一轮，就会在图中“锁定”一个顶点的最终最短距离**。
* 正常连通的情况下，循环跑 $N$ 轮，全图 $N$ 个点的最短距离就全部求出来了。

---

### 第三阶段：贪心选择——找未确定的最近点

```python
        u = -1
        min_d = math.inf
        for i in range(1, n + 1):
            if not visited[i] and dist[i] < min_d:
                min_d = dist[i]
                u = i

        if u == -1:
            break

        visited[u] = True
```

这是朴素解法最核心、也最耗时的一步：
1. **地毯式搜索最小值**：
   * 用一个 `for i in range(1, n + 1)` 遍历所有顶点。
   * 筛选条件是：`not visited[i]`（还没有被敲定）并且 `dist[i] < min_d`（离起点目前最近）。
   * 找到候选池中离起点最近的那个点，记为 `u`。
2. **提前退出检查（连通性判断）**：
   * 如果找了一圈 `u` 仍然是 `-1`，说明剩下的所有点距离都是无穷大（无法从起点到达）。此时图不连通，直接 `break` 提前结束循环。
3. **锁定点 `u`（贪心依据）**：
   * `visited[u] = True`：将点 $u$ 盖章确认。
   * **为什么敢直接敲定 $u$？** 因为图里所有的边权都是正数。$u$ 已经是当前所有未确定点里离起点最近的了，从其他点绕路再走到 $u$，总路程只会更长，绝不可能比当前的 `dist[u]` 更短。

---

### 第四阶段：松弛操作（Relaxation）——用水池 $u$ 更新周边邻居

```python
        for v, weight in graph[u]:
            if not visited[v] and dist[u] + weight < dist[v]:
                dist[v] = dist[u] + weight
```

点 $u$ 的距离彻底确定后，就可以用它作为一个“中转站”，去尝试缩短它周围邻居的距离：
* 遍历从 $u$ 出发能到达的每一个邻居点 $v$。
* 如果邻居 $v$ 还没被敲定（`not visited[v]`），问一个问题：
  > **“如果先从起点走到 $u$，再通过边 $(u, v)$ 走到 $v$（花费 `dist[u] + weight`），会不会比目前记录的走法 `dist[v]` 还要近？”**
* 如果确实更近，就更新记录：`dist[v] = dist[u] + weight`。

---

### 第五阶段：整理并输出答案

```python
    outs = []
    for i in range(1, n + 1):
        if dist[i] == math.inf:
            outs.append(-1)
        else:
            outs.append(dist[i])

    sys.stdout.write('\n'.join(map(str, outs)))
```
* 检查 $1 \sim N$ 各个点的最终距离：
  * 如果依然是初始的 `math.inf`，说明根本走不到，按题目要求转为 `-1`。
  * 否则输出算出的最短路 `dist[i]`。
* 用 `\n.join` 一次性快速输出。

---

### 逻辑总结图解

整个过程就像滚雪球：
1. **初始**：确定集合里只有 `{1}`，起点距离为 0。
2. **扩展**：用点 1 去计算它身边所有邻居的临时距离。
3. **晋升**：在所有临时距离里，挑出最小的那个点（比如点 2），将它加入确定集合。
4. **再扩展**：用点 2 去更新它的邻居。
5. **循环**：重复以上操作，每次将一个离得最近的点拉进“已确定圈子”，直到所有人进圈。