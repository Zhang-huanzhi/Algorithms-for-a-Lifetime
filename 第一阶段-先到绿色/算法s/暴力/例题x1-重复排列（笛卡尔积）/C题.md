https://atcoder.jp/contests/abc416/tasks/abc416_d

---

### C - Concat (X-th)

时间限制: 2 sec / 空间限制: 1024 MiB
分值: 300 分

#### 题目描述
给出 $N$ 个字符串 $S_1, \dots, S_N$。

对于所有元素均在 $1$ 到 $N$ 之间且长度为 $K$ 的数列 $(A_1, \dots, A_K)$，我们将字符串 $f(A_1, \dots, A_K)$ 定义为 $S_{A_1} + S_{A_2} + \dots + S_{A_K}$。这里的 $+$ 表示字符串的拼接。

将所有 $N^K$ 个数列对应的 $f(A_1, \dots, A_K)$ 按字典序升序排列，求其中第 $X$ 小的字符串。

#### 数据范围
- $1 \le N \le 10$
- $1 \le K \le 5$
- $1 \le X \le N^K$
- $S_i$ 是长度不超过 $10$ 且仅由小写英文字母组成的字符串。
- $N, K, X$ 均为整数。

#### 输入格式
输入通过标准输入以下列格式给出：
> $N$ $K$ $X$
> $S_1$
> $\vdots$
> $S_N$

#### 输出格式
输出对应的字符串。

---

#### 输入样例 1
```text
3 2 6
abc
xxx
abc
```

#### 输出样例 1
```text
abcxxx
```

**样例解释 1**
各数列对应的字符串如下：
- $f(1,1) =$ `abcabc`
- $f(1,2) =$ `abcxxx`
- $f(1,3) =$ `abcabc`
- $f(2,1) =$ `xxxabc`
- $f(2,2) =$ `xxxxxx`
- $f(2,3) =$ `xxxabc`
- $f(3,1) =$ `abcabc`
- $f(3,2) =$ `abcxxx`
- $f(3,3) =$ `abcabc`

将这些字符串按字典序升序排列后为：`abcabc`, `abcabc`, `abcabc`, `abcabc`, `abcxxx`, `abcxxx`, `xxxabc`, `xxxabc`, `xxxxxx`。
其中第 $6$ 小的字符串是 `abcxxx`。

---

#### 输入样例 2
```text
5 5 416
a
aa
aaa
aa
a
```

#### 输出样例 2
```text
aaaaaaa
```