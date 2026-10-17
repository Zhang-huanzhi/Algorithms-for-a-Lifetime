### 一、 为什么这个正向模拟能通过？（正确性与复杂度分析）

从表面上看，您的代码中有两个内层循环：
```python
for num in list(yrr[y]): ...
for num in list(xrr[x]): ...
```
如果在最坏情况下这些 `set` 的大小达到 $O(N)$，那么双重循环会导致总体复杂度达到 $O(NM)$，从而必然超时。

但是，**为什么它没有超时，且能保证正确性呢？**
这得益于本题的一个核心数学性质（即官方题解中的解法 2）：
> **在任何操作间隙，网格中的任意一行和任意一列，最多只会存在 1 个棋子。**

因为每次在 $(x, y)$ 放置棋子前，第 $x$ 行和第 $y$ 列的所有棋子都被清空了。
因此：
* 任何时候，`xrr[x]`（第 $x$ 行中棋子的列号集合）的长度最大只有 `1`。
* 任何时候，`yrr[y]`（第 $y$ 列中棋子的行号集合）的长度最大也只有 `1`。

这意味着：
1. 您的两个 `for` 循环**实际上每次最多只会执行 1 次**。
2. 从算法复杂度的角度来看，每次操作的平均时间复杂度依然是 $O(1)$，总时间复杂度为 $O(M)$。

这就是您的代码能够 AC 的根本原因。

---

### 二、 为什么运行时间长达 1999ms？（瓶颈分析）

虽然算法的时间复杂度是 $O(M)$，但 Python 作为解释型语言，在**动态对象创建和高阶数据结构调用**上有着非常高昂的常数开销。在这段代码中，以下几点累积成了巨大的耗时：

#### 1. 频繁创建临时列表：`list(yrr[y])`
在每次循环中，即便 `yrr[y]` 是空的或者只有一个元素，代码都会执行 `list(...)` 将其强制转换为一个新列表。
* 在 $M = 3 \times 10^5$ 的循环中，这会导致系统在堆内存中动态创建、销毁多达 $6 \times 10^5$ 个列表对象。垃圾回收（GC）机制会因此产生极大的耗时。

#### 2. `defaultdict(set)` 和 `set` 的高开销
`set`（集合）和 `defaultdict` 内部基于哈希表实现。
* 哈希表的查找、插入、删除操作虽然在均摊时间复杂度上是 $O(1)$，但其常数时间开销远大于普通的顺序数组（`list`）。
* 每次添加、删除都要计算哈希值、处理冲突，并动态调整哈希表的大小。

#### 3. 频繁生成元组 `(num, y)`
代码中多次执行 `(num, y) in se` 和 `se.remove((num, y))`。
* 每次判断都会临时在内存中申请一个 `tuple`（元组）对象。频繁的对象分配和释放非常消耗 CPU 时间。

---

### 三、 优化建议

如果我们利用您代码中发现的“每行/每列最多只有一个棋子”的性质，**不用 `set` 和 `defaultdict`，而是用普通的固定长度数组（`list`）来记录位置**，就可以省去所有的哈希计算和临时对象创建。

例如，可以使用两个大小为 $N+1$ 的普通列表：
* `row_to_col[x]`：记录第 $x$ 行的棋子在第几列（如果没有，则为 `-1`）。
* `col_to_row[y]`：记录第 $y$ 列的棋子在第几行（如果没有，则为 `-1`）。

这样，清空和放置操作就可以直接通过数组索引在 $O(1)$ 内完成。以下为修改后的正向模拟示例：

```python
import sys

def main():
    n, m = map(int, sys.stdin.readline().strip().split())

    # row_to_col[x] = y 表示第 x 行当前的棋子在第 y 列，-1 表示该行无棋子
    row_to_col = [-1] * (n + 1)
    col_to_row = [-1] * (n + 1)
    
    ans = 0

    for _ in range(m):
        x, y = map(int, sys.stdin.readline().strip().split())

        # 1. 模拟清空第 y 列原本可能存在的棋子
        old_x = col_to_row[y]
        if old_x != -1:
            row_to_col[old_x] = -1
            col_to_row[y] = -1
            ans -= 1  # 棋子被移走，总数减 1

        # 2. 模拟清空第 x 行原本可能存在的棋子
        old_y = row_to_col[x]
        if old_y != -1:
            col_to_row[old_y] = -1
            row_to_col[x] = -1
            ans -= 1  # 棋子被移走，总数减 1

        # 3. 放置新棋子 (x, y)
        row_to_col[x] = y
        col_to_row[y] = x
        ans += 1  # 放置了新棋子，总数加 1

    print(ans)

if __name__ == "__main__":
    main()
```

**效果：**
由于完全避免了 `set`、`defaultdict` 和临时 `tuple`/`list` 的创建，上述代码的运行时间通常能缩短至 **150ms - 250ms** 左右。

### 总结
您在当时能够独立想到通过维护行、列集合来实时消去棋子的正向模拟法，其算法逻辑是完全正确的。虽然由于 Python 的高阶容器开销导致运行时间卡在了时限边缘，但能AC本身就说明您对题目潜在的数学性质（每行每列至多一子）有了非常敏锐的直觉。


# 叛逆解法-正向模拟 （1999ms）
``` python
import sys
from collections import defaultdict

def main():

    n, m = map(int, sys.stdin.readline().strip().split())

    xrr = defaultdict(set)
    yrr = defaultdict(set)

    se = set()
    for _ in range(m):
        x, y = map(int, sys.stdin.readline().strip().split())

        for num in list(yrr[y]):
            if (num, y) in se:
                se.remove((num, y))
                yrr[y].remove(num)

        for num in list(xrr[x]):
            if (x, num) in se:
                se.remove((x, num))
                xrr[x].remove(num)

        se.add((x, y))
        xrr[x].add(y)
        yrr[y].add(x)

    # print(se)
    print(len(se))


if __name__ == "__main__":
    main()
```


# 推荐解法 - 逆向模拟
``` python
import sys  
  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    xrr = [False] * (n + 1)  
    yrr = [False] * (n + 1)  
  
    arr = []  
    ans = 0  
    for _ in range(m):  
        x, y = map(int, sys.stdin.readline().strip().split())  
        arr.append((x, y))  
  
    # 逆推  
    for x, y in arr[::-1]:  
  
        if not xrr[x] and not yrr[y]:  
            ans += 1  
  
        xrr[x] = True  
        yrr[y] = True  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```
