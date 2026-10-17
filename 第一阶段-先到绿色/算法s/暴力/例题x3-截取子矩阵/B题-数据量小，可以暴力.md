https://atcoder.jp/contests/abc430/tasks/abc430_b

---

### B - Count Subgrid  
时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 250 分  

#### 问题描述
有一个由 $N$ 行 $N$ 列组成的网格。网格中从上数第 $i$ 行、从左数第 $j$ 列的格子，若 $S_{i, j}$ 为 `#` 则被涂成黑色，为 `.` 则被涂成白色。

如果从这个网格中任意截取一个纵向 $M$ 行、横向 $M$ 列的子网格区域，那么一共可以得到多少种**不同**的格子涂色方案？

#### 限制
* $1 \le M \le N \le 10$
* $N, M$ 均为整数。
* $S_{i, j}$ 为 `.` 或 `#`。

#### 输入
输入按以下格式从标准输入给出：

$$N \quad M$$
$$S_{1,1}S_{1,2}\dots S_{1,N}$$
$$S_{2,1}S_{2,2}\dots S_{2,N}$$
$$\vdots$$
$$S_{N,1}S_{N,2}\dots S_{N,N}$$

#### 输出
输出一个整数，表示不同涂色方案的数量。

---

#### 样例输入 1
```text
3 2
...
###
#.#
```

#### 样例输出 1
```text
3
```
**样例 1 说明：**  
给定的网格状态如原题图左所示。  
从中截取纵向 2 行、横向 2 列的子网格，共有 4 种截取位置。这 4 种截取方式得到的涂色图形如下：
1. 第一行和第二行的前两列：
   ```text
   ..
   ##
   ```
2. 第一行和第二行的后两列：
   ```text
   ..
   ##
   ```
3. 第二行和第三行的前两列：
   ```text
   ##
   #.
   ```
4. 第二行和第三行的后两列：
   ```text
   ##
   .#
   ```

这 4 个子网格中，前两个方案的图案是完全相同的。因此，一共有 **3** 种不同的涂色方案。

#### 样例输入 2
```text
10 3
..#.......
.###......
.#.#......
#####.....
#...#.....
......####
......#..#
......#...
......#..#
......####
```

#### 样例输出 2
```text
36
```


# Code
``` python
import math  
import sys  
  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    grid = []  
    for _ in range(n):  
        grid.append(list(sys.stdin.readline().strip()))  
  
    se = set()  
  
    for r in range(n - m + 1):  
        for c in range(n - m  + 1):  
            subgrid = tuple(tuple(grid[i][c : c + m]) for i in range(r, r + m))  
            se.add(subgrid)  
  
    print(len(se))  
  
if __name__ == "__main__":  
    main()
```