https://atcoder.jp/contests/abc468/tasks/abc468_d

---

### D - Pre-Palindrome

**时间限制**: 2 秒 / **内存限制**: 1024 MiB

**分值**: 400 分

#### 问题描述

我们称一个由小写英文字母组成的字符串为**好字符串**，当且仅当它满足以下条件：

* 可以通过修改**至多 1 个**字符使其变成回文字符串。

例如，`a`、`iwai`、`abcdcza` 等都是好字符串，而 `abcd`、`atcoder` 等则不是好字符串。需要特别注意的是，回文字符串本身也是好字符串（因为可以通过修改 0 个字符使其变成回文）。

现在给定一个由小写英文字母组成的字符串 $S$。

请计算 $S$ 的所有非空子串（连续子序列）中，有多少个是好字符串。

注意：即使两个子串的内容完全相同，只要它们在 $S$ 中的起始或结束位置不同，就应当视作不同的子串分别进行计数。

<details>
<summary><b>什么是子串？（点击展开/折叠）</b></summary>

字符串的子串是指通过删除原字符串开头和末尾的零个或多个字符后得到的连续字符序列。例如，`ace` 是 `abcde` 的子序列但不是子串，而 `bcd` 是 `abcde` 的子串。

</details>

#### 数据范围

- $S$ 是一个由小写英文字母组成的字符串。
- $S$ 的长度在 $1$ 以上且不超过 $10^4$。

#### 输入格式

输入由标准输入按以下格式给出：

$$S$$

#### 输出格式

输出一个整数，表示好字符串子串的个数。

---

#### 样例输入 1
```text
ababa
```

#### 样例输出 1
```text
13
```
$S$ 中从第 2 个字符到第 3 个字符组成的 `ba`、以及从第 1 个字符到第 5 个字符组成的 `ababa` 等都是好字符串。

在 $S$ 的全部 15 个非空子串中，除了从第 1 个字符到第 4 个字符组成的 `abab`，以及从第 2 个字符到第 5 个字符组成的 `baba` 这 2 个子串外，其余的 13 个子串均为好字符串。因此输出 13。

---

#### 样例输入 2
```text
atcoder
```

#### 样例输出 2
```text
18
```

---

#### 样例输入 3
```text
abccbacbacb
```

#### 样例输出 3
```text
40
```

# AC Code
``` python
import itertools
import sys

def main():
    
    arr = list(sys.stdin.readline().strip())

    # 判断是否回文串？ 中心扩展
    # 可以修改一个？ 动态规划？有必要吗？中心扩展在加一个state就行了吧

    n = len(arr)
    ans = 0
    for i in range(n):
        l = i
        r = i
        state = False
        while l >=0 and r < n:

            if arr[l] != arr[r]:

                if state:
                    break

                else:
                    state = not state
            l -= 1
            r += 1
            ans += 1

    # 还要判断偶数
    for i in range(n):
        l = i
        r = i + 1
        state = False
        while l >=0 and r < n:

            if arr[l] != arr[r]:

                if state:
                    break

                else:
                    state = not state
            l -= 1
            r += 1
            ans += 1


    print(ans)

if __name__ == "__main__":
    main()
```