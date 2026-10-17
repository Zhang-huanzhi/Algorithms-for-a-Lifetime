# 暴力Code TLE
``` python
import sys

def main():

    n = int(sys.stdin.readline().strip())

    total = 2000 * 2000
    arr = []
    out = []
    ans = 0
    grip = [[0] * (2000 + 1) for _ in range(2000 + 1)]

    # 二维差分？
    # 算暴力试试
    for _ in range(n):
        u, d, l, r = map(int, sys.stdin.readline().strip().split())
        arr.append((u,d, l, r))

        for i in range(u, d + 1):
            for j in range(l, r + 1):
                if grip[i][j] == 0:
                    ans += 1

                grip[i][j] += 1


    # 模拟删除
    for k in range(n):
        u, d, l, r = arr[k]
        grip_copy = [row[:] for row in grip]
        ans_copy = ans
        for i in range(u, d + 1):
            for j in range(l, r + 1):

                grip_copy[i][j] -= 1
                if grip_copy[i][j] == 0:
                    ans_copy -= 1

        out.append(total - ans_copy)

    print('\n'.join(map(str, out)))


if __name__ == "__main__":
    main()
```


# 二维差分 + 标记 AC
``` python
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    edge = 2000  
    total = edge * edge  
    arr = []  
  
    # grip 用于记录每个格子被覆盖的次数  
    diff = [[0] * (edge + 2) for _ in range(edge + 2)]  
    grip = [[0] * (edge + 2) for _ in range(edge + 2)]  
  
    # grip_id 用于记录覆盖该格子的云的编号之和  
    diff_id = [[0] * (edge + 2) for _ in range(edge + 2)]  
    grip_id = [[0] * (edge + 2) for _ in range(edge + 2)]  
  
    # 1. 读入数据并进行二维差分  
    for k in range(1, n + 1):  # 编号 k 从 1 到 N        u, d, l, r = map(int, sys.stdin.readline().strip().split())  
        arr.append((u, d, l, r))  
  
        # 针对覆盖次数的差分 (加 1)        diff[u][l] += 1  
        diff[u][r + 1] -= 1  
        diff[d + 1][l] -= 1  
        diff[d + 1][r + 1] += 1  
  
        # 针对云编号的差分 (加 k)        diff_id[u][l] += k  
        diff_id[u][r + 1] -= k  
        diff_id[d + 1][l] -= k  
        diff_id[d + 1][r + 1] += k  
  
    # 2. 还原 grip 和 grip_id (注意循环从 1 开始，避免 i-1 变成 -1)    for i in range(1, edge + 1):  
        for j in range(1, edge + 1):  
            grip[i][j] = (  
                grip[i - 1][j]  
                + grip[i][j - 1]  
                - grip[i - 1][j - 1]  
                + diff[i][j]  
            )  
            grip_id[i][j] = (  
                grip_id[i - 1][j]  
                + grip_id[i][j - 1]  
                - grip_id[i - 1][j - 1]  
                + diff_id[i][j]  
            )  
  
    # 3. 统计结果  
    uncovered_total = 0  # 全局未被任何云覆盖的格子数  
    only_covered_by = [0] * (n + 1)  # only_covered_by[k] 表示仅被云 k 覆盖的格子数  
  
    for i in range(1, edge + 1):  
        for j in range(1, edge + 1):  
            if grip[i][j] == 0:  
                uncovered_total += 1  
            elif grip[i][j] == 1:  
                # 覆盖数正好为 1 时，对应的 grip_id[i][j] 就是那朵云的编号  
                cloud_id = grip_id[i][j]  
                only_covered_by[cloud_id] += 1  
  
    # 4. 生成每朵云被删除后的答案  
    out = []  
    for k in range(1, n + 1):  
        # 移除云 k 后的未覆盖面积 = 原本就没覆盖的 + 只被云 k 覆盖的  
        ans = uncovered_total + only_covered_by[k]  
        out.append(ans)  
  
    # 输出结果  
    print("\n".join(map(str, out)))  
  
  
if __name__ == "__main__":  
    main()
```

# 优化代码
``` python
import sys


def main():
    # 一次性读入所有输入，在 Python 中效率极高
    input_data = sys.stdin.read().split()
    if not input_data:
        return

    n = int(input_data[0])
    edge = 2000

    # 只需要 2 个二维数组（A: 覆盖次数, B: 云 ID 之和）
    A = [[0] * (edge + 2) for _ in range(edge + 2)]
    B = [[0] * (edge + 2) for _ in range(edge + 2)]

    # 1. 读入并进行二维差分
    idx = 1
    for k in range(1, n + 1):
        u = int(input_data[idx])
        d = int(input_data[idx + 1])
        l = int(input_data[idx + 2])
        r = int(input_data[idx + 3])
        idx += 4

        # 差分打标记
        A[u][l] += 1
        A[u][r + 1] -= 1
        A[d + 1][l] -= 1
        A[d + 1][r + 1] += 1

        B[u][l] += k
        B[u][r + 1] -= k
        B[d + 1][l] -= k
        B[d + 1][r + 1] += k

    # 2. 原地求前缀和（先横向累加，再纵向累加）
    # 横向累加
    for i in range(1, edge + 1):
        for j in range(1, edge + 1):
            A[i][j] += A[i][j - 1]
            B[i][j] += B[i][j - 1]

    # 纵向累加
    for i in range(1, edge + 1):
        for j in range(1, edge + 1):
            A[i][j] += A[i - 1][j]
            B[i][j] += B[i - 1][j]

    # 3. 遍历网格统计结果
    uncovered = 0
    only_covered = [0] * (n + 1)

    for i in range(1, edge + 1):
        for j in range(1, edge + 1):
            if A[i][j] == 0:
                uncovered += 1
            elif A[i][j] == 1:
                only_covered[B[i][j]] += 1

    # 4. 简洁地生成并输出答案
    out = [uncovered + only_covered[k] for k in range(1, n + 1)]
    print("\n".join(map(str, out)))


if __name__ == "__main__":
    main()
```