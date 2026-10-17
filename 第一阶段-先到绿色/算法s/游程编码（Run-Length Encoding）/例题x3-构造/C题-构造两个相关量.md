https://atcoder.jp/contests/abc447/tasks/abc447_c

---

### C - 插入与删除 A (Insert and Erase A)

**运行时间限制**: 2 秒 / **内存限制**: 1024 MiB

**分值**: 300 分

#### 题目描述
给出两个由大写英文字母组成的字符串 $S$ 和 $T$。

你可以按任意顺序进行以下两种操作任意多次（也可以是 $0$ 次）：

1. 在 $S$ 的任意位置（包括开头和末尾）插入一个字符 `A`。
2. 从 $S$ 中选择一个字符 `A` 并将其删除。删除后，剩余的字符保持原有顺序拼接在一起。

请判断是否可以将 $S$ 变为与 $T$ 相同的字符串。如果可以，请计算所需操作总次数的最小值。

#### 数据范围与限制
* $S$ 和 $T$ 是由大写英文字母组成的字符串，长度在 $1$ 到 $3 \times 10^5$ 之间。

#### 输入格式
输入从标准输入中给出，格式如下：

$S$
$T$

#### 输出格式
如果可以将 $S$ 变为与 $T$ 一致，输出所需操作总次数的最小值；如果无法实现，则输出 `-1`。

---

### 样例

#### 样例 1
**输入**
```text
ABC
BACA
```
**输出**
```text
3
```
**解释**
可以通过以下总共 3 次操作将 $S$ 变为 $T$：
1. 在 $S$ 的第 2 个字符和第 3 个字符之间插入一个 `A`，此时 $S = \text{ABAC}$。
2. 删除 $S$ 的第 1 个字符 `A`，此时 $S = \text{BAC}$。
3. 在 $S$ 的末尾插入一个 `A`，此时 $S = \text{BACA}$。

无法在 2 次或更少的操作内完成，因此答案是 3。

#### 样例 2
**输入**
```text
ABC
ARC
```
**输出**
```text
-1
```
**解释**
无论进行何种操作，都无法将 $S$ 变为 $T$。

#### 样例 3
**输入**
```text
ABC
ABC
```
**输出**
```text
0
```
**解释**
两个字符串原本就相同，不需要进行任何操作。

#### 样例 4
**输入**
```text
AAAWAZAAAABAAAU
AWAAZABAAAAAUA
```
**输出**
```text
9
```


# AC Code 构造
``` python
import sys  
  
def main():  
  
    s = list(sys.stdin.readline().strip())  
    t = list(sys.stdin.readline().strip())  
  
    s2 = []  
    s3 = []  
  
    cnt = 0  
    for char in s:  
        if char != 'A':  
            s2.append(char)  
            s3.append(cnt)  
            cnt = 0  
        else:  
            cnt += 1  
    s3.append(cnt)  
  
    t2 = []  
    t3 = []  
    cnt = 0  
    for char in t:  
        if char != 'A':  
            t2.append(char)  
            t3.append(cnt)  
            cnt = 0  
        else:  
            cnt += 1  
    t3.append(cnt)  
  
    if s2 != t2:  
        print(-1)  
        return  
  
    ans = 0  
    for a, b in zip(s3, t3):  
        ans += abs(a - b)  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```

# AC Code 双指针
``` python
import sys

def main():
    s = sys.stdin.readline().strip()
    t = sys.stdin.readline().strip()

    # 1. 骨架检查
    if [c for c in s if c != 'A'] != [c for c in t if c != 'A']:
        print(-1)
        return

    # 2. 双指针贪心匹配
    ant = 0
    bnt = 0
    cnt = 0
    n, m = len(s), len(t)

    while ant < n and bnt < m:
        if s[ant] == t[bnt]:
            ant += 1
            bnt += 1
        elif s[ant] == 'A':
            cnt += 1
            ant += 1
        else: # t[bnt] == 'A'
            cnt += 1
            bnt += 1

    # 加上没走完的末尾剩余的 'A'
    cnt += (n - ant) + (m - bnt)
    print(cnt)

if __name__ == "__main__":
    main()
```