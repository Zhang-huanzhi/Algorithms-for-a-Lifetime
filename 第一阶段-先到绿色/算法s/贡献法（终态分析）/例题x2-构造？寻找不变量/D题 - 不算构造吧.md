https://atcoder.jp/contests/abc426/tasks/abc426_d

---

### D - Pop and Insert 
时间限制: 2 秒 / 内存限制: 1024 MiB

分值 : 400 分

#### 问题描述
给定一个由 $0$ 和 $1$ 组成、长度为 $N$ 的字符串 $S$。

你可以对 $S$ 进行以下操作任意次（也可以是 $0$ 次）：

* 删除开头或末尾的一个字符，将该字符翻转（如果是 $0$ 则变为 $1$，如果是 $1$ 则变为 $0$），然后重新插入到任意位置。更严谨地，定义 $r(0) = 1$，$r(1) = 0$，进行以下操作之一：（其中 $S_i$ 表示 $S$ 的第 $i$ 个字符。）
  * 选择任意一个 $i$ ($1 \le i \le N$)，将 $S$ 变更为 $S_2 \dots S_i r(S_1) S_{i+1} \dots S_N$。
  * 选择任意一个 $i$ ($0 \le i \le N-1$)，将 $S$ 变更为 $S_1 \dots S_i r(S_N) S_{i+1} \dots S_{N-1}$。

请计算使 $S$ 的所有字符都变得相同的最小操作次数。可以证明，必然存在这样的操作序列。

给定 $T$ 个测试用例，请对每个测试用例求解。

#### 数据范围
* $1 \le T \le 2 \times 10^5$
* $2 \le N \le 5 \times 10^5$
* $T, N$ 是整数。
* $S$ 是由 $0$ 和 $1$ 组成的长度为 $N$ 的字符串。
* 所有测试用例中 $N$ 的总和不超过 $5 \times 10^5$。

#### 输入格式
输入按以下格式从标准输入给出。

$$T$$
$$\text{case}_1$$
$$\text{case}_2$$
$$\vdots$$
$$\text{case}_T$$

每个测试用例 $\text{case}_i$ 的格式如下：

$$N$$
$$S$$

#### 输出格式
输出 $T$ 行。
第 $i$ 行 ($1 \le i \le T$) 应输出第 $i$ 个测试用例的答案。

---

#### 样例输入 1
```text
3
5
01001
3
000
15
110010111100101
```

#### 样例输出 1
```text
4
0
16
```

**样例解释：**
* 对于第 1 个测试用例，例如通过以下 4 次操作，可以使 $S$ 的所有字符都变为 $0$。由于无法在 3 次或更少的操作内使 $S$ 的所有字符都相同，因此答案为 4。
  1. 删除开头的字符 $0$，将其翻转后的 $1$ 插入到（删除该字符后 $S$ 的）第 1 个字符与第 2 个字符之间。此时 $S = 11001$。
  2. 删除开头的字符 $1$，将其翻转后的 $0$ 插入到（删除该字符后 $S$ 的）第 2 个字符与第 3 个字符之间。此时 $S = 10001$。
  3. 删除末尾的字符 $1$，将其翻转后的 $0$ 插入到（删除该字符后 $S$ 的）末尾。此时 $S = 10000$。
  4. 删除开头的字符 $1$，将其翻转后的 $0$ 插入到（删除该字符后 $S$ 的）开头。此时 $S = 00000$。
* 对于第 2 个测试用例，由于 $S$ 的所有字符从一开始就相同，因此不需要进行任何操作（0次）。

# Code

``` python
import sys  
  
def main():  
  
    t = int(sys.stdin.readline().strip())  
  
    for _ in range(t):  
        n = int(sys.stdin.readline().strip())  
        s = sys.stdin.readline().strip()  
  
        arr = [0 if char == '0' else 1 for char in s]  
  
        total0 = 0  
        total1 = 0  
        for num in arr:  
            if num == 1:  
                total1 += 1  
            else:  
                total0 += 1  
        # print(total0, total1)  
  
        len0 = 0  
        len1 = 0  
  
        state = arr[0]  
        cnt = 1  
        for num in arr[1:]:  
            if num == state:  
                cnt += 1  
            else:  
                if state == 1 and cnt > len1:  
                    len1 = cnt  
                elif state == 0 and cnt > len0:  
                    len0 = cnt  
                cnt = 1  
                state = num  
  
        if state == 1 and cnt > len1:  
            len1 = cnt  
        elif state == 0 and cnt > len0:  
            len0 = cnt  
  
        # print(len0, len1)  
  
         # 代价公式  
        ans0 = total1 + 2 * (total0 - len0)  
        ans1 = total0 + 2 * (total1 - len1)  
  
        print(min(ans0, ans1))  
  
if __name__ == "__main__":  
    main()
```