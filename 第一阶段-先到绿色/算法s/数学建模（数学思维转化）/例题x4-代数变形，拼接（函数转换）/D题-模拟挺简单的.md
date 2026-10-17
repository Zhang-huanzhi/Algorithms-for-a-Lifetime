https://atcoder.jp/contests/abc428/tasks/abc428_d

---


# D - 183184
时间限制: 2.5 秒 / 内存限制: 1024 MiB

分值：400 分

### 问题描述

对于正整数 $x, y$，定义 $f(x, y)$ 如下：

> 将十进制表示的 $x, y$ 分别视为字符串，并按此顺序拼接，将得到的字符串记为 $z$。
> 将 $z$ 重新视为十进制整数时的数值定义为 $f(x, y)$。
> 例如，$f(3, 14) = 314$，$f(100, 3) = 1003$。

给定正整数 $C, D$。求满足以下条件的整数 $x$ 的个数：

* $1 \le x \le D$
* $f(C, C+x)$ 是完全平方数

由于给定了 $T$ 个测试用例，请对每一个测试用例求出相应的答案。

### 限制条件

* $1 \le T \le 3 \times 10^5$
* $1 \le C \le 2 \times 10^8$
* $1 \le D \le 5 \times 10^9$
* 输入的所有值均为整数。

### 输入格式

输入通过标准输入以下列格式给出：

$$\begin{array}{l}
T \\
\text{case}_1 \\
\text{case}_2 \\
\vdots \\
\text{case}_T
\end{array}$$

其中 $\text{case}_i$ 表示第 $i$ 个测试用例。每个测试用例按以下格式给出：

$$C \quad D$$

### 输出格式

输出 $T$ 行。
第 $i$ 行（$1 \le i \le T$）输出针对第 $i$ 个测试用例的答案。

---

### 输入样例 1

```text
4
4 80
183 5000
18 10
824 5000000000
```

### 输出样例 1

```text
3
2
0
1421
```

### 样例解释 1

在**第 1 个测试用例**中（$C=4, D=80$），满足条件的 $x$ 有 $x=5, 37, 80$ 共 $3$ 种情况：
* 当 $x = 5$ 时，$f(C, C+5) = f(4, 9) = 49 = 7^2$
* 当 $x = 37$ 时，$f(C, C+37) = f(4, 41) = 441 = 21^2$
* 当 $x = 80$ 时，$f(C, C+80) = f(4, 84) = 484 = 22^2$

在**第 2 个测试用例**中（$C=183, D=5000$），满足条件的 $x$ 有 $x=1, 3133$ 共 $2$ 种情况：
* 当 $x = 1$ 时，$f(C, C+1) = f(183, 184) = 183184 = 428^2$
* 当 $x = 3133$ 时，$f(C, C+3133) = f(183, 3316) = 1833316 = 1354^2$

在**第 3 个测试用例**中（$C=18, D=10$），满足条件的 $x$ 有 $0$ 种情况。

在**第 4 个测试用例**中（$C=824, D=5000000000$），满足条件的 $x$ 有 $1421$ 种情况。



# 暴力 Code TLE
``` python
import math
import sys

def main():

    t = int(sys.stdin.readline().strip())

    for _ in range(t):
        c, d = map(int, sys.stdin.readline().strip().split())

        # 直接模拟吧
        cnt = 0
        for x in range(1, d+1):

            num = int( str(c) + str(c + x) )

            if math.isqrt(num)**2 == num:
                cnt += 1

        print(cnt)

if __name__ == "__main__":
    main()
```

# AC Code
``` python
import math  
import sys  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
  
    out = []  
    for _ in range(t):  
        c, d = map(int, sys.stdin.readline().strip().split())  
  
        total_count = 0  
        for L in range(1, 11):  
            ten_L = 10 ** L  
            ten_L_minus_1 = 10 ** (L - 1)  
  
            # 约束一， c + x 必须是L位数  
            x_min_1 = ten_L_minus_1 - c  
            x_max_1 = ten_L - 1 - c  
  
            # 约束二， 小于d  
            x_min_2 = 1  
            x_max_2 = d  
  
            # 求 x 的局部安全边界 [L_x, R_x]            L_x = max(x_min_1, x_min_2)  
            R_x = min(x_max_1, x_max_2)  
  
            # 如果交集为空，说明该位数下没有符合条件的 x            if L_x > R_x:  
                continue  
  
            # 2. 计算拼接数 f(C, C+x) 的局部边界  
            # 拼接数学公式：f(C, C+x) = C * 10^L + C + x  
            base = c * ten_L + c  
            lower_bound = base + L_x  
            upper_bound = base + R_x  
  
            # 3. 计算在区间 [lower_bound, upper_bound] 内有多少个完全平方数 y^2            # 对应 y 的范围是：y_min <= y <= y_max  
            # 使用 math.isqrt 保证大数开方的绝对精确  
  
            y_min = math.isqrt(lower_bound - 1) + 1 if lower_bound > 0 else 0  
            y_max = math.isqrt(upper_bound)  
  
            if y_min <= y_max:  
                total_count += (y_max - y_min + 1)  
  
        out.append(str(total_count))  
  
    print('\n'.join(out))  
  
if __name__ == "__main__":  
    main()
```