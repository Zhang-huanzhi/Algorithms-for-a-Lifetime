https://atcoder.jp/contests/abc464/tasks/abc464_e

---

### E - Fill-Rect Query

时间限制: 2 秒 / 内存限制: 1024 MiB
分值: 450 分

#### 题目描述
有一个 $H \times W$ 的网格。初始时，所有方格中都写着大写字母 `A`。我们用 $(i,j)$ 表示从上往数第 $i$ 行、从左往数第 $j$ 列的方格。

接下来将按顺序进行 $Q$ 次操作。在第 $i$ 次操作中，将以左上角为 $(1,1)$、右下角为 $(R_i, C_i)$ 的矩形区域内的所有方格，全部覆写为大写英文字母 $X_i$。

请输出进行完所有操作后的网格状态。

#### 数据范围
* $1 \le H, W$
* $H \times W \le 10^6$
* $1 \le Q \le 2 \times 10^5$
* $1 \le R_i \le H$
* $1 \le C_i \le W$
* $X_i$ 是大写英文字母。
* 输入的所有数值均为整数。

#### 输入格式
输入按以下格式从标准输入给出：

$$
H \quad W \quad Q \\
R_1 \quad C_1 \quad X_1 \\
R_2 \quad C_2 \quad X_2 \\
\vdots \\
R_Q \quad C_Q \quad X_Q
$$

#### 输出格式
输出 $H$ 行。
第 $i$ 行应输出一个长度为 $W$ 的字符串，其中第 $j$ 个字符表示最终网格中方格 $(i,j)$ 上的大写英文字母。

---

#### 输入样例 1
```text
2 3 3
2 2 B
1 3 C
2 1 D
```

#### 输出样例 1
```text
DCC
DBA
```
操作过程如下图（原题面图示）所示：
* 初始状态：所有位置为 `A`。
* 第 1 次操作：将左上角 $(1,1)$ 到右下角 $(2,2)$ 的区域覆写为 `B`。
* 第 2 次操作：将左上角 $(1,1)$ 到右下角 $(1,3)$ 的区域覆写为 `C`。
* 第 3 次操作：将左上角 $(1,1)$ 到右下角 $(2,1)$ 的区域覆写为 `D`。

#### 输入样例 2
```text
1 7 7
1 7 E
1 6 C
1 5 N
1 4 A
1 3 V
1 2 D
1 1 A
```

#### 输出样例 2
```text
ADVANCE
```

#### 输入样例 3
```text
10 10 15
8 9 B
6 7 C
5 8 D
10 6 E
8 5 F
3 10 G
7 3 H
4 6 I
3 1 J
10 2 K
3 6 L
3 3 M
2 5 N
9 1 O
1 4 P
```

#### 输出样例 3
```text
PPPPNLGGGG
ONNNNLGGGG
OMMLLLGGGG
OKIIIIDDBA
OKHFFEDDBA
OKHFFECBBA
OKHFFEBBBA
OKFFFEBBBA
OKEEEEAAAA
KKEEEEAAAA
```

# AC Code
``` python
import sys  
  
def main():  
  
    h, w, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = [ [0] * w for _ in range(h)]  
    brr = {0: 'A'}  
  
    for i in range(1, q + 1):  
  
        r, c, x = map(str, sys.stdin.readline().strip().split())  
        r, c = int(r), int(c)  
        brr[i] = x  
  
        arr[r - 1][c - 1] = i  
  
    ## DP 扩散 逻辑  
    for r in range(h - 1, -1, -1):  
        row = arr[r]  
        if r < h - 1:  
            next_row = arr[r + 1]  
            # 1. 最后一列的边界处理（只受下方 next_row 影响）  
            if next_row[w - 1] > row[w - 1]:  
                row[w - 1] = next_row[w - 1]  
  
            # 2. 中间格子的处理（同时受下方 next_row 和右方 row[c+1] 影响）  
            for c in range(w - 2, -1, -1):  
                val = row[c]  
                n_val = next_row[c]  # 下方格子的操作编号  
                r_val = row[c + 1]  # 右方格子的操作编号  
                if n_val > val:  
                    val = n_val  
                if r_val > val:  
                    val = r_val  
                row[c] = val  
        else:  
            # 3. 最后一行的边界处理（只受右方 row[c+1] 影响）  
            for c in range(w - 2, -1, -1):  
                if row[c + 1] > row[c]:  
                    row[c] = row[c + 1]  
  
    out = []  
    for r in range(h):  
        row = "".join(brr[arr[r][c]] for c in range(w))  
        out.append(row)  
  
    print('\n'.join(out))  
  
if __name__ == "__main__":  
    main()
```
``