https://atcoder.jp/contests/abc433/tasks/abc433_d

---

# D - 183183

时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 400 分

### 题目描述

对于正整数 $x, y$，定义 $f(x, y)$ 如下：

> 将 $x$ 和 $y$ 的不含前导零的十进制表示分别视为字符串，按此顺序将它们拼接得到字符串 $S$，再将 $S$ 解释为十进制整数的值。  
> 例如：$f(12, 3) = 123$，$f(100, 40) = 10040$。

给定正整数 $N, M$ 以及一个长度为 $N$ 的正整数序列 $A = (A_1, A_2, \dots, A_N)$。

请求出满足以下所有条件的整数对 $(i, j)$ 的数量：

* $1 \le i, j \le N$
* $f(A_i, A_j)$ 是 $M$ 的倍数

### 制约条件

* $1 \le N \le 2 \times 10^5$
* $1 \le M \le 10^9$
* $1 \le A_i \le 10^9$
* 所有输入值均为整数

---

### 输入

输入按以下格式从标准输入给出：

> $N$ $M$  
> $A_1$ $A_2$ $\dots$ $A_N$

### 输出

输出满足所有条件的整数对 $(i, j)$ 的数量。

---

### 样例输入 1
```text
2 11
2 42
```

### 样例输出 1
```text
2
```

**样例 1 解释：**
* 当 $(i, j) = (1, 1)$ 时：$f(A_1, A_1) = 22$ 是 $11$ 的倍数。
* 当 $(i, j) = (1, 2)$ 时：$f(A_1, A_2) = 242$ 是 $11$ 的倍数。
* 当 $(i, j) = (2, 1)$ 时：$f(A_2, A_1) = 422$ 不是 $11$ 的倍数。
* 当 $(i, j) = (2, 2)$ 时：$f(A_2, A_2) = 4242$ 不是 $11$ 的倍数。

综上所述，满足条件的整数对只有 $(i, j) = (1, 1), (1, 2)$ 这 $2$ 个，因此输出 $2$。

---

### 样例输入 2
```text
4 7
2 8 16 183
```

### 样例输出 2
```text
4
```

---

### 样例输入 3
```text
5 5
1000000000 1000000000 1000000000 1000000000 1000000000
```

### 样例输出 3
```text
25
```

---

### 样例输入 4
```text
12 13
80 68 862370 82217 8 56 5 168 672624 6 286057 11864
```

### 样例输出 4
```text
10
```


# AC Code 哈希
``` python
import itertools  
import sys  
from collections import Counter  
  
  
def main():  
      
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    cnt = [ Counter() for _ in range(11)]  
  
    # 预处理，这道题不要求 i < j    for x in arr:  
        k = len(str(x))  
        cnt[k][x % m] += 1  
  
    ans = 0  
  
    # 算特征值  
    for x in arr:  
        p = 10  
        for k in range(1, 11):  
  
            key = (-(x * p)) % m  
            ans += cnt[k][key]  
  
            p = (p * 10) % m  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```

# 暴力Code TLE 
``` python
import itertools
import sys
from itertools import product, repeat


def main():
    
    n, m = map(int, sys.stdin.readline().strip().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    # 尝试暴力
    cnt = 0
    for comb in itertools.product(range(n), repeat=2):

        x1, x2 = comb

        y = int(str(arr[x1]) + str(arr[x2]))

        if y % m == 0:
            cnt += 1

    print(cnt)

if __name__ == "__main__":
    main()
```