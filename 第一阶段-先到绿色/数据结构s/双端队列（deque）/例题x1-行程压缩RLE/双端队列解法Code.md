``` python
import sys
from collections import deque


def solve():
    
    q = int(sys.stdin.readline().strip())

    arr = deque()

    for _ in range(q):
        line = sys.stdin.readline().strip().split()

        op = line[0]

        if op == '1':
            c = int(line[1])
            x = int(line[2])
            arr.append([c, x]) 

        if op == '2':
            k = int(line[1])
            ans = 0

            # 直接从 arr 的头部消费元素，直到删除了 k 个
            while k > 0:
                cnt, val = arr[0]
                if k >= cnt:
                    # 当前整组都被删完
                    ans += cnt * val
                    k -= cnt
                    arr.popleft()  # 物理删除，下次不会再遍历到它
                else:
                    # 只需要删当前组的一部分
                    ans += k * val
                    arr[0][0] -= k  # 更新这组剩余的数量
                    k = 0

            print(ans)


if __name__ == '__main__':
    solve()
```