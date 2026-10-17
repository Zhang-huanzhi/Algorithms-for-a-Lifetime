# Gemini怎么说：

从纯算法和工程的视角来看，这份代码属于典型的**“带周期检测的模拟（Simulation with Cycle Detection）”**。

以下是不带修饰的客观分析：

### 1. 时间复杂度分析（最致命的短板）
*   **单步时间开销**：每一次 `while` 循环内部，你需要遍历整个网格计算 `crr`，时间复杂度为 $\mathcal{O}(HW)$。此外，`crr == arr` 的比较以及网格深拷贝 `[row[:] for ...]` 也是 $\mathcal{O}(HW)$。因此，每推演一步的复杂度为 $\mathcal{O}(HW)$。
*   **收敛步数（达到循环的条件）**：由于波纹每次只能向外扩散 1 格，网格必须被波纹完全覆盖后，才会进入周期为 2 的全局死循环。覆盖全图所需的最大步数等于网格的最大维度 $\max(H, W)$。
*   **总时间复杂度**：$\mathcal{O}(HW \times \max(H, W))$。
*   **致命数据点**：当输入为 $H=1000, W=1000$ 时，$HW = 10^6$。要达到周期循环需要运行 $1000$ 步。总计算量约为 $10^6 \times 1000 = 10^9$ 次操作。在 Python 中，$10^9$ 次循环大概需要 10~20 秒，绝对无法通过 2.0 秒的时间限制。**结论：在极大图下必然 TLE。**

### 2. 空间复杂度分析
*   代码维护了三个大小为 $H \times W$ 的二维数组（`arr`, `brr`, `crr`）。
*   **总空间复杂度**：$\mathcal{O}(HW)$。
*   当 $H \times W = 10^6$ 时，三个数组占用内存极小（约几十 MB），远低于题目 1024 MiB 的限制。**结论：空间复杂度极其优秀，没有任何问题。**

### 3. 算法逻辑的优点（亮点）
*   **状态机设计准确**：完美分离了 $T_{N-1}$、$T_N$ 和 $T_{N+1}$，避免了同一数组边读边写造成的状态污染。
*   **周期截断逻辑严密**：
    *   `crr == brr` 准确捕捉了周期为 1 的稳态（如全白）。
    *   `crr == arr` 准确捕捉了周期为 2 的闪烁态。
    *   `remaining_steps % 2` 实现了 $\mathcal{O}(1)$ 的时间跨越，成功把 $10^{100}$ 压缩到了常数次状态赋值。这是解决超大步数问题的标准通用思维（找规律取模）。

### 4. 代码工程实现上的瑕疵
*   **步数定义的微小 Bug**：
    题目要求进行 $10^{100}$ 次操作。你写了 `total_steps = 10 ** 100 + 1`。这会导致最终计算奇偶性时产生 1 的偏差，输出相反的黑白结果。应改为 `total_steps = 10 ** 100`。
*   **高昂的内存拷贝开销**：
    ```python
    arr = [row[:] for row in brr]
    brr = [row[:] for row in crr]
    ```
    Python 中列表的深拷贝非常耗时。由于每一轮 `crr` 都是全新创建的，你完全可以直接使用**引用交换**来推进状态，将 $\mathcal{O}(HW)$ 的拷贝开销降为 $\mathcal{O}(1)$：
    ```python
    arr = brr
    brr = crr
    # 下一轮循环开头 crr = [['.'] * w ...] 会生成新对象，不会覆盖 brr
    ```
*   **硬编码的方向判断**：
    8 个 `elif` 分支虽然在 Python 中可能比 `for` 循环遍历方向数组稍微快一点点（减少了迭代器开销），但代码冗余度高，极易写错下标（比如写错一个 `+` 或 `-` 就 WA，且极难排查）。

### 总结评价
**这是一份逻辑上 100% 跑得通，但受限于问题物理特性而达到性能瓶颈的代码。** 

在算法竞赛中，这类解法被称为**“正解的次优替代品”**。如果这道题的规模是 $H, W \le 100$，这份代码可以直接 AC。但在 $10^6$ 的规模下，通过模拟寻找周期的思路（图论视角的遍历）必然敌不过直接计算距离的思路（图论视角的最短路）。

# Code
``` python
import sys


def main():
    h, w = map(int, sys.stdin.readline().strip().split())

    brr = []
    for _ in range(h):
        brr.append(list(sys.stdin.readline().strip()))

    # arr 存上一步 (N-1), brr 存现状 (N), crr 存下一步 (N+1)
    arr = None

    total_steps = 10 ** 100 + 1
    current_step = 0

    while current_step < total_steps:
        # 每一轮必须拿一张全新的“白纸”来画下一个状态
        crr = [['.'] * w for _ in range(h)]

        # 根据现状 brr，推算下一步 crr
        for i in range(h):
            for j in range(w):
                if brr[i][j] == '.':
                    # 这个方块的四周是否有黑的
                    if i > 0 and j > 0 and brr[i - 1][j - 1] == '#':
                        crr[i][j] = '#'
                    elif i > 0 and brr[i - 1][j] == '#':
                        crr[i][j] = '#'
                    elif i > 0 and j + 1 < w and brr[i - 1][j + 1] == '#':
                        crr[i][j] = '#'
                    elif j > 0 and brr[i][j - 1] == '#':
                        crr[i][j] = '#'
                    elif j + 1 < w and brr[i][j + 1] == '#':
                        crr[i][j] = '#'
                    elif i + 1 < h and j > 0 and brr[i + 1][j - 1] == '#':
                        crr[i][j] = '#'
                    elif i + 1 < h and brr[i + 1][j] == '#':
                        crr[i][j] = '#'
                    elif i + 1 < h and j + 1 < w and brr[i + 1][j + 1] == '#':
                        crr[i][j] = '#'
                elif brr[i][j] == '#':
                    crr[i][j] = '.'

        current_step += 1

        # 周期检测逻辑：
        # 1. 如果下一步等于现状（周期为1，比如全变白了永远不动了）
        if crr == brr:
            break

        # 2. 如果下一步等于上一步（周期为2，开始反复横跳了）
        if arr is not None and crr == arr:
            # 找到了循环！接下来只需要看剩余的步数是奇数还是偶数
            remaining_steps = total_steps - current_step
            if remaining_steps % 2 == 1:
                # 如果还剩奇数步，结果就是现状 (brr) 的下一次变化，因为 crr 已经算出来了，直接把 crr 给 brr
                brr = [row[:] for row in crr]
            else:
                # 如果还剩偶数步，因为是周期2，偶数步后的结果就等于现状 (brr)
                pass
            break

        # 状态向后滚动：上一步 = 现状，现状 = 下一步
        # 注意必须用深拷贝 [row[:] for row ...]，否则会陷入 Python 的引用陷阱
        arr = [row[:] for row in brr]
        brr = [row[:] for row in crr]

    # 输出最终结果
    for row in brr:
        print("".join(row))


if __name__ == '__main__':
    main()
```