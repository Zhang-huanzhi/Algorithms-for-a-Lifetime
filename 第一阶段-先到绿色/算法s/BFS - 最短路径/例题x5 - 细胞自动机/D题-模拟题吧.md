https://atcoder.jp/contests/abc425/tasks/abc425_d

---

### D - Ulam-Warburton Automaton

时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 425 分

#### 问题描述

有一个 $H$ 行 $W$ 列的网格。自上而下第 $i$ 行 $(1 \le i \le H)$、自左而右第 $j$ 列 $(1 \le j \le W)$ 的格子记为格子 $(i, j)$。

初始时，如果 $S_{i,j}$ 为 `#`，则格子 $(i,j)$ 被涂成黑色；如果为 `.`，则被涂成白色。

重复执行以下操作 $10^{100}$ 次：

- 设所有当前被涂成白色、且与其共边相邻的格子中**恰好有一个**是被涂成黑色的格子的集合为 $T$。
- 将 $T$ 中的每个格子涂成黑色。其中，两个格子 $(i_1,j_1)$ 和 $(i_2,j_2)$ 共边相邻是指满足 $|i_1-i_2|+|j_1-j_2|=1$。

请计算在所有操作结束后，被涂成黑色的格子总数。

#### 约束条件

- $2 \le H, W$
- $HW \le 3 \times 10^5$
- $H, W$ 为整数
- $S_{i,j}$ 为 `#` 或 `.`

#### 输入

输入通过标准输入按以下格式给出：

$$H \quad W$$
$$S_{1,1} S_{1,2} \dots S_{1,W}$$
$$S_{2,1} S_{2,2} \dots S_{2,W}$$
$$\vdots$$
$$S_{H,1} S_{H,2} \dots S_{H,W}$$

#### 输出

输出一个整数，表示最终被涂成黑色的格子总数。

---

#### 输入样例 1

```text
9 9
.........
.........
.........
.........
....#....
.........
.........
.........
.........
```

#### 输出样例 1

```text
57
```

#### 样例说明 1

网格状态会随着操作逐步变化。在重复进行 $10^{100}$ 次操作（本样例中状态会在数次操作后不再改变）后，最终被涂成黑色的格子共有 $57$ 个。

#### 输入样例 2

```text
2 2
..
..
```

#### 输出样例 2

```text
0
```

#### 输入样例 3

```text
10 10
..........
....#.....
#.......#.
......#...
.......#..
.....#....
..........
..........
..#...#...
.......#..
```

#### 输出样例 3

```text
64
```


# AC Code

``` python
import sys  
import math  
  
def main():  
  
    h, w = map(int, sys.stdin.readline().strip().split())  
  
    arr = []  
    for _ in range(h):  
        arr.append(list(sys.stdin.readline().strip()))  
  
    n = h * w  
    black = [False] * n  
    cnt = [0] * n  
    last_step = [-1] * n  
  
    curr_q = []  
    ans = 0  
  
    # 初始化  
    for r in range(h):  
        for c in range(w):  
            if arr[r][c] == '#':  
                idx = r * w  + c  
                black[idx] = True  
                curr_q.append(idx)  
                ans += 1  
  
    step = 0  
    while curr_q:  
        affected = []  
  
        for u in curr_q:  
            r, c = u // w, u % w  
  
            for dr, dc in [(-1, 0), (1, 0), (0, -1), (0, 1)]:  
                nr, nc = r + dr, c + dc  
  
                if 0 <= nr < h and 0 <= nc < w:  
                    v = nr * w + nc  
                    if not black[v]:  
                        cnt[v] += 1  
                        if last_step[v] != step:  
                            last_step[v] = step  
                            affected.append(v)  
  
        next_q = []  
        for v in affected:  
            if cnt[v] == 1:  
                black[v] = True  
                next_q.append(v)  
                ans += 1  
  
        curr_q = next_q  
        step += 1  
  
    print(ans)  
  
if __name__ == '__main__':  
    main()
```