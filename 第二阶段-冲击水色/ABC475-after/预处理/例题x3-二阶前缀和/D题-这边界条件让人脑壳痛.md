https://atcoder.jp/contests/abc480/tasks/abc480_d

---

### D - 前缀的拼接 (Concatenation of Prefixes)

时间限制：2 秒 / 内存限制：1024 MiB  
分值：400 分

#### 题目描述

给定一个长度为 $N$ 的整数序列 $(A_1, \dots, A_N)$。

按照以下步骤构造一个长度为 $\frac{N(N+1)}{2}$ 的数列 $B$：

- 最初，序列 $B$ 为空。
- 依次对于 $i = 1, 2, \dots, N$，将 $(A_1, \dots, A_i)$ 追加拼接到 $B$ 的末尾。

接下来给出 $Q$ 个以下格式的询问，请依次回答。

- **询问**：给定 $L$ 和 $R$，求 $B_L + B_{L+1} + \dots + B_R$ 的值。

#### 制约条件

- $1 \le N \le 10^5$
- $1 \le Q \le 10^5$
- $-10^8 \le A_i \le 10^8$
- 对于每个询问，满足 $1 \le L \le R \le \frac{N(N+1)}{2}$
- 所有输入均为整数

#### 输入格式

输入从标准输入按以下格式给出：

```
N Q
A_1 A_2 ... A_N
query_1
query_2
:
query_Q
```

其中每个 $\text{query}_i$ 表示第 $i$ 个询问，格式如下：

```
L R
```

#### 输出格式

输出共 $Q$ 行。  
第 $i$ 行输出第 $i$ 个询问的答案。

---

### 样例说明

#### 样例输入 1
```
4 3
1 10 100 1000
3 6
1 10
8 8
```

#### 样例输出 1
```
121
1234
10
```

构造出的序列为 $B = (1, 1, 10, 1, 10, 100, 1, 10, 100, 1000)$。

- 对于第 1 个询问，答案为 $10 + 1 + 10 + 100 = 121$。
- 对于第 2 个询问，答案为 $1 + 1 + 10 + 1 + 10 + 100 + 1 + 10 + 100 + 1000 = 1234$。
- 对于第 3 个询问，答案为 $10$。

#### 样例输入 2
```
7 1
100000000 100000000 100000000 100000000 100000000 100000000 100000000
1 28
```

#### 样例输出 2
```
2800000000
```
注意答案可能会超出 32 位整型（int）的范围。


# 暴力模拟brr TLE RE
``` python 
import sys
import math
from bisect import bisect_left
from collections import defaultdict


def main():

    n, q = map(int, sys.stdin.readline().strip().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    # 直接模拟brr？不太合理，太长了。算了，试试吧
    brr_len = n * (n + 1) // 2
    brr = []

    for i in range(1, brr_len):
        brr.extend(arr[:i])

    pref_b = [0] * (len(brr) + 1)
    for i in range(len(brr)):
        pref_b[i + 1] = pref_b[i] + brr[i]

    outs = []
    for _ in range(q):
        l, r = map(int, sys.stdin.readline().strip().split())

        ans = pref_b[r] - pref_b[l - 1]
        outs.append(ans)

    print('\n'.join(map(str, outs)))
if __name__ == "__main__":
    main()
```

# 二阶前缀和 AC
``` python
import sys  
import math  
from bisect import bisect_left  
from collections import defaultdict  
  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    pre1 = [0] * (n + 1)  
    for i in range(n):  
        pre1[i + 1] = pre1[i] + arr[i]  
  
    # print(pre1)  
    pre2 = [0] * (n + 1)  
    for i in range(1, n + 1):  
        pre2[i] = pre2[i - 1] + pre1[i]  
    # print(pre2)  
  
    crr = []  
    for i in range(1, n + 1):  
        crr.append(i)  
  
    pre3 = [0] * (n + 1)  
    for i in range(n):  
        pre3[i + 1] = pre3[i] + crr[i]  
  
    outs = []  
    for _ in range(q):  
        l, r = map(int, sys.stdin.readline().strip().split())  
        l = l - 1  
  
        idx1 = bisect_left(pre3, l)  
        if pre3[idx1] > l:  
            idx1 -= 1  
        rem1 = pre1[l - pre3[idx1]]  
        x = rem1 + pre2[idx1]  
  
        idx2 = bisect_left(pre3, r)  
        if pre3[idx2] > r:  
            idx2 -= 1  
        rem2 = pre1[r - pre3[idx2]]  
        y = rem2 + pre2[idx2]  
  
        ans = y - x  
        outs.append(ans)  
  
    print('\n'.join(map(str, outs)))  
if __name__ == "__main__":  
    main()
```