https://atcoder.jp/contests/abc479/tasks/abc479_e

---

### E - Cut and Add

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：450 分

#### 题目描述

给定一棵包含 $N$ 个顶点的树，顶点编号为 $1, 2, \dots, N$。保证 $N$ 是奇数。  
边的编号为 $1, 2, \dots, N-1$，第 $i$ 条边连接顶点 $A_i$ 和 $B_i$。

每个顶点 $i$ 都有一个变量 $x_i$，初始时对于所有顶点 $i$ 均有 $x_i = 0$。

接下来，按 $i = 1, 2, \dots, N-1$ 的顺序依次执行以下操作：

1. 首先，移除边 $i$。此时树被分割成两个连通块。
2. 对于包含**顶点数较多**的连通块中的每一个顶点 $v$，将其变量值加上 $i$（即 $x_v \leftarrow x_v + i$）。  
   （由于 $N$ 是奇数，顶点数较多的连通块必定唯一确定。）
3. 最后，恢复边 $i$。

请计算所有操作完成后，每个顶点的变量值 $x_1, x_2, \dots, x_N$。

#### 制约条件

- 所有输入均为整数
- $N$ 为满足 $3 \le N \le 2 \times 10^5 - 1$ 的奇数
- $1 \le A_i < B_i \le N$
- 给定的图是一棵树

#### 输入格式

输入通过标准输入给出，格式如下：

$$N$$  
$$A_1\ B_1$$  
$$A_2\ B_2$$  
$$\vdots$$  
$$A_{N-1}\ B_{N-1}$$  

#### 输出格式

按如下格式输出最终各顶点变量的值：

$$x_1\ x_2\ \dots\ x_N$$

---

### 样例 1

#### 输入
```text
5
2 4
1 5
1 4
1 3
```

#### 输出
```text
10 6 6 7 8
```

#### 解释
- 对于 $i=1$：
  - 移除边 $1$，树分为包含顶点 $\{2\}$ 的连通块和包含顶点 $\{1, 3, 4, 5\}$ 的连通块。
  - 较大的是包含 $\{1, 3, 4, 5\}$ 的连通块，因此给这些顶点的变量都加上 $1$。
  - 变量变为 $(x_1, x_2, x_3, x_4, x_5) = (1, 0, 1, 1, 1)$。
  - 恢复边 $1$。
- 对于 $i=2$：
  - 移除边 $2$，树分为 $\{5\}$ 和 $\{1, 2, 3, 4\}$。
  - 较大的连通块为 $\{1, 2, 3, 4\}$，给这些顶点的变量都加上 $2$。
  - 变量变为 $(x_1, x_2, x_3, x_4, x_5) = (3, 2, 3, 3, 1)$。
  - 恢复边 $2$。
- 对于 $i=3$：
  - 移除边 $3$，树分为 $\{2, 4\}$ 和 $\{1, 3, 5\}$。
  - 较大的连通块为 $\{1, 3, 5\}$，给这些顶点的变量都加上 $3$。
  - 变量变为 $(x_1, x_2, x_3, x_4, x_5) = (6, 2, 6, 3, 4)$。
  - 恢复边 $3$。
- 对于 $i=4$：
  - 移除边 $4$，树分为 $\{3\}$ 和 $\{1, 2, 4, 5\}$。
  - 较大的连通块为 $\{1, 2, 4, 5\}$，给这些顶点的变量都加上 $4$。
  - 变量变为 $(x_1, x_2, x_3, x_4, x_5) = (10, 6, 6, 7, 8)$。
  - 恢复边 $4$。

最终变量值为 $(10, 6, 6, 7, 8)$。

---

### 样例 2

#### 输入
```text
3
1 2
2 3
```

#### 输出
```text
2 3 1
```

---

### 样例 3

#### 输入
```text
9
4 6
2 3
1 5
7 8
2 9
5 6
2 6
7 9
```

#### 输出
```text
20 36 34 28 23 29 23 19 31
```


# AC Code
``` python
import sys
import math
from collections import deque


def main():

    n = int(sys.stdin.readline().strip())
    edges = []
    for _ in range(n - 1):
        u, v = map(int, sys.stdin.readline().strip().split())
        edges.append((u, v))

    adj = [ [] for _ in range(n + 1)]
    for edge_id, (u, v) in enumerate(edges, start = 1):
        adj[u].append( (v, edge_id))
        adj[v].append( (u, edge_id))

    parent = [0] * (n + 1)
    edge_to_parent = [0] * (n + 1)
    order = []

    visited = [False] * (n + 1)
    q = deque([1])
    visited[1] = True

    while q:
        curr = q.popleft()
        order.append(curr)

        for neighbor, edge_id in adj[curr]:
            if not visited[neighbor]:
                visited[neighbor] = True
                parent[neighbor] = curr
                edge_to_parent[neighbor] = edge_id
                q.append(neighbor)


    sz = [1] * (n + 1)
    for u in reversed(order):
        p = parent[u]
        if p != 0:
            sz[p] += sz[u]

    tag = [0] * (n + 1)

    for u in range(2, n + 1):
        e_id = edge_to_parent[u]

        if sz[u] > n // 2:
            tag[u] += e_id
        else:
            tag[1] += e_id
            tag[u] -= e_id


    for u in order:
        p = parent[u]
        if p != 0:
            tag[u] += tag[p]

    print(*tag[1:])

if __name__ == "__main__":
    main()
```

# TLE Code
``` python
import sys
import math
from collections import deque


def main():

    n = int(sys.stdin.readline().strip())
    edges = []
    for _ in range(n - 1):
        u, v = map(int, sys.stdin.readline().strip().split())
        edges.append((u, v))

    adj = [ [] for _ in range(n + 1)]
    for edge_id, (u, v) in enumerate(edges, start = 1):
        adj[u].append( (v, edge_id))
        adj[v].append( (u, edge_id))

    ans = [0] * (n + 1)

    for cut_id in range(1, n):
        u, v = edges[cut_id - 1]

        visited = [False] * (n + 1)
        q = deque([u])
        visited[u] = True
        component_u = []

        while q:
            curr = q.popleft()
            component_u.append(curr)

            for neighbor, edge_id in adj[curr]:

                if edge_id == cut_id:
                    continue

                if not visited[neighbor]:
                    visited[neighbor] = True
                    q.append(neighbor)

        if len(component_u) > n // 2:
            for node in component_u:
                ans[node] += cut_id
        else:
            for node in range(1, n + 1):
                if not visited[node]:
                    ans[node] += cut_id

    print(*ans[1:])

if __name__ == "__main__":
    main()
```