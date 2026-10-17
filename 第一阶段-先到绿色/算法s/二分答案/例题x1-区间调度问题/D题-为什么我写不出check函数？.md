https://atcoder.jp/contests/abc463/tasks/abc463_d

---

### D - Maximize the Gap  
运行时间限制: 2 sec / 内存限制: 1024 MiB

分值 : 400 分

#### 问题描述
数轴上有 $N$ 块布。第 $i$ 块布（$1 \le i \le N$）覆盖了数轴上的闭区间 $[L_i, R_i]$。数轴上的点可能被两块或更多块布覆盖，也可能不被任何布覆盖。

两块布重叠是指：数轴上存在某个点，同时被这两块布覆盖。

对于互不重叠的两块布，它们之间的距离定义如下：
* 分别任取一块布覆盖的点 $p$ 和另一块布覆盖的点 $q$，它们之间距离 $|p-q|$ 的最小值。

对于选出的任意两块都不重叠的 $K$ 块布，我们将它们的“得分”定义为这些布两两之间距离的最小值。请在 $N$ 块布中选择 $K$ 块布，使得这 $K$ 块布中任意两块都不重叠，并使此时的得分最大。

若无法选择出满足条件的 $K$ 块布，请输出 `-1`。

#### 限制条件
* $2 \le K \le N \le 2 \times 10^5$
* $0 \le L_i < R_i \le 10^9 \quad (1 \le i \le N)$
* 输入均为整数。

#### 输入格式
输入按以下格式从标准输入给出：

$$
N \quad K \\
L_1 \quad R_1 \\
L_2 \quad R_2 \\
\vdots \\
L_N \quad R_N
$$

#### 输出格式
输出答案。

---

#### 输入样例 1
```text
6 3
1 12
2 7
5 9
9 13
10 18
15 20
```

#### 输出样例 1
```text
2
```
**样例说明 1：**  
如果选择第 $2$ 块、第 $4$ 块和第 $6$ 块布，它们之中任意两块都不重叠。第 $2$ 块布与第 $4$ 块布的距离为 $2$，第 $2$ 块布与第 $6$ 块布的距离为 $8$，第 $4$ 块布与第 $6$ 块布的距离为 $2$，因此这种选择方式的得分为 $2$。

由于无法选择 $3$ 块布使得得分达到 $3$ 或以上，因此输出 $2$。

---

#### 输入样例 2
```text
2 2
1 5
5 9
```

#### 输出样例 2
```text
-1
```
**样例说明 2：**  
给定的两块布有重叠，因此无法选出两块互不重叠的布。故输出 `-1`。  
请注意，第 $1$ 块布和第 $2$ 块布在单点 $5$ 处重叠。

---

#### 输入样例 3
```text
20 5
169 748
329 586
529 972
432 520
408 587
138 250
114 656
299 632
755 984
404 772
155 506
832 854
353 465
374 387
384 567
555 631
428 951
104 705
405 530
102 258
```

#### 输出样例 3
```text
35
```


# 暴力Code
``` python
import heapq
import math
import sys
from itertools import combinations


def main():

    n, k = map(int, sys.stdin.readline().strip().split())

    arr = []

    for i in range(n):
        l, r = map(int, sys.stdin.readline().strip().split())
        arr.append((l, r))

    # 算了，先暴力吧。
    ans = -1
    for combin in combinations(arr, k):
        combin = list(combin)
        combin.sort()

        dist = math.inf
        for i in range(1, k):
            curr = combin[i][0] - combin[i-1][1]
            dist = min(dist, curr)

        if dist <= 0:
            continue

        ans = max(dist, ans)

    if ans == math.inf:
        print(-1)
    else:
        print(ans)
if __name__ == '__main__':
    main()
```

# 二分答案
``` python
import heapq
import math
import sys
from itertools import combinations

def main():

    n, k = map(int, sys.stdin.readline().strip().split())

    arr = []

    for i in range(n):
        l, r = map(int, sys.stdin.readline().strip().split())
        arr.append((l, r))

    arr.sort(key = lambda x:(x[1]))

    # 二分答案
    def check(x):
        count = 0
        last_r = -math.inf
        for l, r in arr:
            if l >= last_r + x:
                count += 1
                last_r = r
                if count >= k:
                    return True
        return False

    low = 1
    high = 10 ** 9
    ans = - 1

    while low <= high:
        mid = (low + high) // 2

        if check(mid):
            ans = mid
            low = mid + 1
        else:
            high = mid - 1

    print(ans)

if __name__ == '__main__':
    main()
```