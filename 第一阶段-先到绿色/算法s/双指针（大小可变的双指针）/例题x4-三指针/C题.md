https://atcoder.jp/contests/abc430/tasks/abc430_c

---

### C - Truck Driver

时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 300 分

#### 问题描述

在 AtCoder 国，有一条规则：“卡车司机在驾驶 $A$ 分钟或以上时，必须进行 $B$ 分钟或以上的休息。”

给定一个由字符 `a` 和 `b` 组成的长度为 $N$ 的字符串 $S$，以及两个正整数 $A$ 和 $B$。请计算满足以下所有条件的整数对 $(l, r)$ 的数量。

* $1 \le l \le r \le N$
* $S$ 中从第 $l$ 个字符到第 $r$ 个字符（包含两端）中，字符 `a` 的出现次数不少于 $A$（即 $\ge A$）。
* $S$ 中从第 $l$ 个字符到第 $r$ 个字符（包含两端）中，字符 `b` 的出现次数小于 $B$（即 $< B$）。

#### 数据范围

* $1 \le N \le 3 \times 10^5$
* $1 \le A, B \le N$
* $S$ 是一个仅由 `a` 和 `b` 组成的长度为 $N$ 的字符串。
* 所有输入的数值均为整数。

#### 输入格式

输入按以下格式从标准输入给出：

$$N \quad A \quad B$$
$$S$$

#### 输出格式

输出满足条件的整数对 $(l, r)$ 的数量。

---

#### 输入样例 1
```text
11 4 2
abbaaabaaba
```

#### 输出样例 1
```text
3
```

**样例 1 说明：**  
满足条件的 $(l, r)$ 元组有 $(4, 8)$、$(4, 9)$ 和 $(5, 9)$，共 3 个。

---

#### 输入样例 2
```text
13 1 2
bbbbbbbbbbbbb
```

#### 输出样例 2
```text
0
```

**样例 2 说明：**  
不存在满足条件的 $(l, r)$ 元组。

# 三指针 Code AC
``` python
import sys  
  
def main():  
    input = sys.stdin.readline  
  
    N, A, B = map(int, input().split())  
    S = input().strip()  
  
    ans = 0  
  
    ra = 0  
    rb = 0  
    cnt_a = 0  
    cnt_b = 0  
  
    for l in range(N):  
        while ra < N and cnt_a < A:  
            if S[ra] == 'a':  
                cnt_a += 1  
            ra += 1  
  
        while rb < N and cnt_b < B:  
            if S[rb] == 'b':  
                cnt_b += 1  
            rb += 1  
  
        if cnt_a >= A:  
            if cnt_b >= B:  
                ans += max(0, rb - ra)  
            else:  
                ans += N + 1 - ra  
  
        if l < ra and S[l] == 'a':  
            cnt_a -= 1  
        if l < rb and S[l] == 'b':  
            cnt_b -= 1  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```

# 暴力，双层for循环 TLE
``` python
import math
import sys
from collections import Counter


def main():

    n, a, b = map(int, sys.stdin.readline().strip().split())

    s = sys.stdin.readline().strip()

    # 暴力
    ans = 0
    for i in range(n):
        for j in range(i + 1, n + 1):
            s_sub = s[i:j]
            counter = Counter(s_sub)

            if counter['a'] >= a and counter['b'] < b :
                ans += 1

    print(ans)

if __name__ == "__main__":
    main()