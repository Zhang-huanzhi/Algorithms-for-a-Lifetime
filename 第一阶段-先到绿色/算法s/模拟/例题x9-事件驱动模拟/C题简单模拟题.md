https://atcoder.jp/contests/abc443/tasks/abc443_c

---

### C - 摸鱼上瘾 (Chokutter Addiction)

运行时间限制: 2 秒 / 内存限制: 1024 MiB  
分值 : 300 分

#### 题目描述
AtCoder 公司在时刻 $0$ 开始上班，在时刻 $T$ 下班。时刻 $t$ 与时刻 $t+1$ 之间相隔 $1$ 秒。  
高桥君在工作时间中按照以下规则刷社交软件 chokutter：

- 开始上班的瞬间（时刻 $0$），高桥君就会打开 chokutter。
- 如果在青木君经过高桥君工位后面的瞬间，高桥君正打开着 chokutter，他会**立刻关闭** chokutter。
- 如果高桥君在时刻 $t$ 关闭了 chokutter，他一定会在时刻 $t+100$ **重新打开** chokutter。
- 从上班到下班，青木君一共经过了高桥君工位后面 $N$ 次，其中第 $i$ 次是在时刻 $A_i$。

请问从上班到下班，高桥君一共刷了多少秒的 chokutter？  
保证青木君不会在高桥君刚好打开 chokutter 的瞬间经过。

#### 制约条件
- 输入均为整数
- $0 \leq N \leq 3 \times 10^5$
- $1 \leq A_1 < A_2 < \dots < A_N \leq T \leq 10^9$
- 青木君不会在高桥君刚好打开 chokutter 的瞬间经过工位

#### 输入格式
输入通过标准输入给出，格式如下：
> $N$ $T$  
> $A_1$ $A_2$ $\dots$ $A_N$

#### 输出格式
输出一个整数，表示高桥君刷 chokutter 的总时长（秒）。

---

### 样例解释

#### 样例 1
**输入：**
```text
5 700
100 150 300 350 700
```
**输出：**
```text
500
```
**说明：**
- 时刻 $0$：高桥君打开 chokutter。
- 时刻 $100$：青木君经过，高桥君立即关闭 chokutter（看了 $100$ 秒，当前累计 $100$ 秒）。
- 时刻 $150$：青木君经过，但此时高桥君没有打开 chokutter。
- 时刻 $200$：高桥君重新打开 chokutter。
- 时刻 $300$：青木君经过，高桥君立即关闭 chokutter（看了 $100$ 秒，当前累计 $200$ 秒）。
- 时刻 $350$：青木君经过，但此时高桥君没有打开 chokutter。
- 时刻 $400$：高桥君重新打开 chokutter。
- 时刻 $700$：青木君经过，高桥君立即关闭 chokutter（看了 $300$ 秒，当前累计 $500$ 秒）。
- 最终高桥君总共看了 $500$ 秒的 chokutter。

#### 样例 2
**输入：**
```text
0 1000000000

```
**输出：**
```text
1000000000
```
**说明：**
青木君有可能一次也没有经过。

#### 样例 3
**输入：**
```text
10 1234
395 424 588 745 773 863 910 958 1102 1195
```
**输出：**
```text
734
```

# AC Code
``` python
import sys  
  
def main():  
  
    n, t = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 事件驱动模拟  
    open = 0  
    ans = 0  
    for close in arr:  
  
        if close > open:  
            ans += close - open  
            open = (close + 100)  
  
    if t > open:  
        ans += (t - open)  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```