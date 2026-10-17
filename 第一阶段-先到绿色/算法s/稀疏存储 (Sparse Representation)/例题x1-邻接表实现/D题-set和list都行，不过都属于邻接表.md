https://atcoder.jp/contests/abc406/tasks/abc406_d

---

# D - Garbage Removal / 垃圾清理

时间限制: 2.5 秒 / 内存限制: 1024 MiB

分值：400 分

### 题目描述

存在一个 $H$ 行 $W$ 列的网格。自上而下第 $i$ 行、自左而右第 $j$ 列的格子记为格子 $(i, j)$。

网格上散落着 $N$ 个垃圾，第 $i$ 个垃圾位于格子 $(X_i, Y_i)$。

现在给定 $Q$ 个查询，请按顺序处理这些查询并求出每个查询的答案。每个查询为以下两种类型之一：

- **类型 1**：输入格式为 `1 x`。求出当前网格中第 $x$ 行存在的垃圾数量作为答案。随后，将第 $x$ 行的所有垃圾全部清除（从网格中移除）。
- **类型 2**：输入格式为 `2 y`。求出当前网格中第 $y$ 列存在的垃圾数量作为答案。随后，将第 $y$ 列的所有垃圾全部清除（从网格中移除）。

### 限制条件

- $1 \le H, W, N \le 2 \times 10^5$
- $1 \le X_i \le H$
- $1 \le Y_i \le W$
- 当 $i \neq j$ 时，$(X_i, Y_i) \neq (X_j, Y_j)$
- $1 \le Q \le 2 \times 10^5$
- 对于类型 1 的查询，$1 \le x \le H$
- 对于类型 2 的查询，$1 \le y \le W$
- 所有输入值均为整数。

### 输入格式

输入通过标准输入给出，格式如下：

$$\begin{array}{l}
H \quad W \quad N \\
X_1 \quad Y_1 \\
X_2 \quad Y_2 \\
\vdots \\
X_N \quad Y_N \\
Q \\
\text{query}_1 \\
\text{query}_2 \\
\vdots \\
\text{query}_Q 
\end{array}$$

其中 $\text{query}_i$ 表示第 $i$ 个查询，格式为以下之一：

- `1 x`
- `2 y`

### 输出格式

输出 $Q$ 行。
第 $i$ 行输出第 $i$ 个查询的答案。

---

### 样例输入 1

```text
3 4 5
1 2
1 3
3 4
3 1
2 2
5
1 1
1 2
2 2
2 4
1 2
```

### 样例输出 1

```text
2
1
0
1
0
```

**样例 1 解释：**
- 起初，垃圾位于格子 $(1,2), (1,3), (3,4), (3,1), (2,2)$。
- **第 1 个查询**：第 1 行的垃圾有位于 $(1,2)$ 和 $(1,3)$ 的 $2$ 个，因此答案为 $2$。这两个垃圾被清除，剩余垃圾位于 $(3,4), (3,1), (2,2)$。
- **第 2 个查询**：第 2 行的垃圾有位于 $(2,2)$ 的 $1$ 个，因此答案为 $1$。该垃圾被清除，剩余垃圾位于 $(3,4), (3,1)$。
- **第 3 个查询**：第 2 列没有垃圾，因此答案为 $0$。
- **第 4 个查询**：第 4 列的垃圾有位于 $(3,4)$ 的 $1$ 个，因此答案为 $1$。该垃圾被清除，剩余垃圾位于 $(3,1)$。
- **第 5 个查询**：第 2 行没有垃圾，因此答案为 $0$。

---

### 样例输入 2

```text
1 2 1
1 2
7
2 1
2 1
2 1
2 1
2 1
2 1
2 1
```

### 样例输出 2

```text
0
0
0
0
0
0
0
```

---

### 样例输入 3

```text
4 4 16
1 1
1 2
1 3
1 4
2 1
2 2
2 3
2 4
3 1
3 2
3 3
3 4
4 1
4 2
4 3
4 4
7
2 1
1 1
2 2
1 2
2 3
1 3
2 4
```

### 样例输出 3

```text
4
3
3
2
2
1
1
```


# Code 邻接表解法

``` python
import sys  
  
  
def main():  
      
    h, w, n = map(int, sys.stdin.readline().strip().split())  
  
    # 不用二维数组，用邻接表  
    row = [ [] for _ in range(h)]  
    col = [ [] for _ in range(w)]  
  
    # 是否被查询过  
    xrr = [True] * h  
    yrr = [True] * w  
  
  
    for _ in range(n):  
        # x是行，y是列  
        x, y = map(int, sys.stdin.readline().strip().split())  
        x -= 1  
        y -= 1  
        row[x].append(y)  
        col[y].append(x)  
  
    q = int(sys.stdin.readline().strip())  
    out = []  
    for _ in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        op = int(line[0])  
        cnt = 0  
        if op == 1:  
            x = int(line[1])  
            x -= 1  
  
            if not xrr[x]:  
                out.append(0)  
            else:  
                for num in row[x]:  
                    if yrr[num]:  
                        cnt += 1  
  
                xrr[x] = False  
                out.append(cnt)  
  
        if op == 2:  
            y = int(line[1])  
            y -= 1  
  
            if not yrr[y]:  
                out.append(0)  
  
            else:  
                for num in col[y]:  
                    if xrr[num]:  
                        cnt += 1  
  
                yrr[y] = False  
                out.append(cnt)  
  
    print('\n'.join(map(str, out)))  
  
  
  
if __name__ == "__main__":  
    main()
```