https://atcoder.jp/contests/abc470/tasks/abc470_d

---

# D - Inverse and Swap

时间限制：2 秒 / 内存限制：1024 MiB

分值：400 分

### 题目描述

给定一个 $(1, \dots, N)$ 的排列 $P = (P_1, \dots, P_N)$。

请按顺序处理 $Q$ 个操作（查询）。操作分为以下 2 种类型：

- `1 x y`：交换 $P_x$ 和 $P_y$ 的值。
- `2`：构造一个满足以下条件的 $(1, \dots, N)$ 的排列 $P' = (P'_1, \dots, P'_N)$，并将 $P_1, \dots, P_N$ 的值分别替换为 $P'_1, \dots, P'_N$。（可以证明满足条件的 $P'$ 是唯一存在的。）
  - 对于所有满足 $1 \le i \le N$ 的整数 $i$，都有 $P_{P'_i} = i$。

请输出处理完所有操作后 $P_1, \dots, P_N$ 的最终值。

### 制约条件

- $2 \le N \le 5 \times 10^5$
- $1 \le Q \le 5 \times 10^5$
- $(P_1, \dots, P_N)$ 是 $(1, \dots, N)$ 的一个排列
- 在类型 1 的操作中，$1 \le x < y \le N$
- 所有输入值均为整数

### 输入格式

输入按以下格式从标准输入给出：

$$
N \quad Q \\
P_1 \quad P_2 \quad \dots \quad P_N \\
\text{query}_1 \\
\vdots \\
\text{query}_Q
$$

其中 $\text{query}_q$ 表示第 $q$ 个操作，格式为以下两种之一：

```text
1 x y
```

```text
2
```

### 输出格式

在一行中输出处理完所有操作后的 $P_1, \dots, P_N$ 的值，数值之间用空格分隔。

---

### 样例 1 输入
```text
5 5
2 1 3 5 4
1 2 4
2
1 2 3
1 3 4
2
```

### 样例 1 输出
```text
4 5 2 1 3
```

**样例 1 解释：**
在处理完各个操作后，$P_1, \dots, P_N$ 的状态如下：
- 处理完第 1 个操作后：$P=(2,5,3,1,4)$
- 处理完第 2 个操作后：$P=(4,1,3,5,2)$
- 处理完第 3 个操作后：$P=(4,3,1,5,2)$
- 处理完第 4 个操作后：$P=(4,3,5,1,2)$
- 处理完第 5 个操作后：$P=(4,5,2,1,3)$

---

### 样例 2 输入
```text
7 4
3 7 5 6 4 2 1
2
2
2
2
```

### 样例 2 输出
```text
3 7 5 6 4 2 1
```

---

### 样例 3 输入
```text
10 8
7 3 2 4 8 5 10 9 1 6
2
1 4 10
1 6 9
2
1 9 10
1 3 10
2
1 4 6
```

### 样例 3 输出
```text
3 10 2 8 6 7 1 5 9 4
```

# AC Code
``` python
import sys  
from collections import Counter, defaultdict  
  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    arr = [ arr[x] - 1 for x in range(n)]  
    brr = [0] * n  
  
    for i in range(n):  
        brr[arr[i]] = i  
  
    state = False  
  
    for _ in range(q):  
  
        line = list(sys.stdin.readline().strip().split())  
  
        op = line[0]  
  
        if op == '1':  
            x = int(line[1]) - 1  
            y = int(line[2]) - 1  
  
            if not state:  
                vx, vy = arr[x], arr[y]  
  
                arr[x], arr[y] = vy, vx  
  
                brr[vx], brr[vy] = y, x  
  
            else:  
                vx, vy = brr[x], brr[y]  
  
                brr[x], brr[y] = vy, vx  
  
                arr[vx], arr[vy] = y, x  
  
        if op == '2':  
  
            state = not state  
  
  
    ans = brr if state else arr  
  
    print(*(v + 1 for v in ans))  
  
  
  
  
if __name__ == "__main__":  
    main()
```


# 暴力Code TLE
``` python
import sys
from collections import Counter, defaultdict


def main():

    n, q = map(int, sys.stdin.readline().strip().split())

    prr = list(map(int, sys.stdin.readline().strip().split()))
    arr = dict()
    for i in range(n):
        arr[prr[i]] = i + 1

    arr = dict(sorted(arr.items()))
    # print(arr)
    for _ in range(q):

        line = list(sys.stdin.readline().strip().split())

        op = line[0]

        if op == '1':
            x = int(line[1])
            y = int(line[2])

            prr[x - 1], prr[y - 1] = prr[y - 1], prr[x - 1]
            arr[prr[x - 1]], arr[prr[y - 1]] = arr[prr[y - 1]], arr[prr[x - 1]]


        if op == '2':
            prr = list(arr.values())
            for i in range(n):
                arr[prr[i]] = i + 1



    print(*prr)

if __name__ == "__main__":
    main()
```