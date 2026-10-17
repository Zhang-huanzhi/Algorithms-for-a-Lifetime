https://atcoder.jp/contests/abc468/tasks/abc468_c

---

### C - Between P and Q

**时间限制**: 2 秒 / **内存限制**: 1024 MiB

**分值**: 300 分

#### 问题描述

给定一个整数 $N$ 以及两个由 $(1, 2, \dots, N)$ 排列得到的整数序列 $P = (P_1, P_2, \dots, P_N)$ 和 $Q = (Q_1, Q_2, \dots, Q_N)$。

请问有多少个 $(1, 2, \dots, N)$ 的排列，在字典序上大于 $P$ 且小于 $Q$？

什么是整数序列的字典序？

整数序列 $S = (S_1, S_2, \dots, S_{|S|})$ 在字典序上小于整数序列 $T = (T_1, T_2, \dots, T_{|T|})$，是指以下条件 1 和 2 之一成立。其中 $|S|, |T|$ 分别表示序列 $S, T$ 的长度。

1. $|S| < |T|$ 且 $(S_1, S_2, \dots, S_{|S|}) = (T_1, T_2, \dots, T_{|S|})$。
2. 存在一个整数 $1 \le i \le \min(\{ |S|, |T| \})$，使得以下两个条件同时成立：
   - $(S_1, S_2, \dots, S_{i-1}) = (T_1, T_2, \dots, T_{i-1})$
   - $S_i$（作为数值）小于 $T_i$。


#### 数据范围

- $1 \le N \le 10$
- $P$ 和 $Q$ 均是 $(1, 2, \dots, N)$ 的排列。
- 输入的所有值均为整数。

#### 输入格式

输入由标准输入按以下格式给出：

$$N$$
$$P_1 \quad P_2 \quad \dots \quad P_N$$
$$Q_1 \quad Q_2 \quad \dots \quad Q_N$$

#### 输出格式

输出一个整数，表示满足条件的排列个数。

---

#### 样例输入 1
```text
3
1 3 2
3 1 2
```

#### 样例输出 1
```text
2
```
满足条件的排列有 $(2, 1, 3)$ 和 $(2, 3, 1)$ 这 $2$ 个。因此输出 $2$。

---

#### 样例输入 2
```text
5
5 4 2 1 3
5 1 2 3 4
```

#### 样例输出 2
```text
0
```
不存在满足条件的 $(1, 2, 3, 4, 5)$ 的排列。

---

#### 样例输入 3
```text
7
3 6 5 2 7 1 4
4 1 5 7 2 3 6
```

#### 样例输出 3
```text
223
```


# 暴力 Code AC
``` python
import itertools
import sys

def main():
    
    n = int(sys.stdin.readline().strip())

    prr = tuple(map(int, sys.stdin.readline().strip().split()))
    qrr = tuple(map(int, sys.stdin.readline().strip().split()))

    # 直接暴力不就行了，全排列

    ans = 0
    for comb in itertools.permutations(range(1, n + 1)):

        comb = ( comb )

        if prr < comb < qrr:
            ans += 1

    print(ans)

if __name__ == "__main__":
    main()
```
