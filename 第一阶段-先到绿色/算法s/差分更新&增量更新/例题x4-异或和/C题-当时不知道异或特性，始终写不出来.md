
https://atcoder.jp/contests/abc470/tasks/abc470_c

---


# C - Inc, Dec, Xor

时间限制：2 秒 / 内存限制：1024 MiB

分值：300 分

### 题目描述

有一个长度为 $N$ 的整数序列 $A = (A_1, A_2, \dots, A_N)$。初始时，$A$ 中的所有元素均为 $0$。

接下来有 $Q$ 个操作，请按顺序依次处理。操作共有 $2$ 种类型，格式如下：

* `1 x`：将 $A_x$ 的值增加 $1$。
* `2`：对于每个 $i = 1, 2, \dots, N$，若 $A_i \ge 1$，则将 $A_i$ 的值减少 $1$。

请在**每次**处理完一个操作后，求出当前 $A_1, A_2, \dots, A_N$ 的按位异或（XOR）和。

<details>
<summary>什么是按位异或（XOR）运算</summary>

非负整数 $A, B$ 的按位异或 $A \oplus B$ 定义如下：
* 将 $A, B$ 表示为二进制，若在 $2^k$ ($k \ge 0$) 位上 $A$ 和 $B$ 中恰有一个为 $1$，则 $A \oplus B$ 的二进制第 $2^k$ 位为 $1$，否则为 $0$。

例如，$3 \oplus 5 = 6$（二进制下为：$011 \oplus 101 = 110$）。
通常，$k$ 个非负整数 $p_1, p_2, p_3, \dots, p_k$ 的按位异或定义为 $(\dots((p_1 \oplus p_2) \oplus p_3) \oplus \dots \oplus p_k)$，且可以证明该计算结果与 $p_1, p_2, p_3, \dots, p_k$ 的顺序无关。
</details>

### 数据范围

* $1 \le N \le 5 \times 10^5$
* $1 \le Q \le 5 \times 10^5$
* $1 \le x \le N$
* 所有输入的值均为整数。

---

### 输入格式

输入从标准输入按以下格式给出：

> $N$ $Q$  
> $\text{query}_1$  
> $\text{query}_2$  
> $\vdots$  
> $\text{query}_Q$  

每个操作为以下两种格式之一：
> `1 x`  
> `2`  

### 输出格式

输出 $Q$ 行。

第 $i$ 行（$1 \le i \le Q$）输出处理完第 $i$ 个操作后，序列 $A$ 的所有元素 $A_1, A_2, \dots, A_N$ 的按位异或和。

---

### 样例输入 1

```text
2 5
1 2
1 2
1 1
2
2
```

### 样例输出 1

```text
1
2
3
1
0
```

**样例 1 解释：**
* 处理第 1 个操作后，$A = (0, 1)$。$0, 1$ 的按位异或和为 $1$，因此第 1 行输出 $1$。
* 处理第 2 个操作后，$A = (0, 2)$。$0, 2$ 的按位异或和为 $2$，因此第 2 行输出 $2$。
* 处理第 3 个操作后，$A = (1, 2)$。$1, 2$ 的按位异或和为 $3$，因此第 3 行输出 $3$。
* 处理第 4 个操作后，$A = (0, 1)$。$0, 1$ 的按位异或和为 $1$，因此第 4 行输出 $1$。
* 处理第 5 个操作后，$A = (0, 0)$。$0, 0$ 的按位异或和为 $0$，因此第 5 行输出 $0$。

---

### 样例输入 2

```text
3 8
1 2
1 3
1 1
1 2
1 1
2
1 3
1 1
```

### 样例输出 2

```text
1
0
1
2
1
0
1
2
```

# AC Code
``` python
import sys  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = [0] * (n + 1)  
    idxs = []  
    ans = 0  
    # print(bin(6))  
    for _ in range(q):  
  
        line = list(sys.stdin.readline().strip().split())  
  
        op = line[0]  
  
        if op == '1':  
            x = int(line[1])  
  
            if arr[x] == 0:  
                idxs.append(x)  
  
            ans ^= arr[x] ^ (arr[x] + 1)  
            arr[x] += 1  
  
        if op == '2':  
  
            for v in idxs:  
                ans ^= arr[v] ^ (arr[v] - 1)  
                arr[v] -= 1  
  
            idxs = [v for v in idxs if arr[v] != 0]  
  
        print(ans)  
              
if __name__ == "__main__":  
    main()
```