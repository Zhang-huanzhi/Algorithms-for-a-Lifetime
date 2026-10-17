https://atcoder.jp/contests/abc452/tasks/abc452_c

---

### C - 鱼骨 (Fishbones)

运行时间限制: 2 秒 / 内存限制: 1024 MiB

分值：300 分

#### 题目描述

艺术家高砂君制作了一个鱼骨形状的艺术品。

该艺术品由 $N$ 根肋骨和 $1$ 根脊椎组成。肋骨的编号为 $1$ 到 $N$。

高砂君打算在这 $N+1$ 根骨头上各写一个字符串，使得满足以下所有条件：

* 写在脊椎上的字符串长度为 $N$。
* 对于每根肋骨 $i = 1, \dots, N$，以下条件成立：
  * 写在肋骨 $i$ 上的字符串长度为 $A_i$。
  * 写在肋骨 $i$ 上的字符串的第 $B_i$ 个字符，与写在脊椎上的字符串的第 $i$ 个字符相同。
* 写在这 $N+1$ 根骨头上的所有字符串，都必须是候选字符串 $S_1, \dots, S_M$ 中的某一个（可以重复使用）。
* $S_1, \dots, S_M$ 是由小写英文字母组成的字符串，且互不相同。

对于每个 $j = 1, \dots, M$，请回答以下问题：

* 是否存在一种满足条件的填写方式，使得写在脊椎上的字符串为 $S_j$？

#### 制约条件

* $N$ 是整数
* $1 \le N \le 10$
* $A_i, B_i$ 是整数 ($1 \le i \le N$)
* $1 \le B_i \le A_i \le 10$ ($1 \le i \le N$)
* $M$ 是整数
* $1 \le M \le 200000$
* $S_j$ 是由小写英文字母组成的字符串 ($1 \le j \le M$)
* $1 \le |S_j| \le 10$ ($1 \le j \le M$)
* $S_1, \dots, S_M$ 互不相同

#### 输入

输入按以下格式从标准输入给出：

$$N$$
$$A_1 \quad B_1$$
$$\vdots$$
$$A_N \quad B_N$$
$$M$$
$$S_1$$
$$\vdots$$
$$S_M$$

#### 输出

输出共 $M$ 行。

第 $j$ 行（$1 \le j \le M$），若存在一种满足条件的填写方式使得脊椎上的字符串为 $S_j$，则输出 `Yes`；否则输出 `No`。

---

#### 输入样例 1

```text
5
5 3
5 2
4 1
5 1
3 2
8
retro
chris
itchy
tuna
crab
rock
cod
ash
```

#### 输出样例 1

```text
Yes
Yes
No
No
No
No
No
No
```

**样例解释 1：**

当在肋骨 1、2、3、4、5 上分别写上 `chris`、`retro`、`tuna`、`retro`、`cod` 时，如果脊椎上写的字符串是 `retro`（即 $S_1$），则可以满足所有条件：
* 脊椎上的字符串为 `retro`，长度为 $N=5$。
* 肋骨 1 写的 `chris` 长度为 $A_1=5$，其第 $B_1=3$ 个字符为 'r'，与脊椎的第 1 个字符 'r' 相同。
* 肋骨 2 写的 `retro` 长度为 $A_2=5$，其第 $B_2=2$ 个字符为 'e'，与脊椎的第 2 个字符 'e' 相同。
* 肋骨 3 写的 `tuna` 长度为 $A_3=4$，其第 $B_3=1$ 个字符为 't'，与脊椎的第 3 个字符 't' 相同。
* 肋骨 4 写的 `retro` 长度为 $A_4=5$，其第 $B_4=1$ 个字符为 'r'，与脊椎的第 4 个字符 'r' 相同。
* 肋骨 5 写的 `cod` 长度为 $A_5=3$，其第 $B_5=2$ 个字符为 'o'，与脊椎的第 5 个字符 'o' 相同。

因此对于 $S_1$ （`retro`）输出 `Yes`。

# Code AC 静态布尔数组
``` python 
import sys  
from collections import defaultdict  
from operator import index  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = []  
    for _ in range(n):  
        a, b = map(int, sys.stdin.readline().strip().split())  
        arr.append((a, b))  
  
    m = int(sys.stdin.readline().strip())  
  
    brr = []  
    # 三维布尔数组  
    memo = [ [ [False] * 27 for _ in range(11)] for _ in range(11)]  
    for _ in range(m):  
        s = list(sys.stdin.readline().strip())  
        brr.append(s)  
        s_len = len(s)  
  
        for i in range(s_len):  
            memo[s_len][i + 1][ord(s[i]) - ord('a')] = True  
  
  
    # 检验  
    out = []  
    for xrr in brr:  
  
        if len(xrr) != n:  
           out.append("No")  
           continue  
  
  
        for  idx, comb in enumerate(arr):  
            a, b = comb  
            target_char = ord(xrr[idx]) - ord('a')  
            if not memo[a][b][target_char]:  
                out.append("No")  
                break  
  
        else:  
            out.append("Yes")  
  
    print("\n".join(out))  
  
if __name__ == "__main__":  
    main()
```