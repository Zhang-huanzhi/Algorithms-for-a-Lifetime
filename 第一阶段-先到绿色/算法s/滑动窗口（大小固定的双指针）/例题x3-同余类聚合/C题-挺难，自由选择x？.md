https://atcoder.jp/contests/abc440/tasks/abc440_c

---

### C - 条纹马 (Striped Horse)

**时间限制：** 2 秒 / **内存限制：** 1024 MiB  
**分值：** 300 分

#### 题目描述

苹果同学（Ringo）想把一匹马涂成黑白相间的条纹马（斑马）。

有一排由 $N$ 个格子组成的序列，编号从 $1$ 到 $N$。
起初，所有的格子都是白色的。将第 $i$ 个格子涂成黑色需要花费 $C_i$ 的代价。

现在只进行一次以下操作，将某些格子涂成黑色：

1. 自由选择一个正整数 $x$。
2. 对于所有满足 $1 \le i \le N$ 的整数 $i$，若 $(i + x) \bmod 2W < W$，则将第 $i$ 个格子涂成黑色。

请计算执行该操作所需花费的总代价的最小值。

本题包含 $T$ 组测试数据，请对每组数据求解。

#### 数据范围

* $1 \le T \le 2 \times 10^5$
* $1 \le N \le 2 \times 10^5$
* $1 \le W \le 2 \times 10^5$
* $1 \le C_i \le 10^9$
* 所有测试用例中 $N$ 的总和不超过 $2 \times 10^5$
* 所有测试用例中 $W$ 的总和不超过 $2 \times 10^5$
* 所有输入数据均为整数

#### 输入格式

输入通过标准输入按以下格式给出。其中 $\text{case}_i$ 表示第 $i$ 组测试数据。

$$
T
$$
$$
\text{case}_1
$$
$$
\vdots
$$
$$
\text{case}_T
$$

每组测试数据的格式如下：

$$
N \quad W
$$
$$
C_1 \quad C_2 \quad \dots \quad C_N
$$

#### 输出格式

输出共 $T$ 行。
第 $i$ 行输出第 $i$ 组测试数据的答案（即最小总代价）。

---

#### 样例 1

**输入：**
```plain
4
8 2
1 10 10 1 1 10 10 1
8 3
1 10 10 1 1 10 10 1
8 4
1 10 10 1 1 10 10 1
4 100
100000 100000 100000 100000
```

**输出：**
```plain
4
12
22
0
```

**样例解释：**
在第 1 组测试数据中，选择 $x = 4$ 时，格子 $1, 4, 5, 8$ 会被涂黑，总代价为 $C_1 + C_4 + C_5 + C_8 = 1 + 1 + 1 + 1 = 4$。无法使总代价小于 4，因此最小值为 4。


# Code
``` python
import sys  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
  
        n, w = map(int, sys.stdin.readline().strip().split())  
  
        crr = list(map(int, sys.stdin.readline().strip().split()))  
  
        mod = 2 * w  
        s = [0] * mod  
        for i in range(1, n + 1):  
            s[i % mod] += crr[i - 1]  
  
        s2 = s + s  
  
        cur_sum = sum(s2[:w])  
        min_cost = cur_sum  
  
        for i in range(mod):  
            cur_sum  += s2[i + w] - s2[i]  
            if cur_sum < min_cost:  
                min_cost = cur_sum  
  
        print(min_cost)  
  
if __name__ == "__main__":  
    main()
```