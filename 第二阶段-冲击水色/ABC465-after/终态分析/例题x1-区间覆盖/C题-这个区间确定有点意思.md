https://atcoder.jp/contests/abc478/tasks/abc478_c

---

### C - Sort Subarray

运行时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 300 分

#### 题目描述
给你一个长度为 $N$ 的整数序列 $A = (A_1, A_2, \dots, A_N)$。

你可以对 $A$ 执行以下操作**恰好一次**：
- 选择一个满足 $1 \le i \le N - K + 1$ 的整数 $i$。
- 将 $A_i, A_{i+1}, \dots, A_{i+K-1}$ 按升序（从小到大）排序。更严谨地说，将 $A_i, A_{i+1}, \dots, A_{i+K-1}$ 从小到大排序后的结果记为 $B_1, B_2, \dots, B_K$，然后对于所有 $1 \le j \le K$，同时将 $A_{i+j-1}$ 替换为 $B_j$。

请判断：能否通过这次操作，使得序列 $A$ 最终变为升序（即对所有 $1 \le i < N$，均满足 $A_i \le A_{i+1}$）？

#### 制约条件
- $1 \le K < N \le 2 \times 10^5$
- $1 \le A_i \le N$ ($1 \le i \le N$)
- 输入中的所有数值均为整数。

#### 输入格式
输入按以下格式从标准输入给出：
> $N$ $K$  
> $A_1$ $A_2$ $\dots$ $A_N$  

#### 输出格式
如果能通过恰好一次操作使 $A$ 变为升序，输出 `Yes`；否则，输出 `No`。

---

### 样例 1
#### 输入样例 1
```plain
9 6
1 4 1 4 2 1 3 5 6
```
#### 输出样例 1
```plain
Yes
```
**样例说明 1**  
例如，选择 $i = 2$，则子数组 $(A_2, A_3, A_4, A_5, A_6, A_7) = (4, 1, 4, 2, 1, 3)$ 将被替换为 $(1, 1, 2, 3, 4, 4)$。  
操作后，序列 $A$ 变为 $(1, 1, 1, 2, 3, 4, 4, 5, 6)$，满足升序条件。  
因此输出 `Yes`。

---

### 样例 2
#### 输入样例 2
```plain
3 1
3 2 1
```
#### 输出样例 2
```plain
No
```
**样例说明 2**  
由于 $K = 1$，无论选择哪个 $i$，序列 $A$ 都不会发生变化。因此输出 `No`。

---

### 样例 3
#### 输入样例 3
```plain
30 25
1 2 2 22 14 10 14 10 18 5 15 8 17 22 10 17 11 25 13 16 9 19 26 7 11 12 23 3 30 30
```
#### 输出样例 3
```plain
Yes
```


# AC Code
``` python
import itertools
import sys
import math
from collections import deque


def main():

    n, k = map(int, sys.stdin.readline().strip().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    # 贪心呗， 遇到的第一个去模拟
    # 不行， 贪心不对，终态分析吧，反正最后要变成一个样

    brr = sorted(arr)

    # print(brr)
    l, r = n , -1

    for i in range(n):
        if arr[i] != brr[i]:
            if l == n:
                l = i
            r = i

    # print(r, l)
    if  l == n and r == -1:
        print("Yes")
    elif r - l + 1 <= k:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    main()
```