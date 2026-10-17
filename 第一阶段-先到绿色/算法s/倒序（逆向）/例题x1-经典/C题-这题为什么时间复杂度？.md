https://atcoder.jp/contests/abc474/tasks/abc474_c

---

### C - Remove and Append

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：300 分

#### 题目描述
给你一个 $(1, 2, \dots, N)$ 的排列 $P = (P_1, P_2, \dots, P_N)$。  
请按顺序对于 $q = 1, 2, \dots, Q$ 执行以下操作：

* 从 $P$ 中删除值为 $a_q$ 的元素，并将其添加到 $P$ 的末尾。

请输出执行完全部 $Q$ 次操作后，$P$ 中各个元素的值。

#### 数据范围
* $1 \le N \le 2 \times 10^5$
* $1 \le Q \le 2 \times 10^5$
* $(P_1, P_2, \dots, P_N)$ 是 $(1, 2, \dots, N)$ 的一个排列
* $1 \le a_q \le N$
* 输入的所有数值均为整数

#### 输入格式
输入按以下格式从标准输入给出：

$$
N \quad Q \\
P_1 \quad P_2 \quad \dots \quad P_N \\
a_1 \\
a_2 \\
\vdots \\
a_Q
$$

#### 输出格式
在一行中输出 $Q$ 次操作完成后的 $P_1, P_2, \dots, P_N$，各元素之间用空格隔开。

---

### 样例说明

#### 样例 1
**输入**
```text
4 2
2 4 3 1
3
2
```
**输出**
```text
4 1 3 2
```
**说明**  
最初，$P = (2, 4, 3, 1)$。  
第 1 次操作后，将 $3$ 移到末尾，变为 $P = (2, 4, 1, 3)$。  
第 2 次操作后，将 $2$ 移到末尾，变为 $P = (4, 1, 3, 2)$。

#### 样例 2
**输入**
```text
3 3
1 2 3
1
1
1
```
**输出**
```text
2 3 1
```

#### 样例 3
**输入**
```text
2 5
2 1
1
1
2
2
1
```
**输出**
```text
2 1
```

---

# AC Code
``` python
import sys

def main():

    n, q = map(int, sys.stdin.readline().strip().split())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    brr = []
    for _ in range(q):
        x = int(sys.stdin.readline().strip())
        brr.append(x)
        # 好眼熟的题

    brr.reverse()
    memo = set()
    crr = []
    for x in brr:
        if x not in memo:
            memo.add(x)
            crr.append(x)

    crr.reverse()
    drr = []
    for num in arr:
        if num not in memo:
            drr.append(num)

    out = drr + crr
    print(*out)

if __name__ == "__main__":
    main()
```