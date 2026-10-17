https://atcoder.jp/contests/abc424/tasks/abc424_e

---

# E - Cut in Half

时间限制: 2 秒 / 内存限制: 1024 MiB

分值 : 475 分

### 问题描述

袋子里有 $N$ 根棒子，长度分别为 $A_1, A_2, \dots, A_N$。

你将重复执行以下操作 $K$ 次：

* 从袋子中的棒子里取出最长的一根，将其等分成两半（二等分），然后将这两根新棒子放回袋子中。

在完成 $K$ 次操作后，袋子中一共有 $N+K$ 根棒子。请计算这些棒子中**长度从大到小第 $X$ 长的棒子的长度**。

本题包含 $T$ 个测试用例，请对每个测试用例进行求解。

### 数据范围

* $1 \le T \le 10^5$
* 对于每个测试用例：
  * $1 \le N \le 10^5$
  * $1 \le A_i \le 10^9$
  * $1 \le K \le 10^9$
  * $1 \le X \le N+K$
* 所有测试用例中 $N$ 的总和不超过 $10^5$。
* 输入中的所有值均为整数。

### 输入格式

输入按以下格式从标准输入给出：

$$T$$
$$\text{testcase}_1$$
$$\text{testcase}_2$$
$$\vdots$$
$$\text{testcase}_T$$

其中，每个测试用例 $\text{testcase}_i$ 的格式如下：

$$N \quad K \quad X$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$

### 输出格式

输出 $T$ 行。
第 $i$ 行（$1 \le i \le T$）输出对第 $i$ 个测试用例的答案。
当输出与真实答案的绝对误差或相对误差不超过 $10^{-9}$ 时，将被判定为正确。

---

### 输入样例 1

```text
2
3 4 5
40 20 30
10 100 50
1 2 3 4 5 6 7 8 9 10
```

### 输出样例 1

```text
10.00000000000000000000
0.50000000000000000000
```

#### 样例解释 1

在第 1 个测试用例中，最初袋中有 3 根长度为 $20, 30, 40$ 的棒子。操作过程如下：

1. 取出长度为 $40$ 的棒子，等分成 2 根长度为 $20$ 的棒子放回。袋中棒子长度变为：$20, 20, 20, 30$。
2. 取出长度为 $30$ 的棒子，等分成 2 根长度为 $15$ 的棒子放回。袋中棒子长度变为：$15, 15, 20, 20, 20$。
3. 取出长度为 $20$ 的棒子，等分成 2 根长度为 $10$ 的棒子放回。袋中棒子长度变为：$10, 10, 15, 15, 20, 20$。
4. 取出长度为 $20$ 的棒子，等分成 2 根长度为 $10$ 的棒子放回。袋中棒子长度变为：$10, 10, 10, 10, 15, 15, 20$。

操作结束后，长度从大到小第 $5$ 长的棒子长度为 $10$。



# AC Code
``` python
import sys  
import math  
from bisect import bisect_left  
  
  
def main():  
    # 读取测试用例数量 T    t_line = sys.stdin.readline()  
    if not t_line:  
        return  
    t = int(t_line.strip())  
  
    for _ in range(t):  
        # 读取 N, K, X        nkx_line = sys.stdin.readline()  
        if not nkx_line:  
            break  
        n, k, x = map(int, nkx_line.strip().split())  
  
        # 读取初始棒子数组  
        arr = list(map(int, sys.stdin.readline().strip().split()))  
  
        # 将数组转为浮点数并排序，以便进行快速二分定位  
        arr = [float(val) for val in arr]  
        arr.sort()  
  
        # Check 函数：计算进行 K 次操作后，长度 >= W 的棒子数量  
        def count_ge(W):  
            if W < 1e-12:  
                return n + k  # 极小值处理，防止除以 0  
            # 快速定位 >= 2W 和 >= W 的棒子边界  
            idx_2W = bisect_left(arr, 2 * W)  
            idx_W = bisect_left(arr, W)  
  
            req = 0  
            # 第一阶段：计算将所有 >= 2W 的棒子拆分到 < 2W 所需的操作次数  
            for i in range(idx_2W, n):  
                val = arr[i] / (2 * W)  
                d = int(math.log2(val))  
                req += (1 << (d + 1)) - 1  
                if req > k:  
                    # 此时操作数不足以拆完所有 >= 2W 的棒子，每多切一次，>= W 的数量就 +1                    return (n - idx_W) + k  
  
            # 第二阶段：操作数非常充足，多余的操作（rem）开始消耗 [W, 2W) 区间的棒子，每多切一次，>= W 的数量就 -1            max_p = req + (n - idx_W)  
            rem = k - req  
            if rem > max_p:  
                return 0  
            else:  
                return max_p - rem  
  
        # 对答案 W 进行二分搜索  
        low = 0.0  
        high = arr[-1]  
        for _ in range(80):  
            mid = (low + high) / 2  
            if count_ge(mid) >= x:  
                low = mid  
            else:  
                high = mid  
  
        print(f"{low:.20f}")  
  
  
if __name__ == '__main__':  
    main()
```