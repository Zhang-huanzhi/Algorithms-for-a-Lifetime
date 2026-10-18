https://atcoder.jp/contests/abc478/tasks/abc478_d

---

### D - Range Set Insertion Query

运行时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 400 分

#### 题目描述
有 $N$ 个集合 $S_1, S_2, \dots, S_N$。初始时，$S_1, S_2, \dots, S_N$ 均为空集合。

接下来对这些集合执行 $Q$ 次操作。  
在第 $i$ 次操作（$1 \le i \le Q$）中，会给定一个由三个整数组成的三元组 $(L_i, R_i, X_i)$，并执行以下操作：
- 向集合 $S_{L_i}, S_{L_i+1}, \dots, S_{R_i}$ 中分别添加元素 $X_i$。

请在所有操作执行完毕后，求出每个集合 $S_i$（$i = 1, 2, \dots, N$）的大小（即集合内不同元素的个数）。

#### 制约条件
- $1 \le N \le 2 \times 10^5$
- $1 \le Q \le 2 \times 10^5$
- $1 \le L_i \le R_i \le N$ ($1 \le i \le Q$)
- $1 \le X_i \le Q$ ($1 \le i \le Q$)
- 输入中的所有数值均为整数。

#### 输入格式
输入按以下格式从标准输入给出：
> $N$ $Q$  
> $L_1$ $R_1$ $X_1$  
> $L_2$ $R_2$ $X_2$  
> $\vdots$  
> $L_Q$ $R_Q$ $X_Q$  

#### 输出格式
按顺序输出所有操作结束后 $S_1$ 的元素个数、$S_2$ 的元素个数、$\dots$、$S_N$ 的元素个数，两两之间用空格隔开。

---

### 样例 1
#### 输入样例 1
```plain
8 5
2 5 1
1 4 2
7 8 1
3 6 3
2 5 2
```
#### 输出样例 1
```plain
1 2 3 3 3 1 1 1
```
**样例说明 1**  
5 次操作结束后，各个集合分别为：  
$S_1 = \{2\}$  
$S_2 = \{1, 2\}$  
$S_3 = \{1, 2, 3\}$  
$S_4 = \{1, 2, 3\}$  
$S_5 = \{1, 2, 3\}$  
$S_6 = \{3\}$  
$S_7 = \{1\}$  
$S_8 = \{1\}$  

因此，每个集合的大小依次为 $1, 2, 3, 3, 3, 1, 1, 1$，按格式输出即可。注意：集合具有去重性，例如向已经含有 $2$ 的集合再次添加 $2$ 时，集合大小不会增加。

---

### 样例 2
#### 输入样例 2
```plain
30 20
3 22 11
8 30 10
12 14 7
2 17 4
1 19 12
7 30 15
11 23 2
14 25 17
9 12 7
10 16 7
16 18 19
1 11 14
11 15 4
1 21 6
4 8 10
23 24 11
8 27 10
1 19 12
23 23 16
13 24 12
```
#### 输出样例 2
```plain
3 4 5 6 6 6 7 7 8 8 9 8 8 9 9 10 9 8 7 7 7 6 7 5 3 2 2 2 2 2
```

# AC 倒查表&差分
``` python
import itertools  
import sys  
import math  
from collections import deque, defaultdict  
  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = [ set() for _ in range(n + 1)]  
    # 倒查表  
    memos = defaultdict(list)  
  
    # 还要涉及区间修改？差分吧，会不会还需要离散，这个x有点大？  
  
    for _ in range(q):  
        l, r, x = map(int, sys.stdin.readline().strip().split())  
  
        memos[x].append((l, r))  
  
    diff = [0] * (n + 2)  
  
    for memo in memos:  
        # 不对，中间有可能有空袭，这算是求并集？  
        memos[memo].sort()  
        x, y = memos[memo][0]  
        for l, r in memos[memo][1:]:  
  
            if l <= y:  
                if r > y:  
                    y = r  
            else:  
                diff[x] += 1  
                diff[y + 1] -= 1  
                x, y = l, r  
  
        diff[x] += 1  
        diff[y + 1] -= 1  
  
    ans = [0] * (n + 1)  
    # 直接原地还原吧  
    for i in range(1, n + 1):  
        diff[i] += diff[i - 1]  
  
    print(*diff[1:n + 1])  
  
if __name__ == "__main__":  
    main()
```

# AC 线性扫
``` python
import sys  
import math  
  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = [ [] for _ in range(n + 2) ]  
    brr = [ [] for _ in range(n + 2) ]  
    for _ in range(q):  
        l, r, x = map(int, sys.stdin.readline().strip().split())  
  
        arr[l].append(x)  
        brr[r + 1].append(x)  
  
    cnt = [0] * (q + 1)  
    ans = 0  
    outs = []  
  
    for t in range(1, n + 1):  
  
        for x in brr[t]:  
            cnt[x] -= 1  
            if cnt[x] == 0:  
                ans -= 1  
  
        for x in arr[t]:  
            if cnt[x] == 0:  
                ans += 1  
            cnt[x] += 1  
  
        outs.append(ans)  
  
    print(*outs)  
  
  
if __name__ == "__main__":  
    main()
```