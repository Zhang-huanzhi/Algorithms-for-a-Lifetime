### 1. 暴力代码（TLE）的瓶颈分析

在暴力代码中，您使用了 `itertools.combinations(arr, 3)`。

* **高昂的时间复杂度**：
  从长度为 $N$ 的数组中选择 3 个元素，其组合总数为：
  $$\binom{N}{3} = \frac{N(N-1)(N-2)}{6}$$
  当 $N = 2 \times 10^5$ 时，组合数大约为 $\frac{8 \times 10^{15}}{6} \approx 1.33 \times 10^{15}$。
  在计算机中，即使是简单的 C++ 循环，1 秒内通常也只能处理约 $10^8$ 次操作。Python 作为解释型语言，1 秒内处理的操作次数更少（通常在 $10^7$ 数量级）。面对 $10^{15}$ 级别的循环次数，暴力枚举必然会超时。
* **重复计算与多余操作**：
  暴力方法生成了每一个具体的组合，并对每个组合执行 `set(nums)` 来判断元素种类。这种做法没有利用数组中存在大量重复数字的特性，导致了大量的冗余计算。

---

### 2. 组合计数代码（AC）的优化思路

优化后的代码之所以能将运行时间缩短至毫秒级别，在于它完成了以下几个维度的思维转变：

#### ① 视角转变：从“位置组合”到“数值频次”
暴力法关注的是“哪三个位置的数符合条件”，而优化法关注的是“每个数值在数组中出现了多少次”。
一旦我们统计出每个数值的出现频次（使用 `Counter`），具体数值在数组中的原位置就不再重要了。我们只需要根据这些频次进行数学计算。

#### ② 数学抽象：一括计算代替逐个检查
“恰好有 2 种不同的值”这一条件意味着：**有且仅有一个数值重复出现了 2 次，另一个数值出现了 1 次。**

假设我们指定数值 `val` 作为那个出现 2 次的数：
1. **选出 2 个 `val`**：如果 `val` 出现了 `count` 次，那么从中选出 2 个的方法数是：
   $$\binom{count}{2} = \frac{count \times (count - 1)}{2}$$
2. **选出 1 个非 `val`**：数组中剩下的不等于 `val` 的元素个数为 $N - count$。从中选出 1 个的方法数自然就是 $N - count$。
3. **根据乘法原理**，以 `val` 为重复元素的合格三元组数量为：
   $$\frac{count \times (count - 1)}{2} \times (N - count)$$

对于不同的 `val`，这些组合是完全不重叠的。因此，我们只需要遍历 `Counter` 中的每一个键值对，进行一次简单的乘法和除法运算，然后累加即可。

#### ③ 复杂度的跃升
* **时间复杂度**：
  * **暴力法**：$O(N^3)$
  * **优化法**：统计频次需要 $O(N)$ 时间；`Counter` 中不同键的数量最多为 $N$，因此循环执行最多 $N$ 次，每次循环内是 $O(1)$ 的常数级运算。整体时间复杂度降为 **$O(N)$**。
  对于 $N = 2 \times 10^5$，优化后的代码只需运行约数十万次简单运算，瞬间即可完成。
* **空间复杂度**：
  * 仅需 $O(N)$ 的空间来存储频次哈希表，完全在 1024 MiB 的内存限制之内。

---

### 总结
这种优化思路在处理“计数类”或“组合类”问题时非常通用：
**当枚举所有实体（如子集、子序列、三元组）代价过高时，应考虑通过哈希表等工具统计特征，并利用排列组合公式将“枚举问题”转化为“代数计算问题”。**

# 暴力Code TLE
``` python
import math
import sys

import itertools


def main():

    n = int(sys.stdin.readline().strip())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    cnt = 0
    for nums in itertools.combinations(arr, 3):
        if len(set(nums)) == 2:
            cnt += 1
            
    print(cnt)

if __name__ == "__main__":
    main()
```

# 组合计数Code AC
``` python
import math
import sys

import itertools
from collections import Counter


def main():

    n = int(sys.stdin.readline().strip())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    ans = 0
    counters = Counter(arr)

    for val, count in counters.items():
        if count >= 2:
            cnt = (count * (count - 1) // 2) * (n - count)
            ans += cnt

    print(ans)

if __name__ == "__main__":
    main()
```