https://atcoder.jp/contests/abc436/tasks/abc436_d

---

### D - Teleport Maze (传送迷宫)
时间限制: 2 秒 / 内存限制: 1024 MiB

分值: 400 分

#### 题目描述
有一个包含 $H$ 行 $W$ 列网格的迷宫。我们用 $(i, j)$ 表示从上往下数第 $i$ 行、从左往右数第 $j$ 列的格子。格子 $(i, j)$ 的类型由字符 $S_{i, j}$ 表示，每个字符的具体含义如下：

*   `.`：空地格子
*   `#`：障碍物格子
*   英文字母（`a` - `z`）：传送格子

在迷宫中，你可以按任意顺序执行以下两种操作任意次数：

*   **步行**：从当前所在格子向上下左右四个方向之一移动 $1$ 格。但是，不能移动到障碍物格子，也不能移动到网格范围之外。
*   **传送**：当你处于某个传送格子时，可以移动到与其写有相同字母的任意传送格子。

请判断是否可以从格子 $(1, 1)$ 移动到格子 $(H, W)$。如果可以，求出所需的最小总操作次数；如果不能，输出 `-1`。

#### 数据范围
*   $1 \leq H, W \leq 1000$
*   $H \times W \geq 2$
*   $H, W$ 是整数
*   $S_{i, j}$ 为 `.`, `#` 或小写英文字母
*   $S_{1, 1} \neq$ `#`
*   $S_{H, W} \neq$ `#`

#### 输入格式
输入按以下格式从标准输入给出：

$$
\begin{array}{l}
H \quad W \\
S_{1,1} S_{1,2} \dots S_{1,W} \\
\vdots \\
S_{H,1} S_{H,2} \dots S_{H,W}
\end{array}
$$

#### 输出格式
如果可以从格子 $(1, 1)$ 移动到 $(H, W)$，输出所需的最小总操作次数；如果不可能，则输出 `-1`。

---

#### 样例输入 1
```text
3 4
..a.
####
ba#b
```

#### 样例输出 1
```text
5
```

通过以下操作，可以从格子 $(1, 1)$ 移动到 $(3, 4)$：
1. 步行：从 $(1, 1)$ 移动到 $(1, 2)$。
2. 步行：从 $(1, 2)$ 移动到 $(1, 3)$。
3. 传送：从 $(1, 3)$ 移动到 $(3, 2)$（两者都是字符 `a` 所在的格子）。
4. 步行：从 $(3, 2)$ 移动到 $(3, 1)$。
5. 传送：从 $(3, 1)$ 移动到 $(3, 4)$（两者都是字符 `b` 所在的格子）。

此时的总操作次数为 $5$ 次，这是最少的步数。

---

#### 样例输入 2
```text
3 4
..a.
####
b.#b
```

#### 样例输出 2
```text
-1
```
无法从格子 $(1, 1)$ 移动到 $(3, 4)$。

---

#### 样例输入 3
```text
4 4
xxxx
xxxx
xxxx
xxxx
```

#### 样例输出 3
```text
1
```

---

#### 样例输入 4
```text
7 11
u..#y..#...
k..#.z.#.k.
iju#...#x..
###########
..x#.t.#..n
abc#y..#...
..z#..t#.y.
```

#### 样例输出 4
```text
12
```

# BFS基本功
``` python
import sys  
from collections import defaultdict, deque  
  
  
def main():  
  
    h, w = map(int, sys.stdin.readline().strip().split())  
  
    grip = []  
    for _ in range(h):  
        grip.append(list(sys.stdin.readline().strip()))  
  
    # 建一个传送表,用字典吧  
    arr = defaultdict(list)  
    for i in range(h):  
        for j in range(w):  
            curr = grip[i][j]  
            if curr != '.' and curr != '#':  
                arr[curr].append((i, j))  
  
    # print(arr)  
  
    # BFS搜索  
    q = deque()  
    # 三元组，x坐标，y坐标，步数  
    q.append((0, 0, 0))  
    visited = [ [-1] * w for _ in range(h)]  
    visited[0][0] = 0  
    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]  
  
    while q:  
        x, y, step = q.popleft()  
        if x == h - 1 and y == w - 1:  
            print(step)  
            break  
  
        # 操作一，走一个  
        for dx, dy in directions:  
            nx = x + dx  
            ny = y + dy  
            new_step = step + 1  
  
            if 0 <= nx < h and 0 <= ny < w:  
                if grip[nx][ny] != '#':  
                    if visited[nx][ny] == -1:  
                        visited[nx][ny] = new_step  
                        q.append((nx, ny, new_step))  
  
        # 操作二，传送  
        curr = grip[x][y]  
        if curr != '.':  
            for nx, ny in arr[curr]:  
                # print((nx, ny))  
                if visited[nx][ny] == -1:  
                    visited[nx][ny] = step + 1  
                    q.append((nx, ny, step + 1))  
            # 清空该字母的传送列表，避免重复检索导致超时  
            arr[curr] = []  
    else:  
        print(-1)  
  
if __name__ == "__main__":  
    main()
```