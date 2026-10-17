https://atcoder.jp/contests/abc466/tasks/abc466_e

---

### E - Range Flip  
时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 450 分  

#### 问题描述
有 $N$ 张卡片排成一排。卡片的编号为 $1, 2, \dots, N$。

卡片 $i$ 的正面写有整数 $A_i$，反面写有整数 $B_i$。初始时，所有卡片都是正面朝上。

你可以进行以下操作最多 $K$ 次：

* 选择满足 $1 \le l \le r \le N$ 的两个整数 $l, r$。
* 对于所有满足 $l \le i \le r$ 的整数 $i$，翻转卡片 $i$。这里，翻转卡片是指将操作前朝下的那一面翻转为朝上。

请问在操作结束后，每张卡片朝上的那一面上所写数字的总和最大可以是多少？

#### 限制
* $1 \le N \le 2 \times 10^5$
* $1 \le K \le 10$
* $1 \le A_i, B_i \le 10^9$
* 输入的所有值均为整数。

#### 输入
输入按以下格式从标准输入给出：

$$N \quad K$$
$$A_1 \quad B_1$$
$$A_2 \quad B_2$$
$$\vdots$$
$$A_N \quad B_N$$

#### 输出
输出一个整数，表示可能的最大总和。

---

#### 样例输入 1
```text
7 2
2 1
6 9
3 5
9 2
4 8
7 4
5 6
```

#### 样例输出 1
```text
45
```
**样例 1 说明：**
* 第一次操作选择 $l=2, r=5$，
* 第二次操作选择 $l=4, r=4$，

操作结束后，各卡片朝上的数按卡片编号顺序依次为 $2, 9, 5, 9, 8, 7, 5$，其和为 $45$。

#### 样例输入 2
```text
5 6
9 6
3 2
8 1
7 5
8 4
```

#### 样例输出 2
```text
35
```
**样例 2 说明：**
也可以一次操作都不进行。

#### 样例输入 3
```text
9 1
2 7
9 4
1 1
6 1
3 4
8 9
1 2
7 5
3 9
```

#### 样例输出 3
```text
47
```

# AC code
``` python
import math  
import sys  
  
  
def main():  
  
    n, k = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
    for _ in range(n):  
        a, b = map(int, sys.stdin.readline().strip().split())  
        arr.append((a, b))  
  
    m = 2 * k + 1  
  
    dp = [-math.inf] * (m + 1)  
  
    dp[1] = 0  
  
    for a, b in arr:  
        for j in range(2, m + 1):  
            if dp[j - 1] > dp[j]:  
                dp[j] = dp[j - 1]  
  
        for j in range(1, m + 1):  
            if j % 2 == 1:  
                dp[j] += a  
            else:  
                dp[j] += b  
  
    print(max(dp[1 : m + 1]))  
  
  
if __name__ == "__main__":  
    main()
```