https://atcoder.jp/contests/abc480/tasks/abc480_c

---


### C - 自动停止的淋浴 (Self-Stopping Shower)

时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 300 分

#### 题目描述

高桥正在钱汤（日式澡堂）使用淋浴。这个淋浴器上配有一个按钮。在时刻 $t$ 按下按钮后，热水会在时刻 $t$ 到时刻 $t+K$ 之间的 $K$ 秒内持续流出。如果在热水流出期间再次按下按钮，热水也会从最后一次按下按钮的时刻起，重新持续流出 $K$ 秒。

现在有 $N$ 个青木君，每个人都会按一次按钮。第 $i$ 个青木君会在时刻 $A_i$ 按下按钮。

为了让淋浴在时刻 $0$ 到时刻 $T$ 之间始终有热水流出，高桥君最少需要自己按下多少次按钮？  
注意：按下按钮的瞬间以及最后一次按下按钮后恰好经过 $K$ 秒的瞬间，均视为有热水流出。

#### 数据范围

- $1 \le N \le 2 \times 10^5$
- $1 \le K \le 10^9$
- $1 \le T \le 10^9$
- $0 \le A_i \le 10^9$
- 所有输入的值均为整数。

#### 输入格式

输入通过标准输入按以下格式给出：

$$
N \quad K \quad T  
$$
$$
A_1 \quad A_2 \quad \dots \quad A_N
$$

#### 输出格式

输出一行，表示高桥君最少需要按按钮的次数。

---

#### 样例 1

**输入**
```text
3 4 20
6 7 12
```

**输出**
```text
4
```

**样例说明**  
高桥君可以在时刻 $0, 2, 10, 16$ 按下按钮，这样就能保证在时刻 $0$ 到时刻 $20$ 期间一直有热水流出。  
如果按按钮的次数不超过 $3$ 次，则无法做到一直出水，因此最少需要按 $4$ 次。

---

#### 样例 2

**输入**
```text
9 37 480
12 123 132 213 231 312 321 500 1000
```

**输出**
```text
11
```

# 排序 + 贪心 AC
``` python
import sys
import math
from collections import defaultdict


def main():

    n, k, t = map(int, sys.stdin.readline().strip().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))
    arr.sort()

    # k, t 这么大， 区间覆盖？

    brr = []
    for start in arr:
        brr.append((start, start + k))

    # print(brr)
    # 算算哪些区间是没有覆盖的。
    crr = []
    left = 0
    right = 0
    for start, end in brr:

        if left  < start:
            crr.append((left, start))
            left = end

        left = max(left, end)

    if left < t:
        crr.append((left, t))

    # print(crr)

    ans = 0
    for x, y in crr:

        if x > t:
            break

        if y > t:
            y = t

        diff = y - x

        if diff % k == 0:
            ans += diff // k
        else:
            ans += diff // k + 1

    print(ans)


if __name__ == "__main__":
    main()
```

# 离散 + 差分 
``` python
import sys
import math
from bisect import bisect_left


def main():

    n, k, t = map(int, sys.stdin.readline().strip().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    points = {0, t}

    for x in arr:
        if x < t:
            points.add(x)
            points.add(min(x + k, t))

    coords = sorted(points)
    m = len(coords)

    def get_id(val):
        return bisect_left(coords, val)

    diff = [0] * m
    for x in arr:
        if x < t:
            l = get_id(x)
            r = get_id(min(x + k, t))

            diff[l] += 1
            diff[r] -= 1

    ans = 0
    curr_water = 0
    gap_len = 0

    for i in range(m - 1):
        curr_water += diff[i]
        real_len = coords[i + 1] - coords[i]

        if curr_water == 0:
            gap_len += real_len
        else:
            if gap_len > 0:
                ans += (gap_len + k - 1) // k
                gap_len = 0

    if gap_len > 0:
        ans += (gap_len + k - 1) // k

    print(ans)


if __name__ == "__main__":
    main()
```