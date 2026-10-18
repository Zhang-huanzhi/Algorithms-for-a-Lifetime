https://atcoder.jp/contests/abc477/tasks/abc477_d

---

### D - 遮蔽胶带 (Masking Tape)

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：400 分

#### 题目描述
有 $N$ 个方格排成一排，从左到右依次编号为方格 $1$ 到方格 $N$。颜色用英文字母小写表示。

最初，所有方格的颜色都是 `a`，且每个方格上都没有放置遮蔽胶带（瓷砖）。

接下来会给出 $Q$ 个操作查询，请按给定的顺序依次处理。操作分为以下 2 种类型：

* `1 X`：如果方格 $X$ 上没有胶带，则贴上胶带；如果有胶带，则揭掉胶带。
* `2 C`：将所有**没有贴胶带**的方格的颜色修改为英文字母 $C$。

请输出处理完所有查询后，每个方格的最终颜色（无论该方格最后是否有胶带覆盖）。

#### 数据范围
* $1 \leq N, Q \leq 3 \times 10^5$
* 每个查询均为类型 1 或类型 2 之一
* 类型 1 查询中，$X$ 是满足 $1 \leq X \leq N$ 的整数
* 类型 2 查询中，$C$ 是一个英文字母小写（`a` - `z`）

#### 输入格式
输入从标准输入按以下格式给出：

$$N\ \ Q$$
$$\text{query}_1$$
$$\vdots$$
$$\text{query}_Q$$

每个查询 $\text{query}_i$ ($1 \leq i \leq Q$) 的格式为：
`1 X` 或 `2 C`。

#### 输出格式
输出一个长度为 $N$ 的字符串，表示从左到右每个方格最终的颜色。

---

#### 样例 1

**输入**
```text
3 5
1 2
2 b
1 1
1 2
2 c
```

**输出**
```text
bcc
```

**说明**  
对于胶带的状态，我们用 `#` 表示有胶带，用 `.` 表示没有胶带。  
随着查询的进行，方格的颜色和胶带状态变化如下（初始状态为颜色 `aaa`，胶带 `...`）：
1. 处理第 1 个查询：方格 2 贴上胶带。颜色为 `aaa`，胶带为 `.#.`。
2. 处理第 2 个查询：将未贴胶带的方格颜色改为 `b`。颜色变为 `bab`，胶带为 `.#.`。
3. 处理第 3 个查询：方格 1 贴上胶带。颜色为 `bab`，胶带为 `##.`。
4. 处理第 4 个查询：方格 2 揭掉胶带。颜色为 `bab`，胶带为 `#..`。
5. 处理第 5 个查询：将未贴胶带的方格颜色改为 `c`。颜色变为 `bcc`，胶带为 `#..`。

因此最终输出 `bcc`。虽然方格 1 最终仍然贴着胶带，但依旧要输出该方格本身的颜色。

---

#### 样例 2

**输入**
```text
10 15
1 8
2 m
1 3
1 10
2 q
1 6
1 10
2 d
1 1
2 z
1 9
2 f
1 4
1 7
2 k
```

**输出**
```text
dkmfkqfazk
```


# AC 在线转离线
``` python
import sys
import math

def main():

    n, q = map(int, sys.stdin.readline().strip().split())

    arr = ['a'] * (n + 1)
    # 为什么会是在线查询？这道题看上去需要逆序才对的。
    brr = [False] * (n + 1)
    crr = []
    for i in range(1, q + 1):
        line = list(sys.stdin.readline().strip().split())

        if line[0] == '1':
            x = int(line[1])
            crr.append((1, x))

            brr[x] = not brr[x]

        if line[0] == '2':
            c = line[1]
            crr.append((2, c))

    # 逆序
    crr.reverse()

    memo = set()

    state = [False] * (n + 1)
    for i in range(1, n + 1):
        if not brr[i]:
            memo.add(i)

    for op, num in crr:

        if op == 1:

            brr[num] = not brr[num]

            if not state[num]:
                if not brr[num]:
                    memo.add(num)
                else:
                    memo.discard(num)

        if op == 2:
            # 这个就不一样了，只要确定了，就不会再改
            for x in memo:
                arr[x] = num
                state[x] = True

            memo.clear()

    print(''.join(arr[1:]))

if __name__ == "__main__":
    main()
```