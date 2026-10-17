https://atcoder.jp/contests/abc423/tasks/abc423_c

---

### C - Lock All Doors  
时间限制: 2 秒 / 内存限制: 1024 MiB

分值：300 分

#### 题目描述
有 $N+1$ 个房间排成一列，依次编号为 $0, 1, \dots, N$。

房间之间有 $N$ 扇门，编号为 $1, 2, \dots, N$。门 $i$ 位于房间 $i-1$ 和房间 $i$ 之间。

每个门都有一个表示锁的状态的值 $L_i$。当 $L_i = 0$ 时，门 $i$ 处于未锁状态；当 $L_i = 1$ 时，门 $i$ 处于已锁状态。

高桥君最初在房间 $R$。当且仅当门 $i$ 处于未锁状态时，他才可以在房间 $i-1$ 和房间 $i$ 之间移动。此外，当且仅当高桥君在房间 $i-1$ 或房间 $i$ 时，他才可以对门 $i$ 的锁进行 **开闭操作**。对门 $i$ 进行开闭操作时，如果该锁处于未锁状态，则会被锁上；如果处于已锁状态，则会被开锁。

请示出为了使所有的门都处于已锁状态，所需的最少开闭操作次数。

#### 限制条件
* $2 \le N \le 2 \times 10^5$
* $0 \le R \le N$
* $L_i \in \{0, 1\}$
* 输入的值均为整数。

#### 输入
输入按以下格式从标准输入给出：
> $N$ $R$
> $L_1$ $L_2$ $\dots$ $L_N$

#### 输出
输出一个整数，表示最少操作次数。

---

#### 输入样例 1
```text
6 3
1 0 0 1 0 0
```
#### 输出样例 1
```text
6
```
高桥君可以通过以下行动，用 6 次开闭操作使所有的门都处于已锁状态：
1. 移动到房间 2。
2. 对门 2 进行开闭操作，将门 2 锁上。
3. 移动到房间 3。
4. 对门 4 进行开闭操作，将门 4 开锁。
5. 对门 3 进行开闭操作，将门 3 锁上。
6. 移动到房间 4。
7. 对门 4 进行开闭操作，将门 4 锁上。
8. 移动到房间 5。
9. 对门 5 进行开闭操作，将门 5 锁上。
10. 对门 6 进行开闭操作，将门 6 锁上。

#### 输入样例 2
```text
2 1
0 0
```
#### 输出样例 2
```text
2
```

#### 输入样例 3
```text
8 2
0 1 0 0 1 0 1 1
```
#### 输出样例 3
```text
8
```

---

``` python
import sys  
import math  
  
def main():  
  
    n, s = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    l, r = -1, -1  
  
  
    for i in range(n):  
        if arr[i] == 0:  
            l = i  
            break  
    for i in range(n-1, -1, -1):  
        if arr[i] == 0:  
            r = i  
            break  
  
    if l == r == -1:  
        print(0)  
        return  
  
    l = min(s, l)  
    r = max(s, r)  
  
    cnt = 0  
    for i in range(l, r, 1):  
        if arr[i] == 0:  
            cnt += 1  
        else:  
            cnt += 2  
  
    if l > 0 and arr[l - 1] == 0:  
        cnt += 1  
    if r < n and arr[r] == 0:  
        cnt += 1  
  
    print(cnt)  
  
  
if __name__ == '__main__':  
    main()
```