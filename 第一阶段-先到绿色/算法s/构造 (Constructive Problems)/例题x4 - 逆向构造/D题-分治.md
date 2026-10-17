https://atcoder.jp/contests/abc422/tasks/abc422_d

---

### D - Least Unbalanced
Time Limit: 2 sec / Memory Limit: 1024 MiB

分数：400 分

#### 问题描述
设 $N$ 为正整数。对于一个长度为 $2^N$ 的非负整数序列 $A = (A_1, A_2, \dots, A_{2^N})$，我们将其**不平衡度**定义为通过以下操作得到的非负整数值。

起初，令 $X = 0$。
接下来，重复执行以下一系列操作 $N$ 次：
1. 将 $X$ 更新为 $\max(X, \max(A) - \min(A))$。其中 $\max(A)$ 和 $\min(A)$ 分别代表当前序列 $A$ 的最大值和最小值。
2. 将序列 $A$ 按从前往后的顺序两两配对，求和后组成一个长度为原序列一半的新序列，并将其作为新的 $A$。也就是说，令 $A \leftarrow (A_1 + A_2, A_3 + A_4, \dots, A_{|A|-1} + A_{|A|})$。

最终的 $X$ 即为该序列的不平衡度。

##### 示例说明
例如，当 $N=2, A=(6,8,3,5)$ 时，其不平衡度的计算过程如下（最终结果为 $6$）：
1. 起初，$X = 0$。
2. **第 1 次操作：**
   * 将 $X$ 更新为 $\max(X, \max(A) - \min(A)) = \max(0, 8 - 3) = 5$。
   * 将 $A$ 更新为 $(6+8, 3+5) = (14, 8)$。
3. **第 2 次操作：**
   * 将 $X$ 更新为 $\max(X, \max(A) - \min(A)) = \max(5, 14 - 8) = 6$。
   * 将 $A$ 更新为 $(14+8) = (22)$。
4. 最终得到 $X = 6$。

现在给定一个非负整数 $K$。请构造一个长度为 $2^N$ 且所有元素之和为 $K$ 的非负整数序列，使得该序列的**不平衡度达到最小值**。

#### 数据范围
* $1 \le N \le 20$
* $0 \le K \le 10^9$
* $N, K$ 均为整数。

#### 输入格式
输入通过标准输入给出，格式如下：

$$N \quad K$$

#### 输出格式
设不平衡度达到最小值的序列为 $B=(B_1, B_2, \dots, B_{2^N})$，该序列的不平衡度为 $U$。
请按照以下格式输出答案：

$$U$$
$$B_1 \quad B_2 \quad \dots \quad B_{2^N}$$

如果存在多个满足条件的序列，输出其中任意一个即可。

#### 样例输入 1
```text
1 11
```

#### 样例输出 1
```text
1
5 6
```
序列 $(5, 6)$ 的不平衡度为 $1$，在所有总和为 $11$ 的序列中，这是不平衡度的最小值。

#### 样例输入 2
```text
3 56
```

#### 样例输出 2
```text
0
7 7 7 7 7 7 7 7
```

---

# Code
``` python
import sys  
import math  
  
  
def main():  
  
    n, k = map(int, sys.stdin.readline().strip().split())  
  
    def f(n, k):  
  
        if n == 0:  
            return [k]  
  
        left_sum = k // 2  
        right_sum = k - left_sum  
  
        left_seq = f(n - 1, left_sum)  
        right_seq = f(n - 1, right_sum)  
  
        return left_seq + right_seq  
  
    ans_seq = f(n, k)  
  
    ans = 0 if k % (1 << n) == 0 else 1  
  
    print(ans)  
    print(*ans_seq)  
  
  
if __name__ == '__main__':  
    main()
```