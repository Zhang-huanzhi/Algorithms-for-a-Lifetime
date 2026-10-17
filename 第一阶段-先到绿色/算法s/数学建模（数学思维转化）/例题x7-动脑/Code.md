``` python
import sys
import bisect

def main():

    n, m = map(int, sys.stdin.readline().strip().split())

    arr = [ [0] * n for _ in range(m)]

    for i in range(m):
        a, b = map(int, sys.stdin.readline().strip().split())

        arr[i][a - 1] = 1
        arr[i][b - 1] = 1

    # for num in arr:
    #     print(num)
    ans = 0
    # 对arr旋转九十度
    arr_t = [ [arr[i][j] for i in range(m)] for j in range(n)]
    # for num in arr_t:
    #     print(num)

    # 使用索引循环，确保只比较不同的人，且只比较一次（i < j）
    for i in range(n):
        for j in range(i + 1, n):
            xrr = arr_t[i]
            yrr = arr_t[j]
            if not any(x == 0 and y == 0 for x, y in zip(xrr, yrr)):
                ans += 1

    print(ans)

if __name__ == "__main__":
    main()
```

简要来说，该代码在面对最大数据范围（$N, M \le 2 \times 10^5$）时，资源消耗呈**爆炸式增长**：

### 1. 为什么会 MLE（内存超限）？
* **根本原因**：创建了大小为 $M \times N$ 的二维数组 `arr` 以及转置数组 `arr_t`。
* **具体数据**：$2 \times 10^5 \times 2 \times 10^5 = 4 \times 10^{10}$（400亿）个元素。
* **内存估算**：在 Python 中，存储这样一个巨大的二维列表需要消耗 **100 GB 以上** 的内存，而题目限制只有 **1 GB**。

### 2. 为什么会 TLE（时间超限）？
* **根本原因**：三重嵌套循环，时间复杂度为 $O(N^2 \cdot M)$。
* **具体数据**：
  * 两层玩家循环：执行 $\frac{N^2}{2} \approx 2 \times 10^{10}$（200亿）次。
  * 最内层 `any(...)` 判定：最多遍历 $M = 2 \times 10^5$ 次。
* **总计算量**：最高可达 **$4 \times 10^{15}$ 次** 基本运算。Python 在 2 秒内只能执行约 $10^7 \sim 10^8$ 次运算，这超出了限时的数百万倍。

---

# 优化思路
官方题解的核心优化思路非常惊艳，它用了一个极其巧妙的突破口，直接把空间和时间复杂度从 **$O(NM)$ 和 $O(N^2 M)$** 降到了 **$O(N + M)$**（几乎是瞬间运行完毕）。

以下是官方题解的详细优化思路：

---

### 第一步：锁定“首场决赛”的两个人（最核心的突破口）

任何合法的玩家对 $(x, y)$ 都必须覆盖**每一场**比赛。
所以，他们必然要覆盖**第一场比赛**。
第一场比赛的决赛选手是 $A_1$ 和 $B_1$。

这说明：**$x$ 和 $y$ 中，至少有一个人必须是 $A_1$ 或者是 $B_1$**。

这个结论极其关键！它意味着我们不需要去枚举 $O(N^2)$ 种组合，而只需要分别讨论两种情况：
1. **如果 $A_1$ 在组合中**：去寻找和 $A_1$ 搭配的另一个玩家。
2. **如果 $B_1$ 在组合中**：去寻找和 $B_1$ 搭配的另一个玩家。

---

### 第二步：如何快速找到和 $A_1$ 搭配的另一个玩家？

假设 $A_1$ 在组合中，我们要找另一个玩家 $p$。我们来看看 $A_1$ 缺席了哪些比赛：

* **情况 A：$A_1$ 参加了所有 $M$ 场比赛的决赛（无一缺席）**
  * 既然 $A_1$ 一个人就覆盖了全部比赛，那么另一个玩家 $p$ 选谁都可以（除了 $A_1$ 自己）。
  * 这种情况下，直接产生 **$N - 1$** 个合法组合：$(A_1, p)$，其中 $p \in [1, N]$ 且 $p \neq A_1$。

* **情况 B：$A_1$ 缺席了某些比赛的决赛**
  * 假设 $A_1$ 缺席了第 3 场（选手是 $\{3, 4\}$）和第 5 场（选手是 $\{2, 4\}$）的决赛。
  * 既然 $A_1$ 缺席了，**另一个玩家 $p$ 就必须独自去覆盖这两场比赛**。
  * 也就是说，$p$ 必须同时在第 3 场和第 5 场的决赛选手中。
  * 那么 $p$ 只能从这两场比赛选手的**交集**里选：
    $$p \in \{3, 4\} \cap \{2, 4\} = \{4\}$$
  * 这样，我们**瞬间锁定**了 $p$ 只能是玩家 $4$。此时我们只需要单独验证一下 $(A_1, 4)$ 这个唯一组合是否真的合法即可。
  * **因为交集的基数（每场决赛只有2人）极小，所以交集出来的候选人 $p$ 最多只有 2 个**。

---

### 第三步：同理分析 $B_1$

用同样的方法，找出和 $B_1$ 搭配的所有可能玩家 $q$：
* 如果 $B_1$ 没缺席过任何比赛，直接得到 $N - 1$ 个组合。
* 如果 $B_1$ 有缺席，通过求缺席比赛选手集合的交集，找出最多 2 个候选人 $q$，并进行验证。

---

### 第四步：去重合并，得出答案

最后，我们把从 $A_1$ 找出的合法组合，和从 $B_1$ 找出的合法组合合并在一起。
唯一需要注意的是**去重**：如果 $(A_1, B_1)$ 这一对同时在两边都被判定为合法，只计算一次。

---

### 优化后的复杂度对比

| 指标 | 您的暴力代码 | 官方题解优化后 |
| :--- | :--- | :--- |
| **空间复杂度** | $O(NM)$（~100 GB） | **$O(N + M)$**（~10 MB）只需要存邻接表或每场比赛的人 |
| **时间复杂度** | $O(N^2 M)$（运算 $4 \times 10^{15}$ 次） | **$O(N + M)$**（运算约 $10^6$ 次，0.1秒内完成） |

#  AC Code

``` python
import sys  
import bisect  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    for i in range(m):  
        a, b = map(int, sys.stdin.readline().strip().split())  
  
        arr.append((a, b))  
  
    # 取第一场候选人  
    x, y = arr[0]  
  
    missed_x = []  
    missed_y = []  
  
    for i in range(m):  
        a, b = arr[i]  
        if a != x and b != x:  
            missed_x.append(i)  
        if a != y and b != y:  
            missed_y.append(i)  
  
    valid_pairs = set()  
  
    # 辅助函数：寻找能与 u 配对的玩家  
    def find_partners(u, missed):  
        partners = []  
        if not missed:  
            # 如果没有错过任何一场，说明单独 u 就能覆盖所有比赛  
            # 此时除了 u 以外的任何玩家都可以与 u 配对  
            for z in range(1, n + 1):  
                if z != u:  
                    partners.append(z)  
        else:  
            # 如果存在错过的比赛，那么另一个玩家必须能够覆盖所有错过的比赛。  
            # 也就是说，他必须在所有错过的比赛中都出现。  
            # 我们可以直接以“第一场错过的比赛”的两位选手作为候选人  
            first_missed_idx = missed[0]  
            candidates = [arr[first_missed_idx][0], arr[first_missed_idx][1]]  
  
            # 检查这些候选人是否在所有错过的比赛中都出现  
            for c in candidates:  
                is_valid = True  
                for m_idx in missed:  
                    if arr[m_idx][0] != c and arr[m_idx][1] != c:  
                        is_valid = False  
                        break                if is_valid:  
                    partners.append(c)  
        return partners  
  
    # 寻找与 x 配对的玩家  
    for p in find_partners(x, missed_x):  
        valid_pairs.add((min(x, p), max(x, p)))  
  
    # 寻找与 y 配对的玩家  
    for p in find_partners(y, missed_y):  
        valid_pairs.add((min(y, p), max(y, p)))  
  
    # 输出满足条件的对数  
    print(len(valid_pairs))  
  
  
if __name__ == "__main__":  
    main()
```