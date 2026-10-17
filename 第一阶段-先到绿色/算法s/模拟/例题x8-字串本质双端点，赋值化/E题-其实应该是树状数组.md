https://atcoder.jp/contests/abc441/tasks/abc441_e

---

### E - A > B substring

**时间限制**：2 秒 / **内存限制**：1024 MiB

**分值**：450 分

#### 题目描述

给定一个长度为 $N$、仅由 `A`、`B`、`C` 三种字符组成的字符串 $S$。

在 $S$ 的所有 $\frac{N(N+1)}{2}$ 个非空连续子串中，请计算有多少个子串满足：**包含的字符 `A` 的数量严格大于字符 `B` 的数量**。

请注意：如果两个子串在 $S$ 中的起始或结束位置不同，即使它们内容相同，也视为不同的子串分别计数。

**关于子串**：
字符串 $S$ 的子串是指从 $S$ 的开头删除 0 个或多个字符、从末尾删除 0 个或多个字符后得到的连续字符序列。
例如，`AB` 是 `ABC` 的子串，但 `AC` 不是 `ABC` 的子串。

#### 制约条件

* $1 \le N \le 5 \times 10^5$
* $S$ 是由 `A`, `B`, `C` 组成的长度为 $N$ 的字符串。
* $N$ 是整数。

---

#### 输入格式

输入通过标准输入按以下格式给出：

```text
N
S
```

#### 输出格式

输出 $S$ 的所有非空连续子串中，包含的 `A` 的数量严格大于 `B` 的数量的子串个数。

---

#### 样例 1

**输入**
```text
10
ACBBCABCAB
```

**输出**
```text
8
```

**说明**
以下 $8$ 个子串满足条件（下标从 1 开始）：
1. `A`：从第 $1$ 个字符到第 $1$ 个字符
2. `AC`：从第 $1$ 个字符到第 $2$ 个字符
3. `CA`：从第 $5$ 个字符到第 $6$ 个字符
4. `CABCA`：从第 $5$ 个字符到第 $9$ 个字符
5. `A`：从第 $6$ 个字符到第 $6$ 个字符
6. `ABCA`：从第 $6$ 个字符到第 $9$ 个字符
7. `CA`：从第 $8$ 个字符到第 $9$ 个字符
8. `A`：从第 $9$ 个字符到第 $9$ 个字符

其余子串均不满足条件，因此输出 $8$。
注意：像 `A` 或 `CA` 即使内容相同，只要在原串中的位置不同，就要分别计数。

---

#### 样例 2

**输入**
```text
4
CCBC
```

**输出**
```text
0
```

**说明**
可能不存在任何满足条件的子串。

---

#### 样例 3

**输入**
```text
36
CABACBBBBBAABABACCBCABCCABAABABBCBAC
```

**输出**
```text
136
```

# Code
``` python
import math  
import sys  
from collections import deque, Counter  
  
  
def main():  
  
    n = int(sys.stdin.readline())  
  
    s = sys.stdin.readline().strip()  
  
    # 字串本质双端点， i， j， 再前缀和  
    memo = Counter()  
    pre = 0  
    memo[0] = 1  
  
    curr = 0  
    ans = 0  
    for ch in s:  
  
        if ch == 'A':  
            curr += memo[pre]  
            pre += 1  
  
  
        if ch == 'B':  
            curr -= memo[pre]  
            pre -= 1  
  
  
        ans += curr  
        memo[pre] += 1  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```
