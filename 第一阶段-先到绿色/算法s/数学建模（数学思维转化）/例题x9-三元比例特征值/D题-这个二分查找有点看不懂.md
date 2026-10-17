https://atcoder.jp/contests/abc439/tasks/abc439_d

---

### D - Kadomatsu Subsequence

**时间限制**: 2 秒  
**内存限制**: 1024 MiB  
**分值**: 425 分

---

#### 题目描述

给定一个长度为 $N$ 的整数序列 $A = (A_1, A_2, \dots, A_N)$。

请计算满足以下所有条件的整数三元组 $(i, j, k)$ 的数量：

1. $1 \le i, j, k \le N$
2. $A_i : A_j : A_k = 7 : 5 : 3$
3. $\min(i, j, k) = j$ 或 $\max(i, j, k) = j$

#### 数据范围

* 输入的所有值均为整数。
* $1 \le N \le 3 \times 10^5$
* $1 \le A_i \le 10^9$

---

#### 输入格式

输入通过标准输入给出，格式如下：

$$N$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$

#### 输出格式

输出一个整数，表示满足条件的三元组数量。

---

#### 输入样例 1

```text
10
3 10 7 10 7 6 7 6 5 14
```

#### 输出样例 1

```text
7
```

**样例 1 说明**：  
满足条件的整数三元组 $(i, j, k)$ 共有以下 7 个：

* $(3, 9, 1)$：$A_i : A_j : A_k = 7 : 5 : 3$，且 $\max(i, j, k) = j$。
* $(5, 9, 1)$：$A_i : A_j : A_k = 7 : 5 : 3$，且 $\max(i, j, k) = j$。
* $(7, 9, 1)$：$A_i : A_j : A_k = 7 : 5 : 3$，且 $\max(i, j, k) = j$。
* $(10, 2, 6)$：$A_i : A_j : A_k = 14 : 10 : 6 = 7 : 5 : 3$，且 $\min(i, j, k) = j$。
* $(10, 2, 8)$：$A_i : A_j : A_k = 14 : 10 : 6 = 7 : 5 : 3$，且 $\min(i, j, k) = j$。
* $(10, 4, 6)$：$A_i : A_j : A_k = 14 : 10 : 6 = 7 : 5 : 3$，且 $\min(i, j, k) = j$。
* $(10, 4, 8)$：$A_i : A_j : A_k = 14 : 10 : 6 = 7 : 5 : 3$，且 $\min(i, j, k) = j$。

---

#### 输入样例 2

```text
6
210 210 210 210 210 210
```

#### 输出样例 2

```text
0
```

---

#### 输入样例 3

```text
21
49 30 50 21 35 15 21 70 35 9 50 70 21 49 30 50 70 15 9 21 30
```

#### 输出样例 3

```text
34
```

---

# AC Code
``` python
import sys  
from bisect import bisect_left  
from collections import Counter, defaultdict  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 算特征值  
    pos7 = defaultdict(list)  
    pos5 = defaultdict(list)  
    pos3 = defaultdict(list)  
  
    for l in range(n):  
        val = arr[l]  
        if val % 7 == 0:  
            pos7[val // 7].append(l)  
        if val % 5 == 0:  
            pos5[val // 5].append(l)  
        if val % 3 == 0:  
            pos3[val // 3].append(l)  
  
    ans = 0  
    for t in pos5.keys():  
        total = len(pos7[t]) * len(pos5[t]) * len(pos3[t])  
  
        if total == 0:  
            continue  
  
        invalid = 0  
        for j_idx in pos5[t]:  
            a = bisect_left(pos7[t], j_idx)  
            d = len(pos7[t]) - a  
  
            c = bisect_left(pos3[t], j_idx)  
            b = len(pos3[t]) - c  
  
            invalid += (a * b) + (c * d)  
  
  
        ans += (total - invalid)  
  
    print(ans)  
if __name__ == "__main__":  
    main()
```