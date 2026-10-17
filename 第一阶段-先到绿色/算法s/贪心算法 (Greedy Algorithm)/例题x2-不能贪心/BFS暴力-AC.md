``` python
import itertools
import math
import sys
from collections import deque, defaultdict


sys.setrecursionlimit(10**9)

def main():

    n = int(sys.stdin.readline().strip())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    # 队列里存的是上一步的结果
    q = deque([(0.5, 0)])

    for i in range(n):
        # 【关键改动 1】：使用字典来收集这一轮产生的所有新状态
        # 它可以自动把到达同一个坐标的路径合并，只留次数最大的
        new_states = {}

        while q:
            dist, cnt = q.popleft()

            # 只有两种选择：减去 arr[i] 或 加上 arr[i]
            for next_dist in [dist - arr[i], dist + arr[i]]:
                # 【关键改动 2】：判断是否穿过 0，一行逻辑代替复杂的 if
                # 如果一正一负，相乘一定小于 0
                passed = 1 if dist * next_dist < 0 else 0
                new_cnt = cnt + passed

                # 如果这个坐标还没存过，或者当前次数比之前存的大，则更新
                if new_cnt > new_states.get(next_dist, -1):
                    new_states[next_dist] = new_cnt

        # 将合并后的状态重新放回队列，供下一轮移动使用
        for d, c in new_states.items():
            q.append((d, c))

    # 最后在所有状态里找最大的次数
    ans = 0
    while q:
        _, cnt = q.popleft()
        if cnt > ans:
            ans = cnt
    print(ans)


if __name__ == '__main__':
    main()

```