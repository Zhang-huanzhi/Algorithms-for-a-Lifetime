https://atcoder.jp/contests/abc477/tasks/abc477_c

---

### C - Range Search Query

**时间限制**: 2 秒 / **内存限制**: 1024 MiB  
**分值**: 300 分

#### 题目描述
给定两个字符串 $S$ 和 $T$。  
请处理 $Q$ 个询问。每个询问给出两个整数 $L$ 和 $R$，请判断：由字符串 $S$ 的第 $L$ 个字符到第 $R$ 个字符构成的**子串**中，是否包含 $T$ 作为其子串。

*(注：子串是指从原字符串中截取连续的一段字符所构成的新字符串。)*

#### 制约条件
- $S$ 是长度在 $1$ 到 $4 \times 10^5$ 之间、仅由小写英文字母构成的字符串。
- $T$ 是长度在 $1$ 到 $10$ 之间、仅由小写英文字母构成的字符串。
- $1 \le Q \le 2 \times 10^5$
- $1 \le L \le R \le |S|$（$|S|$ 表示字符串 $S$ 的长度）
- $Q, L, R$ 均为整数。

#### 输入格式
输入通过标准输入按以下格式给出：

$$Q$$
$$S$$
$$T$$
$$\text{query}_1$$
$$\text{query}_2$$
$$\vdots$$
$$\text{query}_Q$$

每个询问 $\text{query}_q$ 的格式如下：
$$L \quad R$$

#### 输出格式
输出 $Q$ 行。  
第 $q$ 行输出第 $q$ 个询问的答案：若 $S$ 的第 $L$ 个字符到第 $R$ 个字符构成的子串包含 $T$ 作为子串，则输出 `Yes`；否则输出 `No`。

---

### 输入输出样例

#### 输入样例 1
```text
3
abcdabc
bc
2 6
3 5
3 7
```

#### 输出样例 1
```text
Yes
No
Yes
```

**样例 1 说明：**  
- 对于第 1 个询问：$S$ 的第 2 到第 6 个字符构成的子串是 `bcdab`。`bcdab` 的第 1 到第 2 个字符构成的子串是 `bc`，与 $T$ 一致，因此 `bcdab` 包含 $T$。答案为 `Yes`。
- 对于第 2 个询问：子串为 `cda`，不包含 `bc`，答案为 `No`。
- 对于第 3 个询问：子串为 `cdabc`，包含 `bc`，答案为 `Yes`。

#### 输入样例 2
```text
10
bbbaaabbbaaaaaabbbab
bbb
5 16
5 6
2 16
6 7
17 19
3 7
7 16
7 20
17 19
2 6
```

#### 输出样例 2
```text
Yes
No
Yes
No
No
No
Yes
Yes
No
No
```


# AC 前缀和
``` python
import sys  
import math  
  
def main():  
  
    q = int(sys.stdin.readline().strip())  
    s = sys.stdin.readline().strip()  
    t = sys.stdin.readline().strip()  
  
    # 子串？找一个log复杂度的判断？还是说预处理？  
  
    # 这个t是固定的，所以说，我可以非常简单的，预处理每一个left端点至少需要的右端点  
    n = len(s)  
    m = len(t)  
    arr = [0] * (n + 1)  
  
    for i in range(n):  
        if s[i: i + m] == t:  
            arr[i] += 1  
  
    # 算前缀？  
    pre = [0] * (n + 1)  
    for i in range(1, n + 1):  
        pre[i] = pre[i - 1] + arr[i - 1]  
  
    # print(pre)  
  
    outs = []  
    for _ in range(q):  
        l, r = map(int, sys.stdin.readline().strip().split())  
  
        if r - l + 1 < m:  
            outs.append("No")  
        else:  
            cnt = pre[r - m + 1] - pre[l - 1]  
            if cnt > 0:  
                outs.append("Yes")  
            else:  
                outs.append("No")  
  
    print("\n".join(outs))  
if __name__ == "__main__":  
    main()
```


# AC 后缀
``` python
import sys
import math

def main():

    q = int(sys.stdin.readline().strip())
    s = sys.stdin.readline().strip()
    t = sys.stdin.readline().strip()

    # 子串？找一个log复杂度的判断？还是说预处理？

    # 这个t是固定的，所以说，我可以非常简单的，预处理每一个left端点至少需要的右端点
    n = len(s)
    m = len(t)
    arr = [math.inf] * n

    for left in range(n):

        if s[left: left + m] == t:
            arr[left] = left + m

    # 算下后缀？
    for i in range(n - 2, -1, -1):
        arr[i] = min(arr[i], arr[i + 1])

    # print(arr)

    outs = []
    for _ in range(q):
        l, r = map(int, sys.stdin.readline().strip().split())

        mini = arr[l - 1]

        if r >= mini:
            outs.append("Yes")
        else:
            outs.append("No")

    print("\n".join(outs))
if __name__ == "__main__":
    main()
```