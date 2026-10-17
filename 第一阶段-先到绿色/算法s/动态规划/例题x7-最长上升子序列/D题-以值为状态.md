https://atcoder.jp/contests/abc446/tasks/abc446_d

---

### D - Max Straight

**运行时间限制：** 2 秒 / **内存限制：** 1024 MiB  
**分值：** 400 分

#### 题目描述
给你一个长度为 $N$ 的整数序列 $A=(A_1, A_2, \dots, A_N)$。

请找出整数序列 $A$ 的一个满足以下条件的**子序列** $B=(B_1, B_2, \dots, B_{|B|})$，并求出其长度的最大值：

* 对于所有满足 $1 \le i \le |B|-1$ 的整数 $i$，均满足 $B_i + 1 = B_{i+1}$。

#### 什么是子序列？
序列 $A$ 的子序列是指从 $A$ 中删除 $0$ 个或多个元素，并将剩余元素按照原来的相对顺序排列而得到的序列。

#### 制约条件
* $1 \le N \le 2 \times 10^5$
* $1 \le A_i \le 10^9$
* 输入的所有值均为整数。

---

#### 输入格式
输入通过标准输入按以下格式给出：

$$N$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$

#### 输出格式
输出满足条件的最长子序列的长度。

---

### 输入输出样例

#### 样例 1
**输入：**
```text
7
3 4 3 5 7 6 2
```
**输出：**
```text
4
```
**说明：**  
$B=(3, 4, 5, 6)$ 是 $A$ 的一个满足条件的子序列，其长度为 $4$。  
不存在更长的满足条件的子序列，因此输出 $4$。

#### 样例 2
**输入：**
```text
5
5 4 3 2 1
```
**输出：**
```text
1
```

#### 样例 3
**输入：**
```text
10
1 2 3 4 5 6 7 8 9 10
```
**输出：**
```text
10
```


# Code
``` python
import sys  
from collections import  defaultdict  
  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    memo = defaultdict(int)  
  
    for num in arr:  
        memo[num] = max(memo[num], memo[num - 1] + 1)  
  
    print(max(memo.values()))  
  
if __name__ == "__main__":  
    main()
```