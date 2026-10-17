https://atcoder.jp/contests/abc425/tasks/abc425_c

---

### 题目名称：C - Rotate and Sum Query
时间限制: 2 秒 / 内存限制: 1024 MiB
分值：350 分

#### 题目描述
给定一个长度为 $N$ 的整数序列 $A=(A_1, A_2, \dots, A_N)$。

请依次处理 $Q$ 个查询。查询共有以下两种类型：

* `1 c`：将序列 $A$ 的首个元素移动到末尾，重复该操作 $c$ 次。（即循环左移 $c$ 次）
* `2 l r`：计算并输出 $\sum_{i=l}^{r} A_i$ 的值。

#### 数据范围
* $1 \le N \le 2 \times 10^5$
* $1 \le Q \le 2 \times 10^5$
* $1 \le A_i \le 10^9$
* $1 \le c \le N$
* $1 \le l \le r \le N$
* 保证至少存在一个第二种类型的查询。
* 所有输入的值均为整数。

#### 输入格式
输入按以下格式从标准输入给出：
> $N \quad Q$  
> $A_1 \quad A_2 \quad \dots \quad A_N$  
> $\text{query}_1$  
> $\text{query}_2$  
> $\vdots$  
> $\text{query}_Q$  

每个查询 $\text{query}_i$ 采用以下两种格式之一：
> `1 c`  

> `2 l r`  

#### 输出格式
对于每个第二种类型的查询，输出其计算结果，每个结果占一行。

---

#### 输入样例 1
```text
4 3
3 1 4 5
2 1 3
1 1
2 2 3
```

#### 输出样例 1
```text
8
9
```

**样例说明 1**  
各个查询的处理过程如下：
* 第 1 个查询：由于 $A_1 + A_2 + A_3 = 3 + 1 + 4 = 8$，因此输出 `8`。
* 第 2 个查询：执行 $1$ 次循环左移操作，序列由 $A=(3, 1, 4, 5)$ 变为 $A=(1, 4, 5, 3)$。
* 第 3 个查询：此时 $A_2 + A_3 = 4 + 5 = 9$，因此输出 `9`。

#### 输入样例 2
```text
5 7
1 2 4 8 16
2 1 5
1 4
1 5
2 1 5
2 2 4
1 1
2 3 3
```

#### 输出样例 2
```text
31
31
7
4
```


# Code 感觉还可以再优化下
``` python
import sys  
import math  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 倍增数组  
    arr = arr + arr  
    offset = 0  
  
    # 前缀和  
    pre = [0] * (2 * n + 1)  
    pre[0] = arr[0]  
    for i in range(1, 2 * n):  
        pre[i] = pre[i-1] + arr[i]  
  
    # print(pre)  
    for _ in range(q):  
        line = list(sys.stdin.readline().strip().split())  
  
        if line[0] == '1':  
            c = int(line[1])  
            offset += c  
            if offset > n:  
                offset -= n  
  
        if line[0] == '2':  
            l = int(line[1])  
            r = int(line[2])  
            # print(l, r)  
            print(pre[r - 1 + offset] - pre[l - 2 + offset])  
  
  
if __name__ == '__main__':  
    main()
```