```python
import sys


def main():

    h, w, k = map(int, sys.stdin.readline().strip().split())

    # 读入网格（arr[i][j] 为整数 0 或 1）
    arr = []
    for i in range(h):
        arr.append(list(map(int, sys.stdin.readline().strip())))

    # --- 第一步：预处理“列方向的前缀和” ---
    # col_pref[i][j] 表示第 j 列中，从第 0 行到第 i-1 行的元素总和。
    # 这样，对于任意列 j，从第 r1 行到第 r2 行的元素和，就可以在 O(1) 时间内算出来：
    # col_pref[r2 + 1][j] - col_pref[r1][j]
    col_pref = [[0] * w for _ in range(h + 1)]
    for i in range(h):
        for j in range(w):
            col_pref[i + 1][j] = col_pref[i][j] + arr[i][j]

    ans = 0
    max_sum = h * w

    # freq 数组用于存放当前一维前缀和出现的次数。
    # 比如 freq[5] = 2 表示前缀和为 5 的位置之前出现了 2 次。
    freq = [0] * (max_sum + 1)

    # --- 第二步：枚举上下边界 r1 和 r2 ---
    for r1 in range(h):
        for r2 in range(r1, h):

            # --- 第三步：在固定的 [r1, r2] 行区间内，进行列压缩和一维子数组计数 ---
            curr_sum = 0  # 记录一维压缩数组的前缀和
            freq[0] = 1  # 初始化：前缀和为 0 的情况默认有 1 种（即什么都不选）
            history = [0]  # 记录本次循环修改过 freq 的哪些位置，方便后续 O(W) 快速复原

            # 遍历每一列 j
            for j in range(w):
                # 1. 压缩：利用预处理的列前缀和，快速算出第 j 列在 r1 到 r2 行之间的和
                col_sum = col_pref[r2 + 1][j] - col_pref[r1][j]

                # 2. 累加，得到当前的一维前缀和
                curr_sum += col_sum

                # 3. 计数：我们需要找之前出现过的某个前缀和，使得当前前缀和 - 之前前缀和 = K
                # 也就是 之前前缀和 = curr_sum - K
                target = curr_sum - k
                if 0 <= target <= max_sum:
                    ans += freq[target]  # 如果 target 存在，就把它的出现次数加到答案里

                # 4. 记录当前前缀和的出现次数
                freq[curr_sum] += 1
                history.append(curr_sum)

            # --- 第四步：复原 freq 数组 ---
            # 因为我们在最外层循环还要用 freq，所以必须清空它。
            # 为了避免每次都重新分配一个 250000 大小的数组（在 Python 中会超时），
            # 我们只把刚才修改过的位置重置为 0，这只需要 O(W) 的时间。
            for val in history:
                freq[val] = 0

    print(ans)


if __name__ == '__main__':
    main()
```