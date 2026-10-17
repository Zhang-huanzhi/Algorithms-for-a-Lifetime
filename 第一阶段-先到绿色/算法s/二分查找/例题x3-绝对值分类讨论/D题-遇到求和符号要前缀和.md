https://atcoder.jp/contests/abc437/tasks/abc437_d

---

### D - Sum of Differences

运行时间限制: 2 秒 / 内存限制: 1024 MiB

配点: 400 分

#### 问题描述

给你一个长度为 $N$ 的正整数序列 $A = (A_1, A_2, \dots, A_N)$ 和一个长度为 $M$ 的正整数序列 $B = (B_1, B_2, \dots, B_M)$。

请计算下式的值模 $998244353$ 的余数：

$$\sum_{i=1}^{N} \sum_{j=1}^{M} |A_i - B_j|$$

#### 约束条件

- $1 \le N, M \le 3 \times 10^5$
- $1 \le A_i, B_j < 998244353$
- 输入的所有值均为整数。

#### 输入

输入按以下格式从标准输入给出：

$$N \quad M$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$
$$B_1 \quad B_2 \quad \dots \quad B_M$$

#### 输出

在一行中输出答案。

---

#### 输入样例 1

```text
4 2
1 6 9 2
3 1
```

#### 输出样例 1

```text
26
```

**样例 1 说明**  
答案为 $|1-3|+|1-1|+|6-3|+|6-1|+|9-3|+|9-1|+|2-3|+|2-1| = 2+0+3+5+6+8+1+1 = 26$。

---

#### 输入样例 2

```text
8 8
185991676 311812083 311812083 84357963 185991676 185991676 724020528 369175631
455049197 387671868 4361724 724020528 724020528 455049197 455049197 724020528
```

#### 输出样例 2

```text
529117255
```

# 前缀和 + 二分查找 AC
``` python
import sys  
from bisect import bisect, bisect_left, bisect_right  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
    MOD = 998244353  
  
    arr.sort()  
    pre = [0] * (n + 1)  
    for i in range(n):  
        pre[i + 1] = pre[i] + arr[i]  
  
    ans = 0  
  
    for num in brr:  
  
        # arr 小于 num的 idx        idx = bisect_left(arr, num)  
  
        sum_less = pre[idx]  
        contrib_less = idx * num - sum_less  
  
        sum_greater = pre[n] - pre[idx]  
        contrib_greater = sum_greater - (n - idx) * num  
  
        ans = (ans + contrib_greater + contrib_less) % MOD  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```