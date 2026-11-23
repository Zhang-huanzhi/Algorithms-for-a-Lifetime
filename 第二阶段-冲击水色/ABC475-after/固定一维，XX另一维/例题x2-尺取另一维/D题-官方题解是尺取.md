https://atcoder.jp/contests/abc485/tasks/abc485_d

---

**D - ABC|ABC|ABC**

时间限制: 2 sec / 内存限制: 1024 MiB

分值：400 分

### 题目描述
给定一个由 `A`、`B`、`C` 组成的长度为 $N$ 的字符串 $S$。

在 2 处位置分割 $S$，将其划分为 3 个非空子串。形式化地说，设 $S_k$ 为 $S$ 的第 $k$ 个字符，选择一对满足 $1 \le i < j < N$ 的整数 $(i, j)$，将 $S$ 分割为 $S_1 S_2 \dots S_i$、$S_{i+1} S_{i+2} \dots S_j$、$S_{j+1} S_{j+2} \dots S_N$ 这 3 个子串。

请计算使得这 3 个子串各自都包含全部字符 `A`、`B`、`C` 的分割方案数。

### 约束条件
- $3 \le N \le 3 \times 10^5$
- $N$ 为整数
- $S$ 是由 `A`、`B`、`C` 组成的长度为 $N$ 的字符串

### 输入
输入通过标准输入以以下格式给出：
```text
N
S
```

### 输出
输出答案。

---

### 样例输入 1
```text
12
AABCCABCCBCA
```

### 样例输出 1
```text
5
```
如果用 `|` 来表示字符串的分割位置，则存在 `AABC|CAB|CCBCA`、`AABC|CABC|CBCA`、`AABC|CABCC|BCA`、`AABCC|ABC|CBCA`、`AABCC|ABCC|BCA` 共 $5$ 种分割方式。

---

### 样例输入 2
```text
3
ABC
```

### 样例输出 2
```text
0
```

---

### 样例输入 3
```text
20
BCACABCACACABCAACBCA
```

### 样例输出 3
```text
65
```

# AC 直接计算另一维
``` python
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    s = ' ' + sys.stdin.readline().strip()  
  
    if 'A' not in s or 'B' not in s or 'C' not in s:  
        print(0)  
        return  
  
    i_min = max(s.find('A'), s.find('B'), s.find('C'))  
    j_max = min(s.rfind('A'), s.rfind('B'), s.rfind('C')) - 1  
  
    ans = 0  
    posA = posB = posC = n + 1  
  
    for i in range(n, 0, -1):  
        if i >= i_min:  
            r = max(posA, posB, posC)  
            if r <= j_max:  
                ans += j_max - r + 1  
  
            if s[i] == 'A':  
                posA = i  
            elif s[i] == 'B':  
                posB = i  
            elif s[i] == 'C':  
                posC = i  
  
    print(ans)  
if __name__ == "__main__":  
    main()
```


# AC 尺取
``` python
import sys

def main():
    n = int(sys.stdin.readline().strip())
    s = ' ' + sys.stdin.readline().strip()

    if 'A' not in s or 'B' not in s or 'C' not in s:
        print(0)
        return

    i_min = max(s.find('A'), s.find('B'), s.find('C'))
    j_max = min(s.rfind('A'), s.rfind('B'), s.rfind('C')) - 1

    ans = 0
    cnt = {'A': 0, 'B': 0, 'C': 0}
    r = i_min  # 初始窗口为 [i_min + 1, i_min]，是个空区间

    # 枚举左分割点 i
    for i in range(i_min, j_max + 1):
        # 1. 右指针 r 不断向右扩展，直到窗口 [i + 1, r] 集齐 A, B, C
        while r < n and (cnt['A'] == 0 or cnt['B'] == 0 or cnt['C'] == 0):
            r += 1
            cnt[s[r]] += 1

        # 2. 如果 r 已经超过了 j_max，后面的 i 只会让 r 更大，直接提前结束
        if r > j_max:
            break

        # 3. 累加当前 i 对应的合法 j 的个数：[r, j_max]
        ans += j_max - r + 1

        # 4. 准备进入下一轮 (i 变成 i + 1)，把 s[i + 1] 移出窗口
        cnt[s[i + 1]] -= 1

    print(ans)

if __name__ == "__main__":
    main()
```