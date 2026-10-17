https://atcoder.jp/contests/abc465/tasks/abc465_d

---

### D - X to Y
时间限制: 2 秒 / 内存限制: 1024 MiB

分值 : 400 分

#### 题目描述
给定整数 $X, Y$ 和一个不小于 $2$ 的整数 $K$。

有一个变量 $x$，初始时 $x = X$。你可以对 $x$ 进行以下操作任意次（可以为 $0$ 次）：

* 选择一个满足 $\lfloor \frac{x}{K} \rfloor = y$ 或 $\lfloor \frac{y}{K} \rfloor = x$ 的整数 $y$，并将 $x$ 的值替换为 $y$。

其中，对于实数 $z$，$\lfloor z \rfloor$ 表示不大于 $z$ 的最大整数（即下取整）。

求将 $x$ 变为 $Y$ 所需的最少操作次数。可以证明，在给定的限制条件下，总是可以通过有限次操作将 $x$ 变为 $Y$。

共有 $T$ 组测试数据，请对每组测试数据求解。

#### 数据范围与限制
* $1 \le T \le 2 \times 10^5$
* $0 \le X, Y \le 10^{18}$
* $2 \le K \le 10^{18}$
* 所有输入值均为整数。

#### 输入格式
输入从标准输入按以下格式给出：

$$T$$
$$\text{case}_1$$
$$\text{case}_2$$
$$\vdots$$
$$\text{case}_T$$

每个测试用例 $\text{case}_i$ ($1 \le i \le T$) 的格式如下：

$$X \quad Y \quad K$$

#### 输出格式
依次输出每个测试用例的答案，每个答案占一行。

---

#### 输入样例 1
```text
4
11 9 3
0 0 2
842 180 7
1948706013487601 48019760148910476 89014537
```

#### 输出样例 1
```text
2
0
7
5
```

#### 样例说明 1
对于第 1 个测试用例：
可以通过以下 $2$ 次操作将 $x$ 变为 $Y$：
1. 选择 $y = 3$。因为 $\lfloor \frac{x}{K} \rfloor = \lfloor \frac{11}{3} \rfloor = 3$，所以这一步操作是合法的。然后，将 $x$ 的值替换为 $3$。
2. 选择 $y = 9$。因为 $\lfloor \frac{y}{K} \rfloor = \lfloor \frac{9}{3} \rfloor = 3$，所以这一步操作是合法的。然后，将 $x$ 的值替换为 $9$。

无法用少于 $2$ 次的操作将 $x$ 变为 $Y$，因此第 1 行输出 $2$。


# AC Code

``` python
import sys
import itertools
from collections import deque


def main():

    t = int(sys.stdin.readline().strip())

    for _ in range(t):
        x, y, k = map(int, sys.stdin.readline().strip().split())

        cnt = 0
        # 随便贪心吧
        while True:

            if x == y:
                break

            if x > y:
                x = x // k
                cnt += 1

                if x == y:
                    break

            else:
                y = y // k
                cnt += 1
    
        print(cnt)

if __name__ == "__main__":
    main()
```

# 题解Code

``` python
input = __import__("sys").stdin.readline

for _ in range(int(input())):
    x, y, k = map(int, input().split())
    ans = 0
    while x != y:
        if x < y:
            x, y = y, x
        x //= k
        ans += 1
    print(ans)

```