https://atcoder.jp/contests/abc464/tasks/abc464_c

---
### C - Plumage Palette

**时间限制**: 2 秒 / **内存限制**: 1024 MiB

**分值**: 300 分

#### 问题描述

高桥君在 $M$ 天里观察了编号为 $1, 2, \dots, N$ 的 $N$ 只鸟。

每一只被观察的鸟都具有颜色 $1, 2, \dots, N$ 这 $N$ 种颜色中的某一种。这些鸟具有一个有趣的特征：它们的颜色在观察期间可能会发生变化。

具体来说，第 $i$ 只鸟在第 $D_i - 1$ 天及以前的观察中颜色为 $A_i$，而在第 $D_i$ 天及以后的观察中颜色变为了 $B_i$。

需要注意的是：
* 如果 $D_i = 1$，则该鸟自第 $1$ 天起颜色就为 $B_i$。
* 如果 $A_i = B_i$，则该鸟在整个观察期间颜色没有发生变化。

对于每个 $j = 1, 2, \dots, M$，请计算在第 $j$ 天的观察中，所有鸟的颜色一共有多少种。

#### 限制条件

* $1 \le N \le 3 \times 10^5$
* $1 \le M \le 3 \times 10^5$
* $1 \le A_i, B_i \le N$
* $1 \le D_i \le M$
* 所有输入均为整数

#### 输入格式

输入按以下格式从标准输入给出：

$$N \quad M$$
$$A_1 \quad D_1 \quad B_1$$
$$A_2 \quad D_2 \quad B_2$$
$$\vdots$$
$$A_N \quad D_N \quad B_N$$

#### 输出格式

输出共 $M$ 行。
其中第 $j$ 行（$1 \le j \le M$）应当输出第 $j$ 天观察到鸟的颜色种类数。

---

#### 输入样例 1
```text
6 7
1 3 2
2 6 5
5 5 1
3 3 5
4 1 6
6 3 6
```

#### 输出样例 1
```text
5
5
3
3
4
4
4
```

在该输入中，在 $7$ 天内观察了 $6$ 只鸟。

* **第 1 天**观察时，各只鸟的颜色分别为 $1, 2, 5, 3, 6, 6$。鸟的颜色总计有 $5$ 种。
* **第 2 天**观察时，各只鸟的颜色分别为 $1, 2, 5, 3, 6, 6$。鸟的颜色总计有 $5$ 种。
* **第 3 天**观察时，各只鸟的颜色分别为 $2, 2, 5, 5, 6, 6$。鸟的颜色总计有 $3$ 种。
* **第 4 天**观察时，各只鸟的颜色分别为 $2, 2, 5, 5, 6, 6$。鸟的颜色总计有 $3$ 种。
* **第 5 天**观察时，各只鸟的颜色分别为 $2, 2, 1, 5, 6, 6$。鸟的颜色总计有 $4$ 种。
* **第 6 天**观察时，各只鸟的颜色分别为 $2, 5, 1, 5, 6, 6$。鸟的颜色总计有 $4$ 种。
* **第 7 天**观察时，各只鸟的颜色分别为 $2, 5, 1, 5, 6, 6$。鸟的颜色总计有 $4$ 种。

# Cde

``` python
import sys
import math
from collections import Counter,defaultdict

def main():

    n, m = map(int, sys.stdin.readline().strip().split())

    arr = [ [] for _ in range(m + 1)]
    brr = [0] * (n + 1)

    cnt = 0
    for i in range(n):
        a, d, b = map(int, sys.stdin.readline().strip().split())

        if d != 1:
            brr[a] += 1
            if brr[a] == 1:
                cnt += 1
        else:
            brr[b] += 1
            if brr[b] == 1:
                cnt += 1

        arr[d].append((a, b))

    print(cnt)

    # 模拟天数,从第二天算起
    for i in range(2, m + 1):

        # 有多少鸟会变化颜色
        for a, b in arr[i]:
            brr[a] -= 1
            if brr[a] == 0:
                cnt -= 1
            brr[b] += 1
            if brr[b] == 1:
                cnt += 1

        print(cnt)
if __name__ == '__main__':
    main()
```