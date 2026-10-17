
### 综合横向对比

| 维度 | 1. 笛卡尔积 (`itertools`) | 2. DFS / 栈模拟 | 3. 二进制枚举 (`1 << n`) |
| :--- | :--- | :--- | :--- |
| **代码简洁度** | **最高**（最直观） | 一般 | 较高 |
| **时间效率（Python）** | 中等（无剪枝） | **最快**（分支剪枝 + 累加传递） | 较慢（位运算在 Python 中较重） |
| **空间复杂度** | $O(N)$ | $O(N)$ | **$O(1)$** |
| **算法扩展性** | 低（仅能做简单枚举） | **极高**（可剪枝、记忆化、转成DP） | **高**（可平滑过渡到状压 DP） |

**一句话总结**：
平时随手快速写、代码量少选 **笛卡尔积**；考查状态思维与底层位运算选 **二进制枚举**；遇到严格时限、需要强力剪枝优化时，坚决选 **DFS**。

# 笛卡尔
``` python
import itertools  
import sys  
import math  
from collections import deque  
  
  
def main():  
  
    n, w = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    for _ in range(n):  
        a, b = map(int, sys.stdin.readline().strip().split())  
        arr.append((a, b))  
  
    ans = 0  
    #  笛卡尔  
    for comb in itertools.product( (0, 1), repeat = n):  
        cur_v = 0  
        cur_w = 0  
  
        for num, idx in zip(comb, range(n)):  
  
            if num == 1:  
                cur_v += arr[idx][0]  
                cur_w += arr[idx][1]  
  
        if cur_w <= w:  
            ans = max(ans, cur_v)  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```

# DFS
``` python
import sys
import math
from collections import deque


def main():

    n, w = map(int, sys.stdin.readline().strip().split())

    arr = []

    for _ in range(n):
        a, b = map(int, sys.stdin.readline().strip().split())
        arr.append((a, b))

    ans = 0
    # DFS, 暴力解法
    brr = [(0, 0, 0)]

    while brr:

        step, cur_v, cur_w = brr.pop()

        if cur_w > w:
            continue

        if step == n:
            ans = max(ans, cur_v)
            continue

        a, b = arr[step]

        brr.append((step + 1, cur_v, cur_w))
        
        brr.append((step + 1, cur_v + a, cur_w + b))

    print(ans)

if __name__ == "__main__":
    main()
```

# 二进制枚举
``` python
import sys
import math
from collections import deque


def main():

    n, w = map(int, sys.stdin.readline().strip().split())

    arr = []

    for _ in range(n):
        a, b = map(int, sys.stdin.readline().strip().split())
        arr.append((a, b))

    ans = 0
    # 二进制枚举，暴力解法
    for i in range(1 << n):
        cur_v = 0
        cur_w = 0

        for j in range(n):
            if (i >> j) & 1:
                cur_v += arr[j][0]
                cur_w += arr[j][1]

            if cur_w > w:
                break

        if cur_w <= w:
            ans = max(ans, cur_v)

    print(ans)

if __name__ == "__main__":
    main()
```


这三种方法代表了在算法竞赛与工程实现中**生成子集（$2^N$ 状态枚举）的三种不同范式**。下面从**原理实现、效率表现、优缺点及适用场景**对它们进行简要评价：

---

### 1. 笛卡尔积（`itertools.product`）

- **工作机制**：  
  利用 Python 标准库的生成器，在 C 语言底层依次产出所有的 $(0, 1)$ 序列组合，外层再遍历元组并求和。
- **优点**：
  - **代码可读性最高**：语义直观（每个位置选 0 还是选 1），非常 Pythonic，不易写出低级逻辑 Bug。
  - **内存极低**：由于返回的是迭代器，哪怕组合再多也是按需惰性生成，内存占用为 $O(N)$。
- **缺点**：
  - **无法剪枝**：无论当前物品组合是否早已超重，生成器都会死板地把所有 $2^N$ 个元组一个不漏地全部吐出来。
  - **重复计算多**：每个组合都要从头遍历累加一遍。
- **评价**：⭐⭐⭐⭐  
  **“最优雅、写得最快”**。非常适合比赛中打草稿、抢时间签到，或者数据规模极小（$N \le 18$）时使用。

---

### 2. DFS（显式栈模拟搜索）

- **工作机制**：  
  把问题视为一棵**二叉决策树**（选 / 不选），通过显式维护一个栈 `brr` 来进行深度优先遍历，参数中动态累加 `cur_v` 与 `cur_w`。
- **优点**：
  - **理论性能上限最高（自带最强剪枝）**：  
    这是三种方法中**唯一能做到“剪枝”**的架构！一旦某个节点出现 `cur_w > w`，以它为根的整个子树（后续所有 $2^{N - \text{step}}$ 种分支）会被瞬间跳过。
  - **避免重复求和**：前缀和是随递归/入栈一路累加下来的，每一步只需做一次加法，而不是每次重新循环累加。
- **缺点**：
  - 代码量比前两种略多，需要额外处理栈状态、边界条件和回溯逻辑。
- **评价**：⭐⭐⭐⭐⭐  
  **“天花板最高、泛用性最强”**。虽然在此题 $N=20$ 下三种都能过，但如果数据稍大一些（比如某些物品体积很大，能剪掉大半分支），DFS 是唯一不会超时的写法。也是面试中考官最希望看到的通用搜索框架。

---

### 3. 二进制枚举（位运算 Bit Brute-Force）

- **工作机制**：  
  利用整数 $i \in [0, 2^N - 1]$ 的二进制位（0 或 1）来表示每种配菜选与不选，通过 `(i >> j) & 1` 检查对应位。
- **优点**：
  - **竞赛通用性最强（国际标准模版）**：在 C++ / Java 等编译型语言中，位运算就是这道题的标准题解。
  - **代码紧凑独立**：无需依赖任何外部库，随手就能写出。
  - 支持内层小剪枝（当 `cur_w > w` 时可以中途 `break`）。
- **缺点**：
  - 在 **标准 Python (CPython)** 中较吃亏：Python 处理动态大整数的移位和解释器循环开销较大，当 $N \ge 20$ 且没用 PyPy 时，耗时会明显高于 C 语言。
- **评价**：⭐⭐⭐⭐  
  **“最经典、必须掌握的算法基本功”**。它不仅是子集生成的利器，更是后续学习**状态压缩动态规划（状压 DP）**的基石思想。

