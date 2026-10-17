https://atcoder.jp/contests/abc434/tasks/abc434_c

---


### C - 高桥拍翅飞行
**执行时间制限:** 2 sec / **内存限制:** 1024 MiB

**分值:** 300 分

#### 题目描述
高桥君决定用气球在空中飞行。高桥君在时刻 $0$（单位：秒）时的初始高度为 $H$，他将飞行 $10^9$ 秒。

高桥君每秒钟最多可以将自己的飞行高度改变 $1$。但是，他的高度不能降到 $0$ 以下（即高度必须严格大于 $0$）。

换句话说，设高桥君在时刻 $t$ 的高度为 $F(t)$，则 $F(t)$ 必须满足以下所有条件：
* $F(0) = H$
* 对于任意 $0 \le t < u \le 10^9$，满足 $-1 \le \frac{F(u) - F(t)}{u - t} \le 1$
* 对于任意 $0 \le t \le 10^9$，满足 $F(t) > 0$

现在有关于高度的 $N$ 个目标。
第 $i$ 个目标是：在时刻 $t_i$ 时，高度必须在 $l_i$ 到 $u_i$ 之间（包含边界，即 $l_i \le F(t_i) \le u_i$）。
高桥君能否进行飞行，从而达成所有目标？

题目将给出 $T$ 个测试用例，请分别求出每个测试用例的答案。

#### 限制条件
* $1 \le T \le 10^5$
* $1 \le N \le 10^5$
* $1 \le H \le 10^9$
* $1 \le t_1 < t_2 < \dots < t_N \le 10^9$
* $1 \le l_i \le u_i \le 10^9$
* 所有测试用例中 $N$ 的总和不超过 $10^5$
* 输入的所有值都是整数。

#### 输入
输入通过标准输入给出，格式如下，其中 $\text{case}_i$ 表示第 $i$ 个测试用例：
> $T$
> $\text{case}_1$
> $\text{case}_2$
> $\vdots$
> $\text{case}_T$

每个测试用例的格式如下：
> $N$ $H$
> $t_1$ $l_1$ $u_1$
> $t_2$ $l_2$ $u_2$
> $\vdots$
> $t_N$ $l_N$ $u_N$

#### 输出
输出 $T$ 行。
第 $i$ 行输出：如果在第 $i$ 个测试用例中能够达成所有目标，则输出 `Yes`；否则输出 `No`。

---

### 样例解释

#### 样例 1
**输入:**
```text
3
2 5
3 1 4
8 9 11
2 6
1 1 4
3 5 8
10 36
27 37 38
30 34 54
38 20 77
45 1 36
49 38 51
52 31 58
65 43 60
71 14 42
73 36 38
85 14 29
```
**输出:**
```text
Yes
No
Yes
```

* **对于第 1 个测试用例：**
  高桥君可以通过以下飞行方式达成所有目标：
  * 时刻 $0$ 时，高桥君在高度 $5$。
  * 从时刻 $0$ 到时刻 $2$，高桥君以每秒 $0.5$ 的速度下降。
  * 时刻 $2$ 时，高桥君的高度为 $5 - 0.5 \times 2 = 4$。
  * 从时刻 $2$ 到时刻 $3$，高桥君保持高度不变。
  * 时刻 $3$ 时，高桥君的高度为 $4$。此时高桥君满足了第 1 个目标（$1 \le 4 \le 4$）。
  * 从时刻 $3$ 到时刻 $8$，高桥君以每秒 $1$ 的速度上升。
  * 时刻 $8$ 时，高桥君的高度为 $4 + 1 \times 5 = 9$。此时高桥君满足了第 2 个目标（$9 \le 9 \le 11$）。

* **对于第 2 个测试用例：**
  无论高桥君如何飞行，都无法达成所有目标。

# Code 区间贪心 AC
``` python
import sys  
  
def main():  
  
    T = int(sys.stdin.readline().strip())  
  
    out = []  
    # 区间贪心  
    for _ in range(T):  
        n, h = map(int, sys.stdin.readline().strip().split())  
        state = True  
  
        curr_time = 0  
        curr_left = h  
        curr_right = h  
  
        for _ in range(n):  
            t, l, u = map(int, sys.stdin.readline().strip().split())  
            avail_t = t - curr_time  
            curr_time = t  
  
            # 看看极限可以移动的范围  
            next_left = curr_left - avail_t  
            next_right = curr_right + avail_t  
  
            # 取交集  
            curr_left = max(l, next_left)  
            curr_right = min(u, next_right)  
  
            if curr_left > curr_right:  
                state = False  
  
        if state:  
            out.append("Yes")  
        else:  
            out.append("No")  
  
    print('\n'.join(out))  
  
  
if __name__ == "__main__":  
    main()
```