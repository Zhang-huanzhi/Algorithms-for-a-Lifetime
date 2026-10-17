https://atcoder.jp/contests/abc471/tasks/abc471_c

---

### C - 饼干与贪吃的高桥君 (Cookies and Greedy Takahashi)

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：300 分

---

#### 题目描述

数轴上有 $N$ 处掉落了饼干。第 $i$ 块饼干掉落的坐标为 $A_i$。

高桥君最初位于数轴的坐标 $0$ 处，他会不断重复以下行动，直到捡起所有 $N$ 块饼干为止：

* **行动**：移动到距离自己当前位置**最近**的饼干所在坐标（如果有多个最近的，则选择**坐标最小**的那个），并捡起该饼干。

请计算高桥君捡完所有饼干的总移动距离。

---

#### 数据范围

* $1 \le N \le 3 \times 10^5$
* $-10^9 \le A_i \le 10^9$
* $A_i \ne 0$
* $A_i$ 互不相同
* 所有输入均为整数

---

#### 输入格式

输入通过标准输入给出，格式如下：

$$N$$
$$A_1 \ \dots \ A_N$$

---

#### 输出格式

输出高桥君移动的总距离。

---

#### 样例 1

**输入**
```text
4
-1 -4 2 -11
```

**输出**
```text
23
```

**说明**  
高桥君的行动如下：
1. 从坐标 $0$ 移动到坐标 $-1$ 并捡起饼干，移动距离为 $1$。
2. 从坐标 $-1$ 移动到坐标 $-4$ 并捡起饼干，移动距离为 $3$。
3. 从坐标 $-4$ 移动到坐标 $2$ 并捡起饼干，移动距离为 $6$。
4. 从坐标 $2$ 移动到坐标 $-11$ 并捡起饼干，移动距离为 $13$。

因此，总移动距离为 $1 + 3 + 6 + 13 = 23$。

注意：在第 2 次行动中，坐标 $-4$ 处的饼干与坐标 $2$ 处的饼干距离当前位置（$-1$）都是 $3$，但高桥君会优先选择坐标更小的 $-4$。

---

#### 样例 2

**输入**
```text
10
1 2 3 4 5 -1 -2 -3 -4 -6
```

**输出**
```text
17
```
# AC Code

``` python
import math  
import sys  
from bisect import bisect_left  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 直接模拟吧  
    arr.sort()  
    idx = bisect_left(arr, 0)  
    # brr 负数  
    # crr 整数  
    brr = arr[:idx]  
    crr = arr[idx:]  
    brr = [abs(brr[i]) for i in range(idx- 1, -1, -1) ]  
  
    x = 0  
    y = 0  
    # false 为 当前在brr， true 为 当前在crr  
    state = False  
  
    # 手动算下第一个  
    opx = brr[x] if x < len(brr) else math.inf  
    opy = crr[y] if y < len(crr) else math.inf  
  
    curr = 0  
    cnt = 0  
    ans = 0  
    if opx <= opy:  
        curr = brr[x]  
        state = False  
        cnt += 1  
        ans += opx  
        x += 1  
    else:  
        curr = crr[y]  
        state = True  
        cnt += 1  
        ans += opy  
        y += 1  
  
    while True:  
  
        if cnt == n:  
            break  
  
        # 如果在brr  
        if not state:  
  
            opx = brr[x] - curr if x < len(brr) else math.inf  
            opy = curr + crr[y] if y < len(crr) else math.inf  
  
            if opx <= opy:  
                curr = brr[x]  
                state = False  
                cnt += 1  
                ans += opx  
                x += 1  
            else:  
                curr = crr[y]  
                state = True  
                cnt += 1  
                ans += opy  
                y += 1  
  
        # 如果在crr  
        else:  
  
            opx = curr + brr[x] if x < len(brr) else math.inf  
            opy = crr[y] - curr if y < len(crr) else math.inf  
  
            if opx <= opy:  
                curr = brr[x]  
                state = False  
                cnt += 1  
                ans += opx  
                x += 1  
            else:  
                curr = crr[y]  
                state = True  
                cnt += 1  
                ans += opy  
                y += 1  
  
    print(ans)  
  
  
if __name__ == "__main__":  
    main()
```