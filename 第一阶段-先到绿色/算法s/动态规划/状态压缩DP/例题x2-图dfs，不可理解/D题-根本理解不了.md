### D - 2x2 Erasing 2  
**时间限制：** 2 秒 / **内存限制：** 1024 MiB  
**分值：** 425 分

#### 题目描述
有一个 $H$ 行 $W$ 列的网格，每个格子被涂成白色或黑色。
我们用 $(i, j)$ 表示从上到下第 $i$ 行（$1 \le i \le H$）、从左到右第 $j$ 列（$1 \le j \le W$）的格子。

网格的初始状态由 $H$ 个长度为 $W$ 且仅包含 `.` 和 `#` 的字符串 $S_1, S_2, \dots, S_H$ 给出。
若 $S_i$ 的第 $j$ 个字符为 `.`，表示格子 $(i, j)$ 为白色；若为 `#`，则表示格子 $(i, j)$ 为黑色。

高桥君希望通过将若干个（可以为 $0$ 个）黑色格子重新涂成白色，使得网格中不存在完全由黑色格子组成的 $2 \times 2$ 区域。
更严谨地说，他希望满足以下条件：
对于所有满足 $1 \le i \le H-1$ 且 $1 \le j \le W-1$ 的整数对 $(i, j)$，在格子 $(i, j)$、$(i, j+1)$、$(i+1, j)$、$(i+1, j+1)$ 中，**至少有一个**格子是白色的。

求高桥君为了达成目标，最少需要将多少个黑色格子重新涂成白色。

本题包含 $T$ 组测试数据，请对每组测试数据分别求出答案。

#### 数据范围
* $1 \le T \le 100$
* $2 \le H \le 7$
* $2 \le W \le 7$
* $S_i$ 是长度为 $W$ 且仅包含 `.` 和 `#` 的字符串
* $T, H, W$ 均为整数

#### 输入格式
输入从标准输入按以下格式给出：

$$T$$
$$\text{case}_1$$
$$\text{case}_2$$
$$\vdots$$
$$\text{case}_T$$

其中 $\text{case}_i$ 表示第 $i$ 个测试数据。每个测试数据的具体格式如下：

$$H \quad W$$
$$S_1$$
$$S_2$$
$$\vdots$$
$$S_H$$

#### 输出格式
输出共 $T$ 行。
第 $i$ 行（$1 \le i \le T$）输出第 $i$ 个测试数据的答案。

---

#### 样例 1 输入
```text
2
5 5
####.
##.##
#####
.####
##.#.
2 2
..
..
```

#### 样例 1 输出
```text
3
0
```

#### 样例解释
对于第 1 个测试数据，网格的初始状态如原题面左图所示。
例如，通过将右图中标有数字的 $3$ 个黑色格子重新涂成白色，可以使网格满足条件。
由于无法通过将 $2$ 个或更少的黑色格子涂白来达成目标，因此最少需要涂白 $3$ 个格子，第一行输出 $3$。

对于第 2 个测试数据，网格初始状态就已经满足条件，因此不需要涂白任何格子，第二行输出 $0$。


---

# AC Code
``` python
import sys  
  
  
def main():  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
        h, w = map(int, sys.stdin.readline().strip().split())  
        arr = []  
        for i in range(h):  
            arr.append(list(sys.stdin.readline().strip()))  
  
        row_masks = []  
        total_black = 0  
  
        # 转成 bitmask        for i in range(h):  
            mask = 0  
            for j in range(w):  
                if arr[i][j] == '#':  
                    mask |= (1 << j)  
                    total_black += 1  
            row_masks.append(mask)  
  
        all_masks = 1 << w  
  
        # 预处理每行所有可选状态（必须是原mask子集）  
        valid_masks = []  
        for i in range(h):  
            cur = []  
            origin = row_masks[i]  
            for mask in range(all_masks):  
                if (mask & ~origin) == 0:  
                    cur.append(mask)  
            valid_masks.append(cur)  
  
        NEG = -10**9  
        dp = {mask: mask.bit_count() for mask in valid_masks[0]}  
  
        for i in range(1, h):  
            ndp = {}  
  
            for mask in valid_masks[i]:  
                cnt = mask.bit_count()  
                best = NEG  
  
                for pmask, val in dp.items():  
                    common = pmask & mask  
  
                    # 检查是否出现 2x2 全黑  
                    if (common & (common << 1)) != 0:  
                        continue  
  
                    best = max(best, val + cnt)  
  
                if best != NEG:  
                    ndp[mask] = best  
  
            dp = ndp  
  
        max_keep = max(dp.values())  
        ans = total_black - max_keep  
        print(ans)  
  
  
if __name__ == '__main__':  
    main()
```