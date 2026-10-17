https://atcoder.jp/contests/abc449/tasks/abc449_c

----

### C - Comfortable Distance

**时间限制**: 2 秒 / **内存限制**: 1024 MiB  
**分值**: 300 分

#### 问题描述
给定一个长度为 $N$ 且仅由小写英文字母组成的字符串 $S$。

请计算满足以下所有条件的整数对 $(i, j)$ 的个数：

* $1 \le i \le j \le N$
* $S_i = S_j$（即字符串中第 $i$ 个字符与第 $j$ 个字符相同）
* $L \le j - i \le R$

#### 数据范围
* $2 \le N \le 5 \times 10^5$
* $1 \le L \le R \le N - 1$
* $N, L, R$ 均为整数。
* $S$ 是一个长度为 $N$ 且仅由小写英文字母组成的字符串。

#### 输入格式
输入按以下格式从标准输入给出：

$$N \quad L \quad R$$
$$S$$

#### 输出格式
输出一个整数，表示满足条件的整数对 $(i, j)$ 的个数。

---

#### 样例输入 1
```text
6 2 4
aabcba
```

#### 样例输出 1
```text
2
```
**样例解释 1**：  
满足所有条件的整数对有 $(i, j) = (2, 6)$ 和 $(3, 5)$，共 2 个。

---

#### 样例输入 2
```text
9 3 6
aaaaaaaaa
```

#### 样例输出 2
```text
18
```

---

#### 样例输入 3
```text
10 2 6
aabbccaabb
```

#### 样例输出 3
```text
6
```


# Ac Code
``` python
import sys  
from collections import Counter  
  
def main():  
  
    n, l, r = map(int, sys.stdin.readline().strip().split())  
    s = sys.stdin.readline().strip()  
  
    # 时间窗口解法  
    # 大小固定的滑动时间窗口  
    window = Counter()  
    ans = 0  
  
    for i in range(n):  
  
        # 对于这个窗口，新来的字符是？  
        # 还要保障其在范围里  
        if i - l >= 0:  
            in_char = s[i - l]  
            window[in_char] += 1  
  
        if i - r - 1 >= 0:  
            out_char = s[i - r]  
            window[out_char] -= 1  
  
        ans += window[s[i]]  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```