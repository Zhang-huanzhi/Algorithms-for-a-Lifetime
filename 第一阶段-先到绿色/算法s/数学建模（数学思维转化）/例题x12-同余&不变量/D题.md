https://atcoder.jp/contests/abc450/tasks/abc450_d

---

### D - 极差最小化 (Minimize Range)

时间限制: 2 秒 / 内存限制: 1024 MiB  
分值: 400 分

#### 题目描述
给定一个长度为 $N$ 的正整数序列 $A$ 和一个正整数 $K$。  
你可以对序列 $A$ 执行任意次（也可以是 0 次）以下操作：

* 选择一个整数 $i$（$1 \le i \le N$），将 $A_i$ 加上 $K$。

请求出 $\max(A) - \min(A)$ 可能达到的最小值。

#### 制约条件
* $1 \le N \le 2 \times 10^5$
* $1 \le K \le 10^9$
* $1 \le A_i \le 10^9$
* 输入的所有值均为整数

#### 输入格式
输入从标准输入中给出，格式如下：

$N$ $K$  
$A_1$ $A_2$ $\dots$ $A_N$  

#### 输出格式
输出一行，表示 $\max(A) - \min(A)$ 的最小值。

---

#### 输入样例 1
```
3 10
3 21 9
```

#### 输出样例 1
```
4
```
**样例 1 解释：**  
* 首先选择 $i = 1$，序列变为 $A = (13, 21, 9)$。  
* 接着选择 $i = 3$，序列变为 $A = (13, 21, 19)$。  
* 再次选择 $i = 1$，序列变为 $A = (23, 21, 19)$。  
此时，$\max(A) - \min(A) = 23 - 19 = 4$。  
无法使差值达到 $3$ 或更小，因此答案为 $4$。

---

#### 输入样例 2
```
5 6
4 100 5 10 450
```

#### 输出样例 2
```
2
```

# AC Code
``` python
import sys  
  
def main():  
  
    n, k = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    arr = [ arr[i] % k for i in range(n) ]  
  
    arr.sort()  
  
    ans = arr[-1] - arr[0]  
  
    for i in range(n - 1):  
        diff = (arr[i] + k) - arr[i + 1]  
        if diff < ans:  
            ans = diff  
  
    print(ans)  
  
  
if __name__ == "__main__":  
    main()
```