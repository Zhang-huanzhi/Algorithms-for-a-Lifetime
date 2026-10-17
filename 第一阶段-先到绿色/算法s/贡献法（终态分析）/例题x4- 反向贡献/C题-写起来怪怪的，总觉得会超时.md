https://atcoder.jp/contests/abc439/tasks/abc439_c

---

### C - 2026

时间限制: 2 秒 / 内存限制: 1024 MiB

分值 : 300 分

#### 题目描述
当正整数 $n$ 满足以下条件时，我们称 $n$ 为 **优秀整数**（Good Integer）：

* 存在**唯一**一组整数组 $(x,y)$ 满足 $0 < x < y$ 且 $x^2 + y^2 = n$。

例如，若 $n = 2026$，可以验证满足 $0 < x < y$ 且 $x^2 + y^2 = n$ 的整数组仅有 $(x,y) = (1,45)$。因此，$2026$ 是一个优秀整数。

现给出一个正整数 $N$。请列举出所有小于或等于 $N$ 的优秀整数。

#### 约束条件
* $1 \le N \le 10^7$
* $N$ 是整数

#### 输入
输入通过标准输入按以下格式给出：
> $N$

#### 输出
设小于或等于 $N$ 的优秀整数共有 $k$ 个，将其按升序排列后的序列为 $(a_1, a_2, \dots, a_k)$。
请按以下格式输出答案。（若 $k=0$，则第二行输出为空行。）

> $k$
> $a_1$ $a_2$ $\dots$ $a_k$

---

#### 输入样例 1
```text
10
```

#### 输出样例 1
```text
2
5 10
```
* 满足 $0 < x < y$ 且 $x^2 + y^2 = 5$ 的整数组仅有 $(x,y)=(1,2)$，因此 $5$ 是优秀整数。
* 满足 $0 < x < y$ 且 $x^2 + y^2 = 10$ 的整数组仅有 $(x,y)=(1,3)$，因此 $10$ 是优秀整数。

小于或等于 $10$ 的优秀整数仅有这两个。

#### 输入样例 2
```text
1
```

#### 输出样例 2
```text
0

```
*(注：由于 $k=0$，第二行为空行。)*

#### 输入样例 3
```text
50
```

#### 输出样例 3
```text
14
5 10 13 17 20 25 26 29 34 37 40 41 45 50
```


---

# AC Code
``` python
import itertools
import math
import sys

def main():

    n = int(sys.stdin.readline().strip())
    # 最大 3163
    memo = []

    for i in range(1, (math.isqrt(n) + 1)):
        x = i * i
        memo.append(x)

    counter = [0] * (n + 1)
    for x, y in itertools.combinations(memo, 2):

        num = x + y
        if num <= n:
            counter[num] += 1

    out = [num for num in range(1, n + 1) if counter[num] == 1]
    out.sort()
    print(len(out))
    print(*out)
if __name__ == "__main__":
    main()
```