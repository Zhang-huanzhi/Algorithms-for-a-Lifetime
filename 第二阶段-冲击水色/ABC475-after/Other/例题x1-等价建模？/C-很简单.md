https://atcoder.jp/contests/abc484/tasks/abc484_c

---
**C - Move One Card**

**执行时间限制**：2 sec / **内存限制**：1024 MiB

**分值**：300 分

### 问题描述

给定两个由正整数组成的长度为 $N$ 的序列 $A = (A_1, A_2, \dots, A_N)$ 和 $B = (B_1, B_2, \dots, B_N)$。

对序列 $A$ **恰好进行 $1$ 次**如下操作：

从 $A$ 中选择一个元素取出，并插入到剩余序列的任意位置。更严格地说，按顺序执行以下 $2$ 个步骤：
1. 选择一个满足 $1 \le i \le N$ 的整数 $i$，令长度为 $N-1$ 的序列 $X = (X_1, X_2, \dots, X_{N-1})$ 为 $(A_1, A_2, \dots, A_{i-1}, A_{i+1}, A_{i+2}, \dots, A_N)$，并令整数 $Y$ 为 $A_i$。
2. 选择一个满足 $1 \le j \le N$ 的整数 $j$，将长度为 $N$ 的序列 $(X_1, X_2, \dots, X_{j-1}, Y, X_j, X_{j+1}, \dots, X_{N-1})$ 作为新的 $A$。

请注意，在插入元素时，可以插入到序列的最左端或最右端。

请判断操作之后能否使 $A$ 与 $B$ 一致。

---

### 约束条件
- $1 \le N \le 2 \times 10^5$
- $1 \le A_i \le N \ (1 \le i \le N)$
- $1 \le B_i \le N \ (1 \le i \le N)$
- 所有输入均为整数

---

### 输入
输入按以下格式从标准输入给出：
```text
N
A_1 A_2 ... A_N
B_1 B_2 ... B_N
```

### 输出
如果恰好进行 $1$ 次操作后能使 $A$ 与 $B$ 一致，在一行中输出 `Yes`；否则输出 `No`。

---

### 样例输入 1
```text
6
1 4 1 4 2 1
1 4 1 1 4 2
```

### 样例输出 1
```text
Yes
```
例如，取出 $A_6$，将其插入到剩余序列从头数第 $3$ 项和第 $4$ 项之间，操作后的 $A$ 可以变为 $(1, 4, 1, 1, 4, 2)$。

因此，请输出 `Yes`。

---

### 样例输入 2
```text
2
1 1
1 2
```

### 样例输出 2
```text
No
```
无论对 $A$ 进行怎样的操作，都无法使其与 $B$ 一致。

---

### 样例输入 3
```text
3
1 2 3
1 2 3
```

### 样例输出 3
```text
Yes
```
将取出的元素插回它原本所在的位置也是可以的。

---

### 样例输入 4
```text
30
25 11 21 15 30 20 17 19 17 21 10 13 22 10 22 14 30 25 17 25 11 19 27 27 15 18 16 16 20 16
25 11 15 30 20 17 19 17 21 10 13 22 10 22 14 30 25 17 25 11 19 27 27 15 21 18 16 16 20 16
```

### 样例输出 4
```text
Yes
```


---

# AC Code
``` python
import sys
from collections import Counter


def main():

    n = int(sys.stdin.readline().strip())

    arr = list(map(int, sys.stdin.readline().strip().split()))
    brr = list(map(int, sys.stdin.readline().strip().split()))

    counterA = Counter(arr)
    counterB = Counter(brr)

    if counterA != counterB:
        print("No")
        return

    if arr == brr:
        print("Yes")
        return
    
    x = 0
    while arr[x] == brr[x]:
        x += 1

    y = n - 1
    while arr[y] == brr[y]:
        y -= 1

    if arr[x] == brr[y] and arr[x + 1 : y + 1] == brr[x : y]:
        print("Yes")
    elif arr[y] == brr[x] and arr[x : y] == brr[x + 1 : y + 1]:
        print("Yes")
    else:
        print("No")
if __name__ == "__main__":
    main()0
```