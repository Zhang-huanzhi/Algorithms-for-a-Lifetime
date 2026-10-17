https://atcoder.jp/contests/abc447/tasks/abc447_d

---

### D - 拿走 ABC 2 (Take ABC 2)

**运行时间限制**: 2 秒 / **内存限制**: 1024 MiB

**分值**: 400 点

#### 题目描述
给定一个仅由字符 `A`、`B`、`C` 这三种字符组成的字符串 $S$。

我们定义一次操作如下：
* 选择一组下标 $(i, j, k)$，满足 $1 \le i < j < k \le |S|$，且 $S_i = \text{'A'}$、$S_j = \text{'B'}$、$S_k = \text{'C'}$。
* 将 $S$ 中的第 $i$、第 $j$、第 $k$ 个字符删除。剩余的字符保持原有的相对顺序拼接在一起（向左靠齐）。

请计算对字符串 $S$ 最多可以进行多少次操作。

#### 数据范围与限制
* $1 \le |S| \le 10^6$
* $S$ 是仅由 `A`、`B`、`C` 组成的字符串

#### 输入格式
输入从标准输入中给出，格式如下：

$S$

#### 输出格式
输出一个整数，表示最多可以进行的操作次数。

---

### 样例

#### 样例 1
**输入**
```text
ABACBCC
```
**输出**
```text
2
```
**解释**
可以通过以下步骤进行 2 次操作：
1. 对 `ABACBCC` 选择 $(i, j, k) = (1, 2, 7)$ 进行操作（取出第 1 个 A、第 2 个 B、第 7 个 C），剩余字符串为 `ACBC`。
2. 对 `ACBC` 选择 $(i, j, k) = (1, 3, 4)$ 进行操作（取出第 1 个 A、第 3 个 B、第 4 个 C），剩余字符串为 `C`。

无法进行 3 次或更多的操作，因此答案为 2。

#### 样例 2
**输入**
```text
CBACBB
```
**输出**
```text
0
```
**解释**
不存在满足 $i < j < k$ 且分别为 A、B、C 的下标三元组，无法进行任何操作。

#### 样例 3
**输入**
```text
BBBAAABCBCBAACBBCAAC
```
**输出**
```text
5
```

# AC Code

``` python
import sys  
  
def main():  
  
    s = list(sys.stdin.readline().strip())  
  
    a = 0  
    ab = 0  
    ans = 0  
  
    for char in s:  
        if char == 'A':  
            a += 1  
  
        elif char == 'B':  
            if a > 0:  
                a -= 1  
                ab += 1  
  
        elif char == 'C':  
            if ab > 0:  
                ab -= 1  
                ans += 1  
  
    print(ans)  
  
if __name__ == "__main__":  
    main()
```