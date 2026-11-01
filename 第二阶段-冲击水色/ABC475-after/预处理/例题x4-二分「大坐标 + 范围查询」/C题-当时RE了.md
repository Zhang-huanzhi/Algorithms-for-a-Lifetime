https://atcoder.jp/contests/abc482/tasks/abc482_c

---

### C - Band Count

运行时间限制: 2 秒 / 内存限制: 1024 MiB

分值: 300 分

#### 题目描述
有一个高为 $H$ 行、宽为 $W$ 列的网格。我们将从上往下数第 $i$ 行、从左往右数第 $j$ 列的格子记作 $(i, j)$。

网格上放置了 $N$ 个棋子，其中第 $i$ 个棋子位于 $(R_i, C_i)$。

请回答 $Q$ 个查询。查询共有以下 2 种类型：

- `1 u d`：输出从第 $u$ 行到第 $d$ 行（包含两端）所放置的棋子总数。
- `2 l r`：输出从第 $l$ 列到第 $r$ 列（包含两端）所放置的棋子总数。

#### 数据范围与约束
- $1 \le H, W \le 10^9$
- $1 \le N \le \min(H \times W, 3 \times 10^5)$
- $1 \le R_i \le H$
- $1 \le C_i \le W$
- 若 $i \neq j$，则 $(R_i, C_i) \neq (R_j, C_j)$（即同一个格子最多只有一个棋子）
- $1 \le Q \le 3 \times 10^5$
- $1 \le u \le d \le H$
- $1 \le l \le r \le W$
- 所有输入的值均为整数

#### 输入格式
输入从标准输入按以下格式给出：

$$
\begin{aligned}
&H\ W\ N\ Q \\
&R_1\ C_1 \\
&R_2\ C_2 \\
&\vdots \\
&R_N\ C_N \\
&\text{query}_1 \\
&\text{query}_2 \\
&\vdots \\
&\text{query}_Q
\end{aligned}
$$

每个查询 $\text{query}_q$ 为以下两种形式之一：

```
1 u d
```
或
```
2 l r
```

#### 输出格式
输出共 $Q$ 行。第 $i$ 行输出第 $i$ 个查询的答案。

---

#### 样例 1

**输入**
```text
5 7 5 6
1 1
2 3
2 5
4 3
5 7
1 2 4
2 3 5
1 3 3
2 1 7
1 2 2
2 7 7
```

**输出**
```text
3
3
0
5
2
1
```

**说明**  
对于第 1 个查询，位于第 2 行到第 4 行的棋子为第 2、3、4 个棋子，共计 3 个。

---

#### 样例 2

**输入**
```text
17 23 12 12
14 8
2 19
17 3
7 11
2 4
10 19
7 8
1 23
14 11
2 23
17 19
7 2
1 2 7
2 9 20
1 3 6
2 8 8
1 11 17
2 1 23
1 1 1
2 20 22
1 7 14
2 3 11
1 15 17
2 19 23
```

**输出**
```text
6
5
0
2
4
12
1
0
6
6
2
5
```

# AC 二分查找
``` python
import math
import sys
from bisect import bisect_right

def main():

    h, w, n, q = map(int, sys.stdin.readline().strip().split())

    rrr = []
    crr = []

    # 无语了，这个h,w 范围太大了，一维都开不了，离散？
    for _ in range(n):
        r, c = map(int, sys.stdin.readline().strip().split())

        rrr.append(r)
        crr.append(c)

    rrr.sort()
    crr.sort()

    outs =[]
    for _ in range(q):
        line = list(sys.stdin.readline().strip().split())

        op = line[0]

        if op == '1':
            u, d = int(line[1]), int(line[2])
            ans = bisect_right(rrr, d) - bisect_right(rrr, u - 1)
            outs.append(ans)

        if op == '2':
            l, r = int(line[1]), int(line[2])
            ans = bisect_right(crr, r) - bisect_right(crr, l - 1)
            outs.append(ans)

    print('\n'.join(map(str, outs)))
if __name__ == "__main__":
    main()
```

# RE 前缀和
``` python
import math
import sys

def main():

    h, w, n, q = map(int, sys.stdin.readline().strip().split())

    rrr = [0] * h
    crr = [0] * w

    for _ in range(n):
        r, c = map(int, sys.stdin.readline().strip().split())

        rrr[r - 1] += 1
        crr[c - 1] += 1

    pre_r = [0] * (h + 1)
    for i in range(1, h + 1):
        pre_r[i] = pre_r[i - 1] + rrr[i - 1]

    pre_c = [0] * (w + 1)
    for i in range(1, w + 1):
        pre_c[i] = pre_c[i - 1] + crr[i - 1]

    outs =[]
    for _ in range(q):
        line = list(sys.stdin.readline().strip().split())

        op = line[0]

        if op == '2':
            u, d = int(line[1]), int(line[2])
            outs.append(pre_c[d] - pre_c[u - 1])

        if op == '1':
            l, r = int(line[1]), int(line[2])
            outs.append(pre_r[r] - pre_r[l - 1])

    print('\n'.join(map(str, outs)))
if __name__ == "__main__":
    main()
```