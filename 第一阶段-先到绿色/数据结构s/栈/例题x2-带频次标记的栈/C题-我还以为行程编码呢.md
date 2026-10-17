https://atcoder.jp/contests/abc438/tasks/abc438_c

---

### C - 1D puyopuyo

#### 题目描述
给你一个长度为 $N$ 的整数序列 $A = (A_1, A_2, \dots, A_N)$。

你可以按照任意顺序进行以下操作任意次（也可以是 $0$ 次）：
* 选择一个满足 $A_k = A_{k+1} = A_{k+2} = A_{k+3}$ 且满足 $1 \le k \le |A| - 3$ 的整数 $k$，并从 $A$ 中删除 $A_k, A_{k+1}, A_{k+2}, A_{k+3}$。（更严谨地说，将 $A$ 替换为 $(A_1, A_2, \dots, A_{k-1}, A_{k+4}, A_{k+5}, \dots, A_N)$。）
  这里，$|A|$ 表示整数序列 $A$ 的当前长度。

请计算在重复进行操作后，最终得到的 $|A|$ 的可能最小值。

#### 制约条件
* $1 \le N \le 2 \times 10^5$
* $1 \le A_i \le N$
* 输入的所有值均为整数。

#### 输入格式
输入按以下格式从标准输入给出：
> $N$
> $A_1$ $A_2$ $\dots$ $A_N$

#### 输出格式
输出在重复进行操作后，最终得到的 $|A|$ 的可能最小值。

---

#### 输入样例 1
```text
10
1 1 1 4 4 4 4 1 2 3
```
#### 输出样例 1
```text
2
```
**样例说明 1：**
通过以下 $2$ 次操作，可以使 $|A| = 2$：
1. 选择 $k = 4$。
   因为 $A_4 = A_5 = A_6 = A_7 = 4$ 成立，所以该选择是合法的。操作后序列变为 $A = (1, 1, 1, 1, 2, 3)$。
2. 选择 $k = 1$。
   因为 $A_1 = A_2 = A_3 = A_4 = 1$ 成立，所以该选择是合法的。操作后序列变为 $A = (2, 3)$。

由于无法使 $|A|$ 变得比 $2$ 更小，因此输出 `2`。

#### 输入样例 2
```text
3
2 1 3
```
#### 输出样例 2
```text
3
```
**样例说明 2：**
从一开始就无法进行任何操作。

#### 输入样例 3
```text
13
1 1 4 4 4 1 1 1 1 4 1 4 1
```
#### 输出样例 3
```text
5
```

# 栈 AC 
``` python
import sys  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 模拟stack  
    brr = []  
    cnt = 0  
    for num in arr:  
  
        brr.append(num)  
  
        if len(brr) >= 4 and brr[-1] == brr[-2] == brr[-3] == brr[-4]:  
            for i in range(4):  
                brr .pop()  
  
  
    print(len(brr))  
  
if __name__ == "__main__":  
    main()
```