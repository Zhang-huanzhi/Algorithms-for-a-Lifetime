https://atcoder.jp/contests/abc438/tasks/abc438_d

---

# D - Tail of Snake

时间限制: 2 秒 / 内存限制: 1024 MiB

## 分值
400 分

## 题目描述
Snuke 正在观察一条蛇，他想知道从哪里到哪里分别是蛇的头部、身体和尾部。
Snuke 将蛇分成了 $N$ 个部分，并对每个部分评估了其“像头部”、“像身体”和“像尾部”的程度。接着，他决定寻找一种划分方式，使得这些评估值的总和最大。

给定三个长度为 $N$ 的整数序列 $A=(A_1, A_2, \ldots, A_N)$、$B=(B_1, B_2, \ldots, B_N)$ 和 $C=(C_1, C_2, \ldots, C_N)$。

对于满足 $1 \leq x < y < N$ 的整数对 $(x,y)$，求以下表达式的最大可能值：
$$ \sum_{i=1}^{x} A_i + \sum_{i=x+1}^{y} B_i + \sum_{i=y+1}^{N} C_i $$

## 数据范围
* $3 \leq N \leq 3 \times 10^5$
* $1 \leq A_i, B_i, C_i \leq 10^6$
* 输入的所有值均为整数。

## 输入格式
输入从标准输入给出，格式如下：

$$N$$
$$A_1 \ A_2 \ \ldots \ A_N$$
$$B_1 \ B_2 \ \ldots \ B_N$$
$$C_1 \ C_2 \ \ldots \ C_N$$

## 输出格式
输出答案。

---

## 输入样例 1
```text
5
1 4 2 4 3
2 3 4 2 2
3 2 4 4 3
```

## 输出样例 1
```text
16
```
**样例说明：**
若选择 $(x,y)=(2,3)$，则总和为：
$$ \sum_{i=1}^{2} A_i + \sum_{i=3}^{3} B_i + \sum_{i=4}^{5} C_i = (1+4) + 4 + (4+3) = 16 $$
这是可以达到的最大值。

## 输入样例 2
```text
3
1 1 1
1 1 1
1 1 1
```

## 输出样例 2
```text
3
```

## 输入样例 3
```text
6
2 10 7 7 7 11
5 7 9 10 9 12
6 6 7 10 12 7
```

## 输出样例 3
```text
50
```

# 动态规划AC Code
``` python
import math  
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
    crr = list(map(int, sys.stdin.readline().strip().split()))  
  
    dp = [ [-math.inf] * 3 for _ in range(n + 1)]  
  
    # 第一个元素必须是a  
    dp[1][0] = arr[0]  
    for i in range(1, n):  
        a = arr[i]  
        b = brr[i]  
        c = crr[i]  
        idx = i + 1  
  
        # 保持拿a  
        dp[idx][0] = a + dp[idx - 1][0]  
  
        # 拿b，刚开始拿b  
        b2 = b + dp[idx - 1][0]  
        # 拿b，持续拿b  
        b1 = b + dp[idx - 1][1]  
        dp[idx][1] = max(b1, b2)  
  
        # 拿c， 刚开始拿c  
        c1 = c + dp[idx - 1][1]  
        # 拿c， 持续拿c  
        c2 = c + dp[idx - 1][2]  
        dp[idx][2] = max(c1, c2)  
  
    print(dp[n][2])  
  
if __name__ == "__main__":  
    main()
```