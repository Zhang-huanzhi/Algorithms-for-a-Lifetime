
https://atcoder.jp/contests/abc468/tasks/abc468_f

---


### F - Chmax  
时间限制: 2 秒 / 内存限制: 1024 MiB

分值：500 分

#### 题目描述
给定一个正整数 $N$ 和一个 $(1, 2, \dots, N)$ 的排列 $P = (P_1, P_2, \dots, P_N)$。

有三个变量 $x, y, c$，初始时 $x = y = c = 0$。

你需要依次对于 $k = 1, 2, \dots, N$ 执行以下两种操作之一：

* **操作 1**：如果 $x < P_k$，则将 $c$ 增加 $1$。随后，将 $x$ 替换为 $\max(x, P_k)$。
* **操作 2**：如果 $y < P_k$，则将 $c$ 增加 $1$。随后，将 $y$ 替换为 $\max(y, P_k)$。

请计算最终 $c$ 的最大可能值。

#### 限制
* $1 \le N \le 5 \times 10^5$
* $P$ 是 $(1, 2, \dots, N)$ 的一个排列。
* 输入的所有值均为整数。

#### 输入
输入按以下格式从标准输入给出：
> $N$  
> $P_1$ $P_2$ $\dots$ $P_N$

#### 输出
输出最终 $c$ 的最大可能值。

---

#### 输入样例 1
```text
5
4 3 1 2 5
```

#### 输出样例 1
```text
4
```
**样例说明 1**：  
可以通过以下操作达到 $c = 4$：
* 当 $k=1$ 时：执行操作 1。状态变为 $(x, y, c) = (4, 0, 1)$。
* 当 $k=2$ 时：执行操作 1。状态变为 $(x, y, c) = (4, 0, 1)$。
* 当 $k=3$ 时：执行操作 2。状态变为 $(x, y, c) = (4, 1, 2)$。
* 当 $k=4$ 时：执行操作 2。状态变为 $(x, y, c) = (4, 2, 3)$。
* 当 $k=5$ 时：执行操作 2。状态变为 $(x, y, c) = (4, 5, 4)$。

无论如何操作，都无法使 $c$ 大于 $4$，因此输出 $4$。

#### 输入样例 2
```text
6
6 5 4 3 2 1
```

#### 输出样例 2
```text
2
```

#### 输入样例 3
```text
9
3 6 5 2 7 8 9 1 4
```

#### 输出样例 3
```text
7
```


# 贪心Code Wa
``` python
import itertools
import sys

def main():
    
    n = int(sys.stdin.readline().strip())
    prr = list(map(int, sys.stdin.readline().strip().split()))

    x, y, c = 0, 0, 0

    # 要让x,y 尽可能变小
    # 贪心呗， 谁相差小就加谁
    for k in range(n):

        abs_x = abs(x - prr[k])
        abs_y = abs(y - prr[k])

        if abs_x <= abs_y:
            if x < prr[k]:
                c += 1
                x = max(x, prr[k])

        if abs_x > abs_y:
            if y < prr[k]:
                c += 1
                y = max(y, prr[k])

    print(c)


if __name__ == "__main__":
    main()
```