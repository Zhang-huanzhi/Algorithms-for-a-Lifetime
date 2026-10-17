https://atcoder.jp/contests/abc427/tasks/abc427_c

---

### C - Bipartize / 运行时间限制: 2 秒 / 内存限制: 1024 MiB

分值：350 分

#### 问题描述
给定一个含有 $N$ 个顶点和 $M$ 条边的简单无向图。图的顶点编号为 $1, 2, \dots, N$。第 $i$ 条边 $(1 \le i \le M)$ 连接顶点 $u_i$ 和顶点 $v_i$。

你可以进行以下操作 $0$ 次或多次：
* 选择一条尚未被删除的边，并将其删除。

您的目标是将该图变为一个**二分图**。请问为了使操作后的图成为二分图，最少需要进行多少次操作？

#### 什么是简单图？
简单图是指不含自环（即 $u_i = v_i$ 的边）和重边（即满足 $u_i = u_j$ 且 $v_i = v_j$ 的边对）的图。

#### 什么是二分图？
二分图是指可以通过将每个顶点染成黑色或白色，从而满足以下条件的图：
* 对于图中的任意一条边，其连接的两个顶点的颜色都不相同。

#### 数据范围
* $2 \le N \le 10$
* $1 \le M \le \frac{N(N-1)}{2}$
* $1 \le u_i < v_i \le N$ $(1 \le i \le M)$
* 给定的图是简单图。
* 所有输入均为整数。

#### 输入
输入按以下格式从标准输入给出：

$$N \quad M$$
$$u_1 \quad v_1$$
$$u_2 \quad v_2$$
$$\vdots$$
$$u_M \quad v_M$$

#### 输出
输出为了使图成为二分图所需进行的最少操作次数。

---

#### 输入样例 1
```text
5 8
1 2
1 3
1 4
2 3
2 5
3 4
3 5
4 5
```

#### 输出样例 1
```text
2
```
例如，通过删除连接顶点 $1$ 和顶点 $3$ 的边，以及连接顶点 $3$ 和顶点 $5$ 的边，可以使该图成为二分图。
由于无法通过 $1$ 次或更少的操作使图成为二分图，因此输出 $2$。

---

#### 输入样例 2
```text
2 1
1 2
```

#### 输出样例 2
```text
0
```
该图起初就是一个二分图。因此，需要进行的操作次数为 $0$。

---

#### 输入样例 3
```text
10 20
5 9
1 4
3 8
1 6
4 10
5 7
5 6
3 7
3 6
5 10
1 3
3 4
6 7
1 2
4 7
1 5
1 9
9 10
4 5
8 9
```

#### 输出样例 3
```text
5
```


# AC Code
``` python
import sys  
import itertools  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    ans = m  
    for _ in range(m):  
        u, v = map(int, sys.stdin.readline().strip().split())  
  
        arr.append((u, v))  
  
    for group in itertools.product([0, 1], repeat=n):  
        cnt = 0  
  
        for u, v in arr:  
            if group[u - 1] == group[v - 1]:  
                cnt += 1  
  
        if cnt < ans:  
            ans = cnt  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```