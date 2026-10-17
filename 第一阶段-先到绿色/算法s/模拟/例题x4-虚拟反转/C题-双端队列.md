https://atcoder.jp/contests/abc465/tasks/abc465_c

---


### C - Reverse Permutation / 翻转排列

时间限制: 2 秒 / 内存限制: 1024 MiB
分值: 300 分

#### 问题描述

给定一个整数 $N$ 以及一个长度为 $N$ 且仅由字符 `o` 和 `x` 组成的字符串 $S$。

有一个长度为 $N$ 的整数序列 $A=(A_1, A_2, \dots, A_N)$。初始时，$A=(1, 2, \dots, N)$。

接下来，对于 $k=1, 2, \dots, N$，按顺序对 $A$ 执行以下操作：
* 如果 $S_k = \text{o}$，则将 $A$ 的前 $k$ 项进行翻转。具体来说，将 $A$ 替换为 $(A_k, A_{k-1}, \dots, A_1, A_{k+1}, A_{k+2}, \dots, A_N)$。
* 如果 $S_k = \text{x}$，则什么都不做。

请输出完成所有操作后的序列 $A$。

#### 数据范围

* $2 \le N \le 5 \times 10^5$
* $N$ 是整数
* $S$ 是一个长度为 $N$ 且仅由 `o` 和 `x` 组成的字符串

#### 输入格式

输入按以下格式从标准输入给出：

$$N$$
$$S$$

#### 输出格式

输出完成所有操作后的序列 $A$ 的各个元素，元素之间用空格分隔。

---

#### 输入样例 1
```text
5
ooxoo
```

#### 输出样例 1
```text
5 2 1 3 4
```

**样例 1 说明：**
序列 $A$ 在每次操作中的变化如下：
* 当 $k=1$ 时：翻转 $A$ 的前 1 项。$A=(1,2,3,4,5)$。
* 当 $k=2$ 时：翻转 $A$ 的前 2 项。$A=(2,1,3,4,5)$。
* 当 $k=3$ 时：什么都不做。
* 当 $k=4$ 时：翻转 $A$ 的前 4 项。$A=(4,3,1,2,5)$。
* 当 $k=5$ 时：翻转 $A$ 的前 5 项。$A=(5,2,1,3,4)$。

完成所有操作后的 $A$ 为 $(5,2,1,3,4)$。

#### 输入样例 2
```text
7
ooooooo
```

#### 输出样例 2
```text
7 5 3 1 2 4 6
```

#### 输入样例 3
```text
15
xooxoxoxoxoxxoo
```

#### 输出样例 3
```text
15 11 10 7 6 3 1 2 4 5 8 9 12 13 14
```

# AC Code 双端
``` python
import sys
import itertools
from collections import deque

def main():

    n = int(sys.stdin.readline().strip())

    s = sys.stdin.readline()

    arr = deque()
    state = False

    for i in range(n):
        num = i + 1

        if not state:
            arr.append(num)
            if s[i] == 'o':
                state = not state
        else:
            arr.appendleft(num)
            if s[i] == 'o':
                state = not state

    res = list(arr)
    if state:
        res.reverse()

    print(*res)

if __name__ == "__main__":
    main()
```

# AC Code 数组模拟，莫名其妙写的
``` python
import sys
import itertools
from collections import deque


def main():

    n = int(sys.stdin.readline().strip())

    s = sys.stdin.readline().strip()
    arr = []
    brr = []
    state = False

    for i in range(n):
        num = i + 1

        if state:
            arr.append(num)
        else:
            brr.append(num)

        if s[i] == 'o':
            state = not state
    
    # 是这样嘛？感觉有点怪
    if state:
        ans =  brr[::-1] + arr
    else:
        ans = arr[::-1] + brr

    print(*ans)


if __name__ == "__main__":
    main()
```