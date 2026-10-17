https://atcoder.jp/contests/abc427/tasks/abc427_d

---

### D - The Simple Game

时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 425 分

#### 题目描述

给定一个拥有 $N$ 个顶点和 $M$ 条边的有向图。顶点编号为 $1$ 到 $N$，第 $i$ 条边是从顶点 $U_i$ 指向顶点 $V_i$ 的有向边。这里，每个顶点的出度至少为 $1$。

每个顶点上都写有一个字符，顶点 $i$ 上写的字符是 $S_i$（即字符串 $S$ 的第 $i$ 个字符）。

Alice 和 Bob 在这个图上使用一个棋子进行以下游戏。

初始时，棋子放在顶点 $1$。Alice 先手，Bob 后手，两人交替进行以下操作，各进行 $K$ 次。

* 设当前棋子所在的顶点为 $u$。选择一个顶点 $v$，使得存在一条从顶点 $u$ 指向顶点 $v$ 的有向边，然后将棋子移动到顶点 $v$。

设最终（两人各操作 $K$ 次后）棋子所在的顶点为 $v$。若 $S_v = \text{A}$，则 Alice 获胜；若 $S_v = \text{B}$，则 Bob 获胜。

请判断在双方都采取最优策略时，游戏的获胜者是谁。

每个输入包含 $T$ 个测试用例。请对每个测试用例进行解答。

#### 数据范围与约束

* $1 \le T$
* $2 \le N, M \le 2 \times 10^5$
* $1 \le K \le 10^9$
* $S$ 是一个长度为 $N$ 且仅由 `A` 和 `B` 组成的字符串。
* $1 \le U_i, V_i \le N$
* 当 $i \neq j$ 时，$(U_i, V_i) \neq (U_j, V_j)$
* 每个顶点的出度至少为 $1$。
* 在一个输入中，所有测试用例的 $N$ 之和不超过 $2 \times 10^5$。
* 在一个输入中，所有测试用例的 $M$ 之和不超过 $2 \times 10^5$。

#### 输入格式

输入按以下格式从标准输入给出。

$$T$$
$$\text{case}_1$$
$$\text{case}_2$$
$$\vdots$$
$$\text{case}_T$$

其中，$\text{case}_i$ 表示第 $i$ 个测试用例，每个测试用例的格式如下：

$$N \quad M \quad K$$
$$S$$
$$U_1 \quad V_1$$
$$U_2 \quad V_2$$
$$\vdots$$
$$U_M \quad V_M$$

#### 输出格式

输出 $T$ 行。  
第 $i$ 行中，若在第 $i$ 个测试用例中双方都采取最优策略时 Alice 获胜，则输出 `Alice`；若 Bob 获胜，则输出 `Bob`。

---

#### 输入样例 1

```text
3
4 6 2
AABB
1 2
2 3
3 1
3 3
3 4
4 2
4 6 2
ABAB
1 2
2 3
3 1
3 3
3 4
4 2
5 8 3
ABABB
1 2
2 2
2 3
3 1
3 4
4 4
4 5
5 3
```

#### 输出样例 1

```text
Alice
Bob
Bob
```

#### 样例说明 1

以下说明第一个测试用例的一种游戏进行过程。请注意，该过程并不代表双方都采取了最优策略。

* 初始时，棋子放在顶点 $1$。
* Alice 将棋子移动到顶点 $2$。
* Bob 将棋子移动到顶点 $3$。
* Alice 将棋子移动到顶点 $3$。
* Bob 将棋子移动到顶点 $1$。
* 此时，由于 $S_1 = \text{A}$，Alice 获胜。

# 题解 AC Code
``` python
import sys  
import itertools  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
        # 读取 N, M, K        n, m, k = map(int, sys.stdin.readline().strip().split())  
  
        # 读取字符串 S        s = sys.stdin.readline().strip()  
  
        # 建立有向图邻接表  
        G = [[] for _ in range(n)]  
        for _ in range(m):  
            u, v = map(int, sys.stdin.readline().strip().split())  
            u -= 1  # 转换为 0-indexed            v -= 1  # 转换为 0-indexed            G[u].append(v)  
  
        # 初始化 dp 数组，代表游戏结束时的胜负状态  
        dp = [char == 'A' for char in s]  
  
        # 逆向模拟 K 轮  
        for step in range(k):  
  
            # --- 1. 倒推 Bob 的回合 ---            ep_bob = [True] * n  
            for i in range(n):  
                for j in G[i]:  
                    if not dp[j]:  
                        ep_bob[i] = False  
            dp = ep_bob  
  
            # --- 2. 倒推 Alice 的回合 ---            ep_alice = [False] * n  
            for i in range(n):  
                for j in G[i]:  
                    if dp[j]:  
                        ep_alice[i] = True  
            dp = ep_alice  
  
        # 输出当前测试用例的答案  
        if dp[0]:  
            print("Alice")  
        else:  
            print("Bob")  
  
  
  
if __name__ == "__main__":  
    main()
```