# 优化点

**利用历史状态，避免重复计算**

其实算是增量更新

核心优化点：用一个数组保存了上一步的状态，这样就不至于每次都需要重新建栈，消除了

| 维度 | 暴力模拟 | 双栈Code | 优化效果 |
| :--- | :--- | :--- | :--- |
| **单次操作时间** | $O(N)$ （与当前字符串长度成正比） | **$O(1)$** （恒定常数时间） | **极大提升**，彻底解决超时 |
| **总体时间复杂度**| $O(Q^2)$ | **$O(Q)$** | 从无法运行到 0.5 秒内解决 |
| **空间复杂度** | $O(Q)$ （每次 `check` 还要频繁创建和销毁临时列表） | **$O(Q)$** （只有两个稳定增长的栈，内存分配更平稳） | **显著改善**，降低了内存抖动 |
| **I/O 耗时** | 慢（执行 $Q$ 次 `print`） | **极快**（执行 1 次整体 `print`） | **明显提升**，避免 I/O 瓶颈 |

双栈代码的优化非常标准且实用，它展示了如何通过**合理的数学建模**和**状态存储（空间换时间）**，将一个低效的模拟算法改造成契合竞赛要求的方案。

# 暴力模拟

``` python
import sys

def main():

    q = int(sys.stdin.readline().strip())

    # 栈
    arr = []

    # 检查是否合法（
    def check(arr):
        xrr = list(arr)
        yrr = []

        while xrr:
            x = xrr.pop()  
            if x == ')':
                yrr.append(')')
            elif yrr:
                yrr.pop()
            else:
                return False

        if not xrr and not yrr:
            return True

        return False

    for _ in range(q):
        line = sys.stdin.readline().strip().split()

        op = line[0]

        if op == '1':
            c = line[1]
            arr.append(c)

            if check(arr):
                print("Yes")
            else:
                print("No")

        if op == '2':
           arr.pop()
           if check(arr):
               print("Yes")
           else:
               print("No")


if __name__ == "__main__":
    main()
```


# 双栈Code
``` python
import sys

def main():

    q = int(sys.stdin.readline().strip())

    # 栈
    arr = [0]
    brr = [0]

    results = []
    for _ in range(q):
        line = sys.stdin.readline().strip().split()

        op = line[0]

        if op == '1':
            c = line[1]

            step = 1 if c == '(' else -1
            curr = arr[-1] + step

            cnt = min(brr[-1], curr)

            arr.append(curr)
            brr.append(cnt)

        if op == '2':
            arr.pop()
            brr.pop()

        if arr[-1] == 0 and brr[-1] == 0:
            results.append("Yes")
        else:
            results.append("No")

    print('\n'.join(results))
if __name__ == "__main__":
    main()
```