https://atcoder.jp/contests/abc463/tasks/abc463_e

---

### E - Roads and Gates
时间限制: 2 秒 / 内存限制: 1024 MiB

分值: 450 分

#### 题目描述
AtCoder 国有 $N$ 个城市和 $M$ 条道路。
第 $i$ 条道路（$1 \le i \le M$）双向连接城市 $u_i$ 和城市 $v_i$，通过该道路在两个城市之间单向移动需要花费 $T_i$ 分钟。

此外，每个城市都设有一个传送门。利用传送门，你可以从城市 $i$（$1 \le i \le N$）直接传送到城市 $j$（$1 \le j \le N$），此过程需要花费 $X_i + X_j + Y$ 分钟。

除了上述的道路和传送门之外，没有其他方法可以在 AtCoder 国的城市之间移动。

对于每个 $k = 2, 3, \dots, N$，请解决以下问题：
* 求从城市 $1$ 移动到城市 $k$ 所需的最小时间。

注意，在同一个城市内从一条道路或传送门转移到另一条道路或传送门所需的时间可以忽略不计（即花费时间为 $0$）。

#### 限制条件
* $2 \le N \le 2 \times 10^5$
* $0 \le M \le 2 \times 10^5$
* $1 \le u_i < v_i \le N$ （$1 \le i \le M$）
* $1 \le T_i \le 10^9$ （$1 \le i \le M$）
* $1 \le X_i \le 10^9$ （$1 \le i \le N$）
* $1 \le Y \le 10^9$
* 输入的所有数据均为整数。

#### 输入格式
输入通过标准输入给出，格式如下：

$$N \quad M \quad Y$$
$$u_1 \quad v_1 \quad T_1$$
$$u_2 \quad v_2 \quad T_2$$
$$\vdots$$
$$u_M \quad v_M \quad T_M$$
$$X_1 \quad X_2 \quad \dots \quad X_N$$

#### 输出格式
依次输出 $k = 2, 3, \dots, N$ 的问题答案，各答案之间用空格隔开。

---

#### 输入样例 1
```text
7 7 3
1 2 1
1 3 6
2 3 4
3 5 8
3 7 4
4 5 2
4 7 9
3 1 4 1 5 9 2
```

#### 输出样例 1
```text
1 5 6 8 14 7
```

**样例 1 说明：**
例如，从城市 $1$ 移动到城市 $7$ 可以通过以下方式在 $7$ 分钟内完成：
1. 使用第 $1$ 条道路，花费 $1$ 分钟从城市 $1$ 移动到城市 $2$。
2. 使用传送门，花费 $1 + 2 + 3 = 6$ 分钟从城市 $2$ 传送至城市 $7$。

由于无法在 $6$ 分钟或更短的时间内从城市 $1$ 移动到城市 $7$，因此对于 $k=7$ 的最小时间为 $7$。

#### 输入样例 2
```text
2 0 1000000000
1000000000 1000000000
```

#### 输出样例 2
```text
3000000000
```

**样例 2 说明：**
请注意，答案可能会超过 $2^{31} - 1$，因此在编程时需要使用 64 位整数类型（例如 C++ 中的 `long long`）。

#### 输入样例 3
```text
12 20 873
2 7 940
6 9 444
6 11 809
7 8 786
9 10 468
7 10 234
6 10 660
4 12 939
8 10 896
1 11 953
8 10 818
4 8 967
3 9 724
6 7 929
3 4 948
1 3 999
10 11 724
7 10 338
1 8 967
1 12 733
581 978 950 629 583 729 554 712 438 930 774 279
```

#### 输出样例 3
```text
2432 999 1672 2037 1762 1753 967 1723 1677 953 733
```


---

# AC Code
``` python
import bisect  
import heapq  
import sys  
from collections import deque  
  
  
def main():  
    n, m, y = map(int, sys.stdin.readline().strip().split())  
  
    # 数组大小为 n+1，我们可以直接把 0 号节点作为虚拟中转点  
    arr = [[] for i in range(n + 1)]  
  
    # 读取 m 条双向边  
    for i in range(m):  
        u, v, t = map(int, sys.stdin.readline().strip().split())  
        arr[u].append((v, t))  
        arr[v].append((u, t))  
  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 建立与虚拟节点 0 的连接  
    # brr 是 0 索引的，城市 i (1-based) 对应的 X_i 值为 brr[i-1]    for i in range(1, n + 1):  
        x_i = brr[i - 1]  
        # 城市 i -> 虚拟节点 0，代价为 X_i        arr[i].append((0, x_i))  
        # 虚拟节点 0 -> 城市 i，代价为 X_i + Y        arr[0].append((i, x_i + y))  
  
    # 存储到各节点的最短距离，初始化为无穷大  
    cost = [float('inf')] * (n + 1)  
    cost[1] = 0  
  
    # 优先队列中存储 (距离, 节点)  
    q = [(0, 1)]  
  
    while q:  
        d, u = heapq.heappop(q)  
  
        if d > cost[u]:  
            continue  
  
        for v, t in arr[u]:  
            if cost[u] + t < cost[v]:  
                cost[v] = cost[u] + t  
                heapq.heappush(q, (cost[v], v))  
  
    print(*(cost[2:]))  
  
  
if __name__ == '__main__':  
    main()
```