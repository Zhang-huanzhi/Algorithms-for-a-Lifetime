https://atcoder.jp/contests/abc483/tasks/abc483_d

---
### 题目描述

数轴上有一个机器人。最初，机器人位于坐标 $0$ 处。此外，高桥君拥有一个长度为 $N$ 的整数序列 $A = (A_1, A_2, \dots, A_N)$。

高桥君将进行以下操作：

1. **至多选择一个** $A$ 中的元素，并将其正负号反转。也就是说，选择一个满足 $1 \le i \le N$ 的下标 $i$（或者不选），将 $A_i$ 修改为 $-A_i$。
2. 启动机器人。机器人启动后将进行 $N$ 次移动。在第 $i$ 次（$1 \le i \le N$）移动中，设机器人当前所在坐标为 $x$，它将沿直线移动到坐标 $x + A_i$。

高桥君希望让机器人**至少经过一次的整数坐标的数量**尽可能多。
请计算高桥君采取最优操作时，机器人至少经过一次的整数坐标数量的最大可能值。

注意：机器人是连续移动的。如果机器人在某次移动中从坐标 $S$ 移动到坐标 $T$：
- 若 $S \le T$，则视为经过了所有满足 $S \le x \le T$ 的整数坐标 $x$；
- 若 $S > T$，则视为经过了所有满足 $T \le x \le S$ 的整数坐标 $x$。

### 数据范围

- $1 \le N \le 2 \times 10^5$
- $-10^9 \le A_i \le 10^9$
- $A_i \neq 0$
- 输入的所有数值均为整数。

### 输入格式

输入通过标准输入按以下格式给出：

```text
N
A_1 A_2 \dots A_N
```

### 输出格式

输出高桥君采取最优操作时，机器人至少经过一次的整数坐标数量的最大值。

---

### 样例说明 1

#### 输入样例 1
```text
3
-2 3 -2
```

#### 输出样例 1
```text
8
```

最初序列为 $A = (-2, 3, -2)$。如果高桥君将 $A_2$ 的符号反转，序列变为 $A = (-2, -3, -2)$。此时机器人的行动如下：
- 第 1 次移动：从 $0$ 移动到 $-2$，经过的整数坐标为 $-2, -1, 0$。
- 第 2 次移动：从 $-2$ 移动到 $-5$，经过的整数坐标为 $-5, -4, -3, -2$。
- 第 3 次移动：从 $-5$ 移动到 $-7$，经过的整数坐标为 $-7, -6, -5$。

因此，机器人至少经过一次的整数坐标为 $-7, -6, -5, -4, -3, -2, -1, 0$，共 $8$ 个，这也是可能的最大值，所以输出 $8$。

如果高桥君不反转任何元素的符号，机器人将按照 $0 \to -2 \to 1 \to -1$ 移动，经过的整数坐标为 $-2, -1, 0, 1$，共 $4$ 个。

---

### 样例说明 2

#### 输入样例 2
```text
2
2 3
```

#### 输出样例 2
```text
6
```

若不对 $A$ 进行任何修改，机器人按照 $0 \to 2 \to 5$ 移动，经过的整数坐标为 $0, 1, 2, 3, 4, 5$，共 $6$ 个。
这就是最大值，因此输出 $6$。


# AC Code 前后缀
```python
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    s = [0] * (n + 1)  
    for i in range(n):  
        s[i + 1] = s[i] + arr[i]  
  
    max_s = max(s)  
    min_s = min(s)  
    ans = max_s - min_s + 1  
  
    pref_max = [0] * (n + 1)  
    pref_min = [0] * (n + 1)  
    pref_max[0] = s[0]    
    pref_min[0] = s[0]  
    for i in range(1, n + 1):  
        pref_max[i] = max(pref_max[i - 1], s[i])  
        pref_min[i] = min(pref_min[i - 1], s[i])  
  
    suff_max = [0] * (n + 1)  
    suff_min = [0] * (n + 1)  
    suff_max[n] = s[n]  
    suff_min[n] = s[n]  
    for i in range(n - 1, -1, -1):  
        suff_max[i] = max(suff_max[i + 1], s[i])  
        suff_min[i] = min(suff_min[i + 1], s[i])  
  
    for k in range(n):  
        left_max = pref_max[k]  
        left_min = pref_min[k]  
  
        diff = 2 * arr[k]  
        right_max = suff_max[k + 1] - diff  
        right_min = suff_min[k + 1] - diff  
  
        curr_max = max(left_max, right_max)  
        curr_min = min(left_min, right_min)  
  
        ans = max(ans, curr_max - curr_min + 1)  
  
    print(ans)  
if __name__ == "__main__":  
    main()
```

# AC Code 动态规划
``` python
from itertools import accumulate
import sys


def main():
    input = sys.stdin.read
    data = list(map(int, input().split()))
    if not data:
        return

    n = data[0]
    arr = data[1:]

    # 1. 前缀和：[0, S1, S2, ..., Sn]
    s = [0] + list(accumulate(arr))

    # 2. 前缀最大/最小值（一行搞定，不会有任何下标Bug）
    pref_max = list(accumulate(s, max))
    pref_min = list(accumulate(s, min))

    # 3. 后缀最大/最小值（从右往左做 accumulate 再翻转回来）
    suff_max = list(accumulate(s[::-1], max))[::-1]
    suff_min = list(accumulate(s[::-1], min))[::-1]

    # 不做修改时的基础答案
    ans = pref_max[-1] - pref_min[-1] + 1

    # 4. 枚举反转哪一个数（枚举 arr 中的每一个元素）
    # 反转 arr[k] 时：
    # 前半截对应 s[0...k] -> pref_max[k]
    # 后半截对应 s[k+1...n] -> suff_max[k+1] - 2*arr[k]
    for k, x in enumerate(arr):
        diff = 2 * x

        cur_max = max(pref_max[k], suff_max[k + 1] - diff)
        cur_min = min(pref_min[k], suff_min[k + 1] - diff)

        ans = max(ans, cur_max - cur_min + 1)

    print(ans)


if __name__ == "__main__":
    main()
```