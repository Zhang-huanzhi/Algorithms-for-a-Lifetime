``` python

import sys
sys.setrecursionlimit(1_000_000)
input = sys.stdin.readline

def main():
    n = int(input().strip())

    # 1) 邻接表建树（无向）
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    size = [0] * (n + 1)

    # 2) DFS: 同时求 parent、depth、size
    def dfs(u, p):
        parent[u] = p
        size[u] = 1  # 先算自己
        for v in adj[u]:
            if v == p:
                continue
            depth[v] = depth[u] + 1
            dfs(v, u)
            size[u] += size[v]  # 回溯累加子树大小

    dfs(1, 0)

    # 3) 输出
    print(*parent[1:])
    print(*depth[1:])
    print(*size[1:])

if __name__ == "__main__":
    main()
    
    
```