``` python
import sys

def solve():
    q = int(sys.stdin.readline().strip())

    arr = []
    head_idx = 0  # 记录当前未删除的头部元素的下标

    for _ in range(q):
        line = sys.stdin.readline().strip().split()

        op = line[0]

        if op == '1':
            c = int(line[1])
            x = int(line[2])
            # 使用列表 [c, x] 存储，以便后续可以直接修改 c 的值
            arr.append([c, x])

        elif op == '2':
            k = int(line[1])
            ans = 0

            while k > 0:
                cnt, val = arr[head_idx]
                if k >= cnt:
                    # 这一组全部被删除，指针 head_idx 向后移动
                    ans += cnt * val
                    k -= cnt
                    head_idx += 1  # 相当于逻辑上的 popleft()
                else:
                    # 这一组只被删除了一部分，直接修改 arr 中的剩余数量
                    ans += k * val
                    arr[head_idx][0] -= k
                    k = 0

            print(ans)

if __name__ == '__main__':
    solve()
```