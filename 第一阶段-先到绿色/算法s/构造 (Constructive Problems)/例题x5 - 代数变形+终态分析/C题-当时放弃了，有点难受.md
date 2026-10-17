https://atcoder.jp/contests/abc467/tasks/abc467_c

---

### C - Adjacent Sums (easy)

时间限制: 2 sec / 内存限制: 1024 MiB
分值: 300 分

#### 题目描述
※ C 题的题目与 E 题相同，仅红字标出的限制条件（制约）有所不同。

给你两个由 $0$ 到 $M-1$ 之间的整数组成的整数序列 $A = (A_1, A_2, \dots, A_N)$ 和 $B = (B_1, B_2, \dots, B_{N-1})$。  
$A$ 和 $B$ 的长度分别为 $N$ 和 $N-1$。  
你可以对 $A$ 进行任意次以下操作：

* 选择一个满足 $1 \le i \le N$ 的整数 $i$，将 $A_i$ 的值增加 $1$。

请计算使 $A$ 满足以下条件所需的最少操作次数。  
另外，在题目的限制条件下，可以证明一定存在能够满足条件的操作方案。

* 对于每个 $i = 1, 2, \dots, N-1$，$(A_i + A_{i+1})$ 除以 $M$ 的余数等于 $B_i$。

#### 数据范围
* $2 \le N \le 2 \times 10^5$
* <span style="color: red;">$M = 2$</span>
* $0 \le A_i \le M-1$
* $0 \le B_i \le M-1$
* 所有输入的值均为整数。

#### 输入格式
输入按以下格式从标准输入给出：

$$N \quad M$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$
$$B_1 \quad B_2 \quad \dots \quad B_{N-1}$$

#### 输出格式
输出一行，表示最少的操作次数。

---

#### 输入样例 1
```text
3 2
1 1 1
1 1
```

#### 输出样例 1
```text
1
```
在第 1 次操作中选择 $i=2$，则 $A$ 变为 $(1, 2, 1)$。  
此时 $A_1 + A_2 = 1 + 2 = 3$，$A_2 + A_3 = 2 + 1 = 3$，它们除以 $M=2$ 的余数均为 $1$，满足条件。  
因为初始状态 $A = (1, 1, 1)$ 不满足条件，所以最少操作次数为 $1$。

---

#### 输入样例 2
```text
2 2
1 1
0
```

#### 输出样例 2
```text
0
```

---

#### 输入样例 3
```text
10 2
0 0 0 1 1 0 1 0 1 0
0 1 0 1 0 1 0 1 0
```

#### 输出样例 3
```text
4
```



# Ac code
``` python
import sys  
  
def main():  
  
    n, m = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 前一共i确定了，那么后一个i+1也就确定了  
    # 所以只有两种状态  
  
    # 构造x1, 第一个数值为0  
    crr = [0]  
    for i in range(n-1):  
        ai = crr[i]  
        crr.append( (brr[i] - ai) % 2)  
  
    ans1 = 0  
    for num1, num2 in zip(crr, arr):  
        if num1 != num2:  
            ans1 += 1  
  
    # 构造x2， 第一个数值为1  
    crr = [1]  
    for i in range(n - 1):  
        ai = crr[i]  
        crr.append( (brr[i] - ai) % 2)  
      
    ans2 = 0  
    for num1, num2 in zip(crr, arr):  
        if num1 != num2:  
            ans2 += 1  
  
    print(min(ans1, ans2))  
  
  
if __name__ == "__main__":  
    main()
```