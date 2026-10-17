``` python
import itertools
import math
import sys
from collections import deque, defaultdict

sys.setrecursionlimit(10 ** 9)


def main():
    # 读取测试用例数量
    T = int(sys.stdin.readline().strip())

    for _ in range(T):
        n = int(sys.stdin.readline().strip())
        s = sys.stdin.readline().strip()

        num_states = 1 << n  # 2^n
        target_state = num_states - 1  # 目标状态（二进制全是1，比如 n=3 时为 7）

        # 预先计算好每种药代表的数值，避免在循环中重复计算 2**med
        # pow2 = [1, 2, 4, 8, 16, ...]
        pow2 = [1 << i for i in range(n)]

        # 用布尔列表代替集合 set，速度极快
        visited = [False] * num_states
        visited[0] = True  # 空瓶状态已访问

        # 队列中直接存整数（代表当前的配方状态）
        queue = deque([0])
        possible = False

        while queue:
            curr_state = queue.popleft()

            # 成功配出所有药
            if curr_state == target_state:
                possible = True
                break

            # 尝试往当前瓶子里加入一种新的药
            for med in range(n):
                # 检查第 med 种药是否已经加过了
                # (curr_state & pow2[med]) == 0 表示瓶子里还没有这种药
                if (curr_state & pow2[med]) == 0:
                    # “加药”操作：直接做加法得到新状态
                    next_state = curr_state + pow2[med]

                    # 如果这个新状态没访问过，且在 s 中是安全的 ('0')
                    # s 是 0-indexed 的，所以下标是 next_state - 1
                    if not visited[next_state] and s[next_state - 1] == '0':
                        visited[next_state] = True
                        queue.append(next_state)

        if possible:
            print("Yes")
        else:
            print("No")


if __name__ == '__main__':
    main()
```