https://atcoder.jp/contests/abc481/tasks/abc481_d

---

### D - 自动驾驶（Autopilot）

运行时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 400 分

#### 题目描述

宇宙中有 $N$ 个行星和 $M$ 条航线。行星的编号为 $1$ 到 $N$，航线的编号为 $1$ 到 $M$。

航线 $i$ 从行星 $A_i$ 单向连接到行星 $B_i$。

在这个宇宙空间中，飞船可以执行以下 $2$ 种操作：

1. **自动驾驶**：移动到与当前所在行星有航线直接相连的所有行星中**编号最小**的行星，该操作**不消耗燃料**。
2. **手动操作**：任意选择一个与当前所在行星有航线直接相连的行星，**消耗 $1$ 单位燃料**并移动到该行星。

请计算飞船从行星 $S$ 出发到达行星 $T$ 所需消耗的**最小燃料量**。如果无法到达，则输出 `-1`。

#### 数据范围

- $2 \le N \le 2 \times 10^5$
- $1 \le M \le 2 \times 10^5$
- $1 \le S, T \le N$
- $S \neq T$
- $1 \le A_i, B_i \le N$
- $A_i \neq B_i$
- 所有输入值均为整数。

#### 输入格式

输入通过标准输入按以下格式给出：

$$
N \quad M \quad S \quad T
$$
$$
A_1 \quad B_1
$$
$$
\vdots
$$
$$
A_M \quad B_M
$$

#### 输出格式

输出一个整数，表示到达行星 $T$ 所需的最小燃料消耗量；若无法到达则输出 `-1`。

---

#### 样例 1

**输入**
```text
7 10 2 1
2 5
2 4
4 2
4 3
3 1
3 5
3 6
5 6
6 1
6 7
```

**输出**
```text
1
```

**样例解释 1**  
按照以下路径移动，可以仅消耗 $1$ 单位燃料从行星 $2$ 到达行星 $1$：
1. 从行星 $2$ 移动到行星 $4$（相连的有 $4, 5$，其中最小的是 $4$），不消耗燃料。
2. 从行星 $4$ 移动到行星 $3$，消耗 $1$ 单位燃料。
3. 从行星 $3$ 移动到行星 $1$（相连的有 $1, 5, 6$，其中最小的是 $1$），不消耗燃料。

---

#### 样例 2

**输入**
```text
7 10 2 7
2 5
2 4
4 2
4 3
3 1
3 5
3 6
5 6
6 1
6 7
```

**输出**
```text
2
```

**样例解释 2**  
按照以下路径移动，可以消耗 $2$ 单位燃料从行星 $2$ 到达行星 $7$：
1. 从行星 $2$ 移动到行星 $4$，不消耗燃料。
2. 从行星 $4$ 移动到行星 $2$，不消耗燃料。
3. 从行星 $2$ 移动到行星 $5$，消耗 $1$ 单位燃料。
4. 从行星 $5$ 移动到行星 $6$，不消耗燃料。
5. 从行星 $6$ 移动到行星 $7$，消耗 $1$ 单位燃料。

---

#### 样例 3

**输入**
```text
2 1 1 2
2 1
```

**输出**
```text
-1
```

**样例解释 3**  
无法从行星 $1$ 到达行星 $2$。

# AC Dijkstra
```python
import heapq
import math
import sys

def main():

    n, m, s, t = map(int, sys.stdin.readline().strip().split())

    adj = [ [] for _ in range(n + 1)]
    min_v = [math.inf] * (n + 1)

    for _ in range(m):
        u, v = map(int, sys.stdin.readline().strip().split())

        adj[u].append(v)
        if v < min_v[u]:
            min_v[u] = v

    dist = [math.inf] * (n + 1)
    dist[s] = 0

    pq = [(0, s)]

    while pq:
        d, u = heapq.heappop(pq)

        if d > dist[u]:
            continue

        if u == t:
            break

        if min_v[u] != math.inf:
            mv = min_v[u]

            if d < dist[mv]:
                dist[mv] = d
                heapq.heappush(pq, (d, mv))

        cost_next = d + 1
        for v in adj[u]:
            if cost_next < dist[v]:
                dist[v] = cost_next
                heapq.heappush(pq, (cost_next, v))

    print(dist[t] if dist[t] != math.inf else -1)


if __name__ == "__main__":
    main()
```

# AC 01BFS
```python
import math
import sys
from collections import defaultdict, deque


def main():

    n, m, s, t = map(int, sys.stdin.readline().strip().split())

    graph = [ [] for _ in range(n + 1)]

    arr = [math.inf] * (n + 1)
    for _ in range(m):
        a, b = map(int, sys.stdin.readline().strip().split())

        graph[a].append(b)

        if b <= arr[a]:
            arr[a] = b

    # 01 BFS? Dijstra?
    q = deque()
    dist =  [math.inf] * (n + 1)

    q.append(s)
    dist[s] = 0

    ans = -1
    while q:

        u = q.popleft()
        cur = dist[u]

        if u == t:
            ans = dist[u]
            break

        v = arr[u]

        if v != math.inf:
            if cur  < dist[v]:
                dist[v] = cur
                q.appendleft(v)

        for v in graph[u]:
            if cur + 1 < dist[v]:
                dist[v] = cur + 1
                q.append(v)

    print(ans)

if __name__ == "__main__":
    main()
```