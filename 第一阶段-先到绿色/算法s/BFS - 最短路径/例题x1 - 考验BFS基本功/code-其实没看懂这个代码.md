``` python
import sys  
from itertools import combinations  
  
sys.setrecursionlimit(2000)  
  
  
def solve():  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    # 用邻接矩阵表示原图，如果 u 和 v 之间有边，则 original_grid[u][v] = 1    # 这样可以 $O(1)$ 时间判断某条边是不是原图里的边  
    original_grid = [[0] * (n + 1) for _ in range(n + 1)]  
  
    for _ in range(m):  
        a, b = map(int, sys.stdin.readline().strip().split())  
        original_grid[a][b] = 1  
        original_grid[b][a] = 1  
  
    deg = [0] * (n + 1)  # 记录当前每个顶点的度数  
    max_kept_edges = 0  # 记录最多能保留多少条原图的边 (即最大 K 值)  
  
    # u 表示当前正在为哪个顶点寻找连线  
    # current_k 表示当前已经保留了多少条原图的边  
    def dfs(u, current_k):  
        nonlocal max_kept_edges  
  
        # 如果所有点都处理完了  
        if u == n + 1:  
            # 如果所有点度数都刚好是 2，说明这是一个合法的 2-正则图  
            if all(deg[i] == 2 for i in range(1, n + 1)):  
                max_kept_edges = max(max_kept_edges, current_k)  
            return  
  
        # 剪枝：如果当前点度数已经超标，说明不合法  
        if deg[u] > 2:  
            return  
  
        needed = 2 - deg[u]  
        if needed == 0:  
            # 当前点度数已满，直接看下一个点  
            dfs(u + 1, current_k)  
            return  
  
        # 寻找后面可以连线的候选点  
        candidates = []  
        for v in range(u + 1, n + 1):  
            if deg[v] < 2:  
                candidates.append(v)  
  
        if len(candidates) < needed:  
            return  
  
        # 尝试所有连线组合  
        for comb in combinations(candidates, needed):  
            # 计算这次连线能“保留”多少条原图的边  
            kept_count = 0  
            for v in comb:  
                deg[v] += 1  
                if original_grid[u][v] == 1:  
                    kept_count += 1  
            deg[u] += needed  
  
            # 递归  
            dfs(u + 1, current_k + kept_count)  
  
            # 回溯  
            deg[u] -= needed  
            for v in comb:  
                deg[v] -= 1  
  
    # 开始搜索  
    dfs(1, 0)  
  
    # 根据公式：最小操作次数 = N + M - 2 * 最大保留边数  
    ans = n + m - 2 * max_kept_edges  
    print(ans)  
  
  
if __name__ == '__main__':  
    solve()
```