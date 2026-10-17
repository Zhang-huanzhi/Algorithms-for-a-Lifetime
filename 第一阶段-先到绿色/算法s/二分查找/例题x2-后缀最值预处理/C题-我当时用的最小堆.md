https://atcoder.jp/contests/abc463/tasks/abc463_c

---

### C - Tallest at the Moment  
运行时间限制: 2 sec / 内存限制: 1024 MiB

分值 : 300 分

#### 问题描述
当前，会议室里有 $N$ 个高桥君。第 $i$ 个高桥君（$1 \le i \le N$）的身高为 $H_i$，并将在距今 $L_i$ 分钟后离开会议室。一旦离开会议室，高桥君就不会再返回。

现给出 $Q$ 个查询，请依次回答。在第 $i$ 个查询（$1 \le i \le Q$）中，会给出一个整数 $T_i$。请回答在距今 $T_i + \frac{1}{2}$ 分钟后，依然留在会议室里的高桥君的身高最大值。在题目的限制条件下，保证在距今 $T_i + \frac{1}{2}$ 分钟后，会议室里至少还有 $1$ 个高桥君。

#### 限制条件
* $1 \le N \le 3 \times 10^5$
* $1 \le H_i \le 10^9 \quad (1 \le i \le N)$
* $1 \le L_1 \le L_2 \le \dots \le L_N \le 10^9$
* $1 \le Q \le 3 \times 10^5$
* $0 \le T_i < L_N \quad (1 \le i \le Q)$
* 输入均为整数。

#### 输入格式
输入按以下格式从标准输入给出：

$$
N \\
H_1 \quad L_1 \\
H_2 \quad L_2 \\
\vdots \\
H_N \quad L_N \\
Q \\
T_1 \quad T_2 \quad \dots \quad T_Q
$$

#### 输出格式
输出共 $Q$ 行。在第 $i$ 行（$1 \le i \le Q$）中，输出第 $i$ 个查询的答案。

---

#### 输入样例 1
```text
4
31 4
26 5
3 5
15 9
4
3 4 5 6
```

#### 输出样例 1
```text
31
26
15
15
```
**样例说明 1：**  
* 距今 $3 + \frac{1}{2}$ 分钟后，当前在会议室里的所有高桥君都还留在会议室里。因此，第 1 个查询（$T_1 = 3$）的答案是 $\{31, 26, 3, 15\}$ 中的最大值，即 $31$。
* 距今 $5 + \frac{1}{2}$ 分钟后，会议室里只有第 4 个高桥君。因此，第 3 个查询（$T_3 = 5$）的答案是 $\{15\}$ 中的最大值，即 $15$。

---

#### 输入样例 2
```text
10
587 138
772 155
755 404
519 408
529 432
169 586
114 632
249 656
329 972
299 984
14
443 801 824 276 399 314 300 510 311 580 498 930 359 5
```

#### 输出样例 2
```text
329
329
329
755
755
755
755
329
755
329
329
329
755
772
```

# 二分查找
``` python
import bisect  
import heapq  
import sys  
  
  
def main():  
    n = int(sys.stdin.readline().strip())  
  
    hrr = []  
    lrr = []  
  
    for i in range(n):  
        h, l = map(int, sys.stdin.readline().strip().split())  
        hrr.append(h)  
        lrr.append(l)  
  
    q = int(sys.stdin.readline().strip())  
  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 预处理后缀最大值  
    suf_max = [0] * (n + 1)  
    for i in range(n - 1, -1, -1):  
        suf_max[i] = max(hrr[i], suf_max[i + 1])  
  
    out = []  
  
    for t in brr:  
        idx = bisect.bisect_right(lrr, t)  
  
        out.append(str(suf_max[idx]))  
  
    print('\n'.join(out))  
  
  
if __name__ == '__main__':  
    main()
```

# 最小堆code
``` python
import heapq
import sys


def main():

    n = int(sys.stdin.readline().strip())

    arr = []

    for i in range(n):
        h, l = map(int, sys.stdin.readline().strip().split())
        arr.append((-h, l))

    q = int(sys.stdin.readline().strip())

    brr = list(map(int, sys.stdin.readline().strip().split()))

    crr = []
    # 对brr排序
    for i in range(q):
        crr.append((brr[i], i))

    crr.sort(key = lambda x: (x[0], x[1]))

    # print(crr)
    out = []
    # 最大堆
    heapq.heapify(arr)

    for t, idx in crr:

        while arr and arr[0][1] <= t:
            heapq.heappop(arr)

        out.append((-arr[0][0], idx))

    # print(out)

    out.sort(key= lambda x: (x[1]))

    for ans, _ in out:
        print(ans)
        
    # 谴责这道题的设计，最后这个q查询列表，居然不是递增的，匪夷所思，为什么要折腾这么一下？

if __name__ == '__main__':
    main()
```