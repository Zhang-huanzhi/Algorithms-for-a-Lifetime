`itertools.permutations` 在 C 语言底层使用的是**非递归的迭代算法**
在学算法时，老师一定教的是递归回溯。但 itertools 使用迭代（非递归）有几个原因：

1. **消除递归开销**：C 语言中频繁的函数调用（压栈/出栈）会有额外开销。
2.  **状态保存**：迭代器需要“停下来”。当你 for p in permutations(...) 时，程序每次只计算一个排列。迭代算法通过 indices 和 cycles 两个数组完美记录了当前停在哪个位置，下次进来直接接着跑。
---
### Python 仿真版
这是官方文档中给出的逻辑模拟
``` python
def my_permutations(iterable, r=None):
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    if r > n:
        return

    indices = list(range(n))
    cycles = list(range(n, n-r, -1)) # 核心：控制交换的计数器

    # 第一个排列
    yield tuple(pool[i] for i in indices[:r])

    while n:
        # 从后往前寻找可以交换的位置
        for i in reversed(range(r)):
            cycles[i] -= 1
            if cycles[i] == 0:
                # 这一位已经试过了所有可能，将 indices[i:] 循环左移
                # 相当于把当前元素踢到最后，恢复部分有序状态
                indices[i:] = indices[i+1:] + indices[i:i+1]
                cycles[i] = n - i # 重置计数器
            else:
                j = cycles[i]
                # 将第 i 位与倒数第 j 位交换
                indices[i], indices[-j] = indices[-j], indices[i]
                yield tuple(pool[i] for i in indices[:r])
                break # 找到了下一个排列，跳出 for 循环，等待下一次 next()
        else:
            # 所有的 cycles 都归零了，说明全部排列完成
            return
```