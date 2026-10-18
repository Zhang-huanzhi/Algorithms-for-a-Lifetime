https://atcoder.jp/contests/abc476/tasks/abc476_c

---

### C - 第三小的数 / Third Largest Number

运行时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 300 分

#### 题目描述

给定一个大于等于 $3$ 的整数 $N$ 和一个长度为 $N$ 的正整数序列 $A = (A_1, A_2, \dots, A_N)$。

请对每个 $k = 3, 4, \dots, N$ 分别解决以下问题：

* 将 $A_1, A_2, \dots, A_k$ **按降序（从大到小）** 排序后，求排在第 $3$ 位的值（即第 3 大的值）。

#### 数据范围

* $3 \le N \le 5 \times 10^5$
* $1 \le A_i \le 10^9$
* 输入的所有值均为整数。

#### 输入格式

输入通过标准输入按以下格式给出：

> $N$  
> $A_1$ $A_2$ $\dots$ $A_N$

#### 输出格式

输出 $N - 2$ 行。

第 $i$ 行（$1 \le i \le N - 2$）输出 $k = i + 2$ 时的答案。

---

#### 输入样例 1

```text
5
1 2 1 2 3
```

#### 输出样例 1

```text
1
1
2
```

**样例说明 1：**
* 当 $k=3$ 时：将 $A_1, A_2, A_3$ 降序排序后为 $2, 1, 1$。第 3 个数为 $1$，因此第 1 行输出 $1$。
* 当 $k=4$ 时：将 $A_1, A_2, A_3, A_4$ 降序排序后为 $2, 2, 1, 1$。第 3 个数为 $1$，因此第 2 行输出 $1$。
* 当 $k=5$ 时：将 $A_1, A_2, A_3, A_4, A_5$ 降序排序后为 $3, 2, 2, 1, 1$。第 3 个数为 $2$，因此第 3 行输出 $2$。

---

#### 输入样例 2

```text
10
1 1 1 3 2 5 4 3 6 5
```

#### 输出样例 2

```text
1
1
1
2
3
3
4
5
```

---

#### 输入样例 3

```text
10
11 9 1 3 17 19 10 19 17 3
```

#### 输出样例 3

```text
1
3
9
11
11
17
17
17
```

# AC 动态维护
``` python
import sys

def main():

    n = int(sys.stdin.readline().strip())
    arr = list(map(int, sys.stdin.readline().strip().split()))

    # 这是，前缀集合？第三个是什么鬼？
    a, b, c = sorted(arr[:3], reverse=True)

    outs = [c]
    for i in range(3, n):

        num = arr[i]

        if num > a:
            a, b, c = num, a, b
        elif num > b:
            b, c = num, b
        elif num > c:
            c = num

        outs.append(c)

    print('\n'.join(map(str, outs)))

if __name__ == "__main__":
    main()
```

# AC 最小堆
``` python
import heapq  
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    brr = arr[:3]  
    heapq.heapify(brr)  
  
    outs = [brr[0]]  
  
    for i in range(3, n):  
  
        num = arr[i]  
  
        if num > brr[0]:  
            heapq.heappop(brr)  
            heapq.heappush(brr, num)  
  
        outs.append(brr[0])  
  
    print('\n'.join(map(str, outs)))  
  
if __name__ == "__main__":  
    main()
```