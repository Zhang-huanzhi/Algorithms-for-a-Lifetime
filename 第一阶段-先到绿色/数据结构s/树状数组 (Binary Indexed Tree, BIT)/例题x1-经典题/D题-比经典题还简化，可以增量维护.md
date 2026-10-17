https://atcoder.jp/contests/abc442/tasks/abc442_d

---

### D - 交换与区间和 (Swap and Range Sum)

时间限制: 2 秒 / 内存限制: 1024 MiB  
题目分值: 400 分

#### 题目描述
给你一个长度为 $N$ 的数列 $A=(A_1, A_2, \dots, A_N)$。

接下来给出 $Q$ 个操作，请按顺序进行处理。每个操作属于以下两种类型之一：

- `1 x` : 交换 $A_x$ 和 $A_{x+1}$ 的值。
- `2 l r` : 计算 $\sum_{i=l}^r A_i$ 的值（即区间 $[l, r]$ 的元素之和）。

#### 制约条件
- $2 \le N \le 2 \times 10^5$
- $1 \le Q \le 5 \times 10^5$
- $1 \le A_i \le 10^4$
- 对于第 1 类操作，满足 $1 \le x \le N-1$
- 对于第 2 类操作，满足 $1 \le l \le r \le N$
- 输入的所有数值均为整数

#### 输入格式
输入通过标准输入按以下格式给出：

$$
N \quad Q \\
A_1 \quad A_2 \quad \dots \quad A_N \\
\text{query}_1 \\
\text{query}_2 \\
\vdots \\
\text{query}_Q
$$

其中，$\text{query}_i$ 表示第 $i$ 个操作，格式为以下两种之一：

```text
1 x
```
或
```text
2 l r
```

#### 输出格式
设第 2 类操作的个数为 $q$，请输出 $q$ 行。  
第 $i$ 行 $(1 \le i \le q)$ 应包含第 $i$ 个第 2 类操作的计算结果。

---

### 样例 1

#### 输入样例 1
```text
4 4
2 7 1 8
1 2
2 1 2
1 1
2 2 4
```

#### 输出样例 1
```text
3
17
```

**样例解释 1**：
- 第 1 个操作：交换 $A_2$ 和 $A_3$ 的值，数列变为 $A=(2,1,7,8)$。
- 第 2 个操作：计算 $A_1 + A_2$，结果为 $2+1=3$。
- 第 3 个操作：交换 $A_1$ 和 $A_2$ 的值，数列变为 $A=(1,2,7,8)$。
- 第 4 个操作：计算 $A_2 + A_3 + A_4$，结果为 $2+7+8=17$。

---

### 样例 2

#### 输入样例 2
```text
8 10
22 75 26 45 72 81 47 29
2 2 7
2 6 8
2 4 4
1 2
2 1 3
1 1
2 2 4
1 2
1 4
2 1 1
```

#### 输出样例 2
```text
346
157
45
123
142
26
```

# 增量维护 Code
``` python
import math  
import sys  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    pre = [0] * (n + 1)  
    for i in range(1, n + 1):  
        pre[i] = pre[i - 1] + arr[i - 1]  
  
    outs = []  
    for _ in range(q):  
        line = sys.stdin.readline().strip().split()  
  
        op = line[0]  
  
        if op == '1':  
            x = int(line[1])  
  
            pre[x] = pre[x] - arr[x - 1] + arr[x]  
            arr[x], arr[x - 1] = arr[x - 1], arr[x]  
  
        if op == '2':  
            l = int(line[1])  
            r = int(line[2])  
  
            ans = pre[r] - pre[l - 1]  
            outs.append(ans)  
  
    print('\n'.join(map(str, outs)))  
if __name__ == "__main__":  
    main()
```