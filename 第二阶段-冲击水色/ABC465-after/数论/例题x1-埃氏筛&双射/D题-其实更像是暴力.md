https://atcoder.jp/contests/abc475/tasks/abc475_d

---

### D - 字母算式素数 (Alphametic Prime)

时间限制：2 秒 / 内存限制：1024 MiB  
得分：400 分

---

### 题目描述

给定一个由小写英文字母组成的字符串 $S$。如果存在满足以下条件的素数（质数）$P$，请输出其中任意一个。

**条件：**
设将 $P$ 表示为没有前导零的十进制字符串为 $T$。
$|S| = |T|$，且对于所有满足 $1 \le i < j \le |S|$ 的下标对 $(i, j)$，均有 $S_i = S_j$ 与 $T_i = T_j$ 等价（即相同的字母必须映射为相同的数字，不同的字母必须映射为不同的数字，形成一一对应关系）。

---

### 制约条件

* $S$ 是长度在 $1$ 以上、$7$ 以下的由小写英文字母构成的字符串。

---

### 输入格式

输入从标准输入给出，格式如下：

> $S$

---

### 输出格式

如果不存在满足条件的素数，输出 `-1`。  
如果存在，输出满足条件的任意一个素数。若有多个满足条件的答案，输出其中任意一个均视为正确。

---

### 样例 1

**输入**
```text
motor
```

**输出**
```text
10607
```

*解释：除了该答案外，输出如 `40709` 等也被视为正确。但例如 `81817` 则不满足条件（因为字母 `m` 和 `t` 都被映射成了数字 `8`，不满足不同字母对应不同数字的条件），属于错误答案。*

---

### 样例 2

**输入**
```text
byebye
```

**输出**
```text
-1
```

*解释：当不存在满足条件的素数时，输出 `-1`。*

---

### 样例 3

**输入**
```text
coconut
```

**输出**
```text
1010237
```

### Code 正向构造
``` python
 import sys  
  
  
def main():  
    s = sys.stdin.readline().strip()  
  
    # 什么玩意题意  
    # 构造，还要满足是质数，七位数，也不是很大嘛，直接把所有的质数提前算出来，然后一个个套不就得了。  
  
    n = len(s)  
  
    # 埃氏筛？直接套模板了呀  
    def f(n):  
        high = 10 ** n  
        is_prime = bytearray([1]) * high  
        if high > 0:  
            is_prime[0] = 0  
        if high > 1:  
            is_prime[1] = 0  
  
        for i in range(2, int(high ** 0.5) + 1):  
            if is_prime[i]:  
                is_prime[i * i: high: i] = b"\x00" * (  
                        (high - 1 - i * i) // i + 1  
                )  
        return is_prime  
  
    arr = f(n)  
  
    low = 10 ** (n - 1) if n > 1 else 2  
  
    brr = [i for i in range(low, 10 ** n) if arr[i]]  
  
    for num in brr:  
  
        crr = str(num)  
  
        memo1 = dict()  
        memo2 = dict()  
        state = True  
  
        for c1, c2 in zip(s, crr):  
  
            if c1 in memo1 and memo1[c1] != c2:  
                state = False  
                break  
            if c2 in memo2 and memo2[c2] != c1:  
                state = False  
                break  
            memo1[c1] = c2  
            memo2[c2] = c1  
  
        if state:  
            print(num)  
            break  
    else:  
        print(-1)  
  
  
if __name__ == "__main__":  
    main()
```