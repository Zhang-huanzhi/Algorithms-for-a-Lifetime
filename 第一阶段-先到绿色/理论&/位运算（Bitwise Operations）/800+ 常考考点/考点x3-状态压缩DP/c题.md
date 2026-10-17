https://atcoder.jp/contests/abc415/tasks/abc415_c

---

**C - Mixture (混合物)**
运行时间限制: 2 sec / 内存限制: 1024 MiB
分值 : 350 分

### 题目描述
有 $N$ 种药品 $1,2,\dots,N$。你的目标是将它们全部混合在一起。
给定一个由 $0$ 和 $1$ 组成、长度为 $2^N -1$ 的字符串 $S$。该字符串表示以下信息：

首先，将混合了 $1$ 种或更多种药品的状态 $i$ ($1 \le i \le 2^N -1$) 定义如下：
当且仅当将 $i$ 用二进制表示时，从低位（右侧）起第 $k$ ($1 \le k \le N$) 位为 $1$，表示该状态包含药品 $k$。
例如，$13$ 的二进制表示为 $1101_{(2)}$，因此状态 $13$ 表示混合了药品 $1,3,4$ 的状态。

- 当 $S$ 的第 $i$ 个字符为 `0` 时，状态 $i$ 是 **安全** 的。
- 当 $S$ 的第 $i$ 个字符为 `1` 时，状态 $i$ 是 **危险** 的。

你将使用以下操作来混合药品：
1. 首先，准备一个空瓶子。
2. 然后，重复以下步骤：
   - 选择 $1$ 种尚未倒入瓶中的药品，将其倒入瓶中。
   - 此时，瓶中混合的药品状态绝不能是危险的。

请判断是否可以通过此操作制造出所有药品都混合在一起的状态。

给定 $T$ 个测试用例，请分别求出答案。

### 约束条件
- $T$ 是 $1$ 到 $40000$ 之间的整数
- $N$ 是 $1$ 到 $18$ 之间的整数
- $S$ 是由 `0` 和 `1` 组成、长度为 $2^N -1$ 的字符串
- 在一个输入中，所有测试用例的 $|S|=2^N -1$ 的总和不超过 $5 \times 10^5$

### 输入格式
输入以以下格式从标准输入给出：
> $T$
> $\text{case}_1$
> $\text{case}_2$
> $\vdots$
> $\text{case}_T$

$\text{case}_i$ 表示第 $i$ 个测试用例。每个测试用例的格式如下：
> $N$
> $S$

### 输出格式
输出 $T$ 行。
第 $i$ 行输出第 $i$ 个测试用例的答案。
对于每个测试用例，如果能制造出所有药品都混合在一起的状态，输出 `Yes`，否则输出 `No`。

---

### 输入样例 1
```text
5
3
0010000
3
0010110
1
1
2
100
4
001110010101110
```

### 输出样例 1
```text
Yes
No
No
Yes
Yes
```

### 样例 1 解释
此输入包含 $5$ 个测试用例。

**第 1 个测试用例如下：**
- 存在 $3$ 种药品。
- 仅药品 $1,2$ 混合的状态 $3$ 是危险的，其余状态都是安全的。
例如，可以通过以下步骤制造出所有药品混合的状态：
1. 首先，将药品 $2$ 倒入瓶中。瓶中只有药品 $2$，这是状态 $2$，因此是安全的。
2. 接着，将药品 $3$ 倒入瓶中。瓶中有药品 $2,3$，这是状态 $6$，因此是安全的。
3. 最后，将药品 $1$ 倒入瓶中。瓶中有药品 $1,2,3$，这是状态 $7$，因此是安全的。

**第 2 个测试用例如下：**
- 存在 $3$ 种药品。
- 药品 $1,2$ 混合的状态 $3$、药品 $1,3$ 混合的状态 $5$、药品 $2,3$ 混合的状态 $6$ 是危险的，其余状态都是安全的。
在这种情况下，无法制造出所有药品都混合的状态。

**第 3 个测试用例如下：**
- 存在 $1$ 种药品。
- 仅有药品 $1$ 混合的状态 $1$ 就是危险的，因此无法制造出所有药品混合的状态。

# AC Code
``` python
import sys  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
  
    out = []  
    for _ in range(t):  
        n = int(sys.stdin.readline().strip())  
        s = sys.stdin.readline().strip()  
  
        limit = 1 << n  
        dp = [False] * limit  
        dp[0] = True  
  
        for mask in range(1, limit):  
            if s[mask - 1] == '1':  
                continue  
  
            temp = mask  
            can_reach = False  
  
            for i in range(n):  
                # 检查 mask 里面有没有第 i 种药（看第 i 位是不是 1）  
                if (mask >> i) & 1:  
                    # 如果有，把第 i 种药拿走，得到 pre_mask                    pre_mask = mask ^ (1 << i)  
                    if dp[pre_mask]:  
                        can_reach = True  
                        break  
            dp[mask] = can_reach  
  
        if dp[limit - 1]:  
            out.append("Yes")  
        else:  
            out.append("No")  
  
    print('\n'.join(out))  
  
if __name__ == "__main__":  
    main()
```


再优化
 ``` python
 import sys  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
      
    out = []  
    for _ in range(t):  
        n = int(sys.stdin.readline().strip())  
        s = sys.stdin.readline().strip()  
  
        limit = 1 << n  
        dp = [False] * limit  
        dp[0] = True  
  
        for mask in range(1, limit):  
            if s[mask - 1] == '1':  
                continue  
  
            temp = mask  
            can_reach = False  
  
            while temp > 0:  
                lowbit = temp & -temp  
                pre_mask = mask ^ lowbit  
  
                if dp[pre_mask]:  
                    can_reach = True  
                    break                temp ^= lowbit  
  
            dp[mask] = can_reach  
              
        if dp[limit - 1]:  
            out.append("Yes")  
        else:  
            out.append("No")  
  
    print('\n'.join(out))  
  
if __name__ == "__main__":  
    main()
 ```