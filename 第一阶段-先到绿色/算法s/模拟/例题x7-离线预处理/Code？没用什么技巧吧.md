# 暴力
``` python
import sys


def main():
    
    n, m = map(int, sys.stdin.readline().strip().split())

    arr = []

    for _ in range(m):
        xrr = list(map(int, sys.stdin.readline().strip().split()))
        xrr = xrr[1:]
        arr.append(xrr)

    brr = list(map(int, sys.stdin.readline().strip().split()))

    accept_food = set()
    accept_menu = set()
    cnt = 0
    out = []
    # 暴力一道一道去扫描
    for num in brr:
        accept_food.add(num)

        # 暴力每一个菜
        for i in range(m):
            if i in accept_menu:
                continue

            for food in arr[i]:
                if food not in accept_food:
                    break
            else:
                accept_menu.add(i)
                cnt += 1

        out.append(cnt)

    print('\n'.join(map(str, out)))


if __name__ == "__main__":
    main()
```

您的代码在逻辑上是正确的，但在面对题目中的最大数据范围时，会遇到 **运行超时（TLE，Time Limit Exceeded）** 的问题。

### 为什么会超时？

在您的代码中，外层循环遍历 `brr`（共 $N$ 次），内层循环遍历所有尚未能食用的菜品，并逐一检查它们的食材。
* 设菜品总数为 $M$，食材总数为 $\sum K_i$。
* 最坏情况下（例如，很多菜品直到最后一天才能吃），每次循环都要检查大部分菜品的食材。
* 这会导致总的时间复杂度达到 $O(N \sum K_i)$。
* 根据制约条件，$N \le 3 \times 10^5$ 且 $\sum K_i \le 3 \times 10^5$，最坏情况下的计算量约为 $9 \times 10^{10}$ 次操作，这在 2 秒内是无法运行完的。


# AC Code，预处理arr
``` python
import sys  
from bisect import bisect_left, bisect_right  
  
  
def main():  
      
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
  
    for _ in range(m):  
        xrr = list(map(int, sys.stdin.readline().strip().split()))  
        xrr = xrr[1:]  
        arr.append(xrr)  
  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 建立 food 和 day 的 映射表  
    brr_rule = dict()  
    for i in range(n):  
        brr_rule[brr[i]] = i  
  
    # 映射arr  
    for i in range(m):  
        arr[i] = [ brr_rule[num] for num in arr[i] ]  
  
    # 将arr压缩，只保留有用信息 最迟的一天  
    crr = [ max(nums) for nums in arr]  
  
    crr.sort()  
  
    # print(crr)  
  
    out = []  
    # 二分找 index    for day in range(1, n + 1):  
        idx = bisect_right(crr, day - 1)  
  
        out.append(idx)  
  
    print('\n'.join(map(str, out)))  
if __name__ == "__main__":  
    main()
```



### 1. 关注步骤的时间复杂度

```python
    for i in range(m):
        arr[i] = [ brr_rule[num] for num in arr[i] ]
```

* **分析**：
  * 外层循环执行 $M$ 次（即菜品的数量）。
  * 内层列表推导式遍历当前菜品 $i$ 的所有食材，执行次数等于该菜品的食材数 $K_i$。
  * `brr_rule[num]` 是对字典（哈希表）的查找操作，在 Python 中其平均时间复杂度为 $O(1)$。
  * 因此，这个步骤的总操作次数等于所有菜品的食材数量之和，即 $\sum_{i=1}^{M} K_i$。
* **复杂度**：**$O(\sum K_i)$**。
* **实际计算量**：根据题目制约条件，$\sum K_i \le 3 \times 10^5$。因此，这一步在 Python 中只需大约数十毫秒即可完成。

---

### 2. 整段代码的时间复杂度分析

我们将代码拆解为以下几个部分来计算总复杂度：

| 步骤 | 代码段 | 时间复杂度 | 说明 |
| :--- | :--- | :--- | :--- |
| **1. 输入读取** | `sys.stdin.readline` 循环 | $O(M + \sum K_i)$ | 读入所有菜品数据。 |
| **2. 建立映射表** | `brr_rule` 的字典生成 | $O(N)$ | 遍历一次长度为 $N$ 的 `brr`。 |
| **3. 食材转换** | 转换 `arr` 中的食材为天数（您关注的步骤） | $O(\sum K_i)$ | 对所有食材进行一次 $O(1)$ 的字典查询。 |
| **4. 压缩求最大值**| `crr = [ max(nums) for nums in arr]` | $O(\sum K_i)$ | 找出每道菜最晚克服的食材天数，各菜品耗时 $O(K_i)$。 |
| **5. 排序** | `crr.sort()` | $O(M \log M)$ | 对长度为 $M$ 的列表 `crr` 进行排序。 |
| **6. 二分查找** | `for day in range(1, n + 1)` 结合 `bisect_right` | $O(N \log M)$ | 进行 $N$ 次二分查找，每次查找消耗 $O(\log M)$。 |

#### 总体时间复杂度
将上述所有步骤相加，整段代码的总体时间复杂度为：
$$O(\sum K_i + M \log M + N \log M)$$
也可以写为：
$$O(\sum K_i + (N + M) \log M)$$

---

### 3. 运行效率评估

根据题目的最大制约条件（$N, M, \sum K_i \le 3 \times 10^5$）：
* $\sum K_i \approx 3 \times 10^5$
* $(N + M) \log_2 M \approx (6 \times 10^5) \times 19 \approx 1.14 \times 10^7$

总计算量大约在 $1.2 \times 10^7$ 次常数级操作左右。在 Python 中，这种简单的循环和内置的 C 语言实现函数（如 `sort`、`bisect`）运行效率极高，通常在 **0.1 秒 ~ 0.3 秒** 内即可执行完毕（题目限制为 2 秒），非常安全。