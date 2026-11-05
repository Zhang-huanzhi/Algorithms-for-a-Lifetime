如果不使用官方库，在算法竞赛中最推荐手写的是 **Dinic 算法**（AtCoder 的 `MFGraph` 底层也是基于它实现的）。

Dinic 算法的核心思想分为两步循环：
1. **BFS 分层**：从源点 $S$ 开始，计算每个点到源点的最短距离（称为“层次”），如果汇点 $T$ 到不了就结束。
2. **DFS 寻找增广路**：沿着层次严格递增的方向（只走 $d[v] = d[u] + 1$ 的边）推水流，配合**当前弧优化**大幅提速。

---

### Dinic 最大流算法的类封装

你可以直接把这个类当作模板保存：

```python
import sys
from collections import deque

class MaxFlow:
    """
    Dinic 算法实现的最大流
    节点编号支持 0 到 n-1（如果你的编号是 1 到 n，初始化时传 n + 1 即可）
    """
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, cap):
        """添加一条从 u 到 v，容量为 cap 的有向边"""
        # 正向边存 [终点, 容量, 反向边在邻接表中的下标]
        forward_edge = [v, cap, len(self.g[v])]
        # 反向边初始容量为 0
        backward_edge = [u, 0, len(self.g[u])]
        
        self.g[u].append(forward_edge)
        self.g[v].append(backward_edge)

    def _bfs(self, s, t):
        """通过 BFS 对图进行分层"""
        self.level = [-1] * self.n
        self.level[s] = 0
        queue = deque([s])
        
        while queue:
            u = queue.popleft()
            for v, cap, _ in self.g[u]:
                # 只有残余容量大于 0 且未被访问的点才可以分层
                if cap > 0 and self.level[v] < 0:
                    self.level[v] = self.level[u] + 1
                    queue.append(v)
                    
        # 返回汇点 t 是否可达
        return self.level[t] != -1

    def _dfs(self, u, t, pushed):
        """通过 DFS 寻找增广路（包含当前弧优化）"""
        if pushed == 0 or u == t:
            return pushed

        # 从当前弧（cur[u]）开始检查，避免重复遍历已经饱和的边
        for i in range(self.cur[u], len(self.g[u])):
            self.cur[u] = i
            v, cap, rev_idx = self.g[u][i]

            # 只能沿着分层递增的方向走，且残量必须大于 0
            if self.level[u] + 1 != self.level[v] or cap == 0:
                continue

            # 递归向下推流
            tr = self._dfs(v, t, min(pushed, cap))
            if tr == 0:
                continue

            # 更新正向边和反向边的残余容量
            self.g[u][i][1] -= tr
            self.g[v][rev_idx][1] += tr
            return tr

        return 0

    def flow(self, s, t):
        """计算从 s 到 t 的最大流"""
        max_flow = 0
        INF = 10**18

        # 不断进行 BFS 分层 + DFS 推流，直到没有增广路为止
        while self._bfs(s, t):
            self.cur = [0] * self.n  # 当前弧优化初始化
            while True:
                pushed = self._dfs(s, t, INF)
                if pushed == 0:
                    break
                max_flow += pushed

        return max_flow
```

### 核心设计细节说明：
1. **成对反向边（Residual Edge）**：
   在图论中，退流操作是通过走“反向边”实现的。这里通过 `rev_idx` 记录了反向边在对方邻接表中的下标，能够在 $O(1)$ 时间内快速修改反向边的容量。
2. **当前弧优化（`self.cur`）**：
   在同一次 BFS 分层中，如果某条边已经流满了或无法到达汇点，下一次 DFS 就没必要再看它。`cur[u]` 记录了每个节点当前访问到哪条边，这是 Dinic 能够高效运行的关键优化。