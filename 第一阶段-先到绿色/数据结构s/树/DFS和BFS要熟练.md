两个树的通用模板：`DFS` 和 `BFS`。都包含建图、`parent/depth`，DFS 还带 `size`。

```python
# ===== 树模板 1：DFS（递归）=====
import sys
sys.setrecursionlimit(1_000_000)
input = sys.stdin.readline

def main():
    n = int(input().strip())
    adj = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    size = [0] * (n + 1)

    def dfs(u: int, p: int) -> None:
        parent[u] = p
        size[u] = 1
        for v in adj[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dfs(v, u)
            size[u] += size[v]

    dfs(1, 0)

    # 示例输出（按需删改）
    print("parent:", *parent[1:])
    print("depth :", *depth[1:])
    print("size  :", *size[1:])

if __name__ == "__main__":
    main()
```

```python
# ===== 树模板 2：BFS（队列）=====
from collections import deque
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    adj = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    parent = [0] * (n + 1)
    depth = [-1] * (n + 1)

    q = deque([1])
    depth[1] = 0
    parent[1] = 0

    while q:
        u = q.popleft()
        for v in adj[u]:
            if depth[v] != -1:   # 已访问
                continue
            depth[v] = depth[u] + 1
            parent[v] = u
            q.append(v)

    # 示例输出（按需删改）
    print("parent:", *parent[1:])
    print("depth :", *depth[1:])

if __name__ == "__main__":
    main()
```

最常用记忆版：
1. DFS：`dfs(u,p)` + `if v==p: continue`
2. BFS：`depth[v]==-1` 判断未访问
3. 树是无向边，建图一定 `u->v` 和 `v->u` 都加。