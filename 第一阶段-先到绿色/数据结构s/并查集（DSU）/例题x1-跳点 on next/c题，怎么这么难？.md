https://atcoder.jp/contests/abc426/tasks/abc426_c

---

### C - Upgrade Required  
时间限制: 2 秒 / 内存限制: 1024 MiB

分值：300 分

#### 题目描述
某操作系统共有 $N$ 个版本，按照从旧到新的顺序编号为 $1, 2, \dots, N$。
现有 $N$ 台电脑，起初第 $i$ 台电脑的操作系统版本为 $i$。

请依次进行 $Q$ 次操作。其中第 $i$ 次操作如下：
* 将当前操作系统版本为 $X_i$ 或更低的所有电脑升级到版本 $Y_i$（其中 $Y_i > X_i$）。然后，输出在此次操作中进行了升级的电脑台数。

请注意，对于 $i < Q$，每一次操作都是在前一次操作完成后的状态下继续进行的。

#### 约束条件
* 所有输入均为整数
* $2 \le N \le 10^6$
* $1 \le Q \le 2 \times 10^5$
* $1 \le X_i < Y_i \le N$

#### 输入
输入通过标准输入以以下格式给出：

$$
\begin{array}{l}
N \quad Q \\
X_1 \quad Y_1 \\
X_2 \quad Y_2 \\
\vdots \\
X_Q \quad Y_Q
\end{array}
$$

#### 输出
输出 $Q$ 行。
其中第 $i$ 行输出在第 $i$ 次操作中进行了升级的电脑台数。

---

#### 输入样例 1
```text
8 5
2 6
3 5
1 7
5 7
7 8
```

#### 输出样例 1
```text
2
1
0
3
7
```

#### 样例解释 1
该输入包含 5 次操作。

起初，8 台电脑的版本分别为 $1, 2, 3, 4, 5, 6, 7, 8$。
* **第 1 次操作**：将版本为 $2$ 或更低的电脑升级到版本 $6$。
  共有 2 台电脑被升级，各电脑的版本变为 $6, 6, 3, 4, 5, 6, 7, 8$。
* **第 2 次操作**：将版本为 $3$ 或更低的电脑升级到版本 $5$。
  共有 1 台电脑被升级，各电脑的版本变为 $6, 6, 5, 4, 5, 6, 7, 8$。
* **第 3 次操作**：将版本为 $1$ 或更低的电脑升级到版本 $7$。
  共有 0 台电脑被升级，各电脑的版本仍为 $6, 6, 5, 4, 5, 6, 7, 8$。
* **第 4 次操作**：将版本为 $5$ 或更低的电脑升级到版本 $7$。
  共有 3 台电脑被升级，各电脑的版本变为 $6, 6, 7, 7, 7, 6, 7, 8$。
* **第 5 次操作**：将版本为 $7$ 或更低的电脑升级到版本 $8$。
  共有 7 台电脑被升级，各电脑的版本变为 $8, 8, 8, 8, 8, 8, 8, 8$。


# 暴力Code TLE
``` python
import sys
from collections import Counter

def main():

    n, q = map(int, sys.stdin.readline().strip().split())

    arr = [*range(1, n+1)]

    for _ in range(q):
        x, y = map(int, sys.stdin.readline().strip().split())

        cnt = 0
        for i in range(n):
            if arr[i] <= x:
                arr[i] = y
                cnt += 1

        print(cnt)

if __name__ == "__main__":
    main()
```
# DUS Code AC

``` python
import sys

def main():
    n, q = map(int, sys.stdin.readline().strip().split())

    # cnt[v] 表示当前版本 v 有多少台电脑
    cnt = [0] + [1] * n + [0]

    # parent[v] 表示从 v 开始，下一个仍可能有电脑的版本
    parent = list(range(n + 2))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    ans_list = []

    for _ in range(q):
        x, y = map(int, sys.stdin.readline().strip().split())

        upgraded = 0

        # 找当前最小的、仍有电脑的版本
        v = find(1)

        # 清空所有 <= x 的非空版本
        while v <= x:
            upgraded += cnt[v]
            cnt[v] = 0

            # 版本 v 已经没有电脑了，以后直接跳过它
            parent[v] = find(v + 1)
            v = parent[v]

        # 被升级的电脑全部进入版本 y
        cnt[y] += upgraded

        ans_list.append(str(upgraded))

    print("\n".join(ans_list))

if __name__ == "__main__":
    main()
```