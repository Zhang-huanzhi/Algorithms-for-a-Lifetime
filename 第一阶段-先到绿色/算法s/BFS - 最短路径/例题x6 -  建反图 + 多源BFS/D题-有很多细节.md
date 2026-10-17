https://atcoder.jp/contests/abc472/tasks/abc472_d

---

### D - 疯狂轰炸 (Bomber Mad)

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：400 分

#### 题目描述

有一个 $H$ 行 $W$ 列的网格。每个格子要么是**空格子**，要么是**炸弹格子**。我们将从上往下第 $i$ 行、从左往右第 $j$ 列的格子记为 $(i, j)$。网格的信息由 $H$ 个长度为 $W$ 的字符串 $S_1, S_2, \dots, S_H$ 给出，其中：
- 若 $S_i$ 的第 $j$ 个字符是 `.`，则表示 $(i, j)$ 是空格子；
- 若 $S_i$ 的第 $j$ 个字符是 `#`，则表示 $(i, j)$ 是炸弹格子。

另外，对于一个空格子 $(i, j)$，如果第 $i$ 行和第 $j$ 列中**都没有炸弹格子**，则称该格子为 **“安全的空格子”**。

在一次移动中，你可以从当前所在的格子移动到上下左右相邻的空格子中（不能移动到炸弹格子上）。请计算满足以下条件的空格子 $(i, j)$ 的数量：

> 从 $(i, j)$ 出发，可以在 **$K$ 次以内（包含 $K$ 次）** 的移动中到达某个 **“安全的空格子”**。

#### 约束条件

- $1 \le H, W \le 5 \times 10^5$
- $H \times W \le 5 \times 10^5$
- $0 \le K \le H \times W - 1$
- $S_i$ 是由 `.` 和 `#` 组成的长度为 $W$ 的字符串
- $H, W, K$ 均为整数

#### 输入格式

输入通过标准输入按以下格式给出：

$$
H \quad W \quad K
$$
$$
S_1
$$
$$
S_2
$$
$$
\vdots
$$
$$
S_H
$$

#### 输出格式

输出满足条件的空格子的数量。

---

### 样例输入 1
```plain
3 3 1
#..
...
..#
```

### 样例输出 1
```plain
5
```
**样例 1 解释**：  
“安全的空格子” 只有 $(2,2)$。  
能够在 $1$ 次以内移动到达 $(2,2)$ 的空格子有 $(1,2), (2,1), (2,2), (2,3), (3,2)$ 共 5 个，因此答案为 5。

---

### 样例输入 2
```plain
2 3 0
...
...
```

### 样例输出 2
```plain
6
```
**样例 2 解释**：  
因为网格中没有炸弹，所以全部 6 个格子都是“安全的空格子”。因此，所有空格子在移动 0 次的情况下均满足条件。

---

### 样例输入 3
```plain
5 7 2
..#....
..#....
.......
...#...
...#...
```

### 样例输出 3
```plain
29
```


# 我的AC Code
``` python
import math
import sys
from collections import deque


def main():

    h, w, k = map(int, sys.stdin.readline().strip().split())

    arr = []
    for _ in range(h):
        xrr = list(sys.stdin.readline().strip())
        arr.append(xrr)

    # 这不是那啥， 多源BFS，逆路径，我记得做过
    # 从安全格扩张，走k步，走到的任何炸弹格都符合条件

    # 先算源，有哪些安全格
    xrr = []
    for i in range(h):
        for j in range(w):
            if arr[i][j] == '#':
                break
        else:
            xrr.append(i)
    yrr = []
    for j in range(w):
        for i in range(h):
            if arr[i][j] == '#':
                break
        else:
            yrr.append(j)

    q = deque()
    visited = [[-1] * w for _ in range(h)]
    for x in xrr:
        for y in yrr:
            q.append((x, y, 0))
            visited[x][y] = 0

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
    # 开始扩散
    while q:
        # bfs， 这题最短路径，应该不能dfs
        x, y, step = q.popleft()

        for dx, dy in directions:
            nx, ny = x + dx, y + dy

            if 0 <= nx < h and 0 <= ny < w:
                if arr[nx][ny] != '#' and visited[nx][ny] == -1:
                    q.append((nx, ny, step + 1))
                    visited[nx][ny] = step + 1

    # 算算有多少个大于k
    ans = 0
    for x in range(h):
        for y in range(w):
            if arr[x][y] != '#' and 0 <= visited[x][y] <= k:
                ans += 1

    print(ans)

if __name__ == "__main__":
    main()
```


# 