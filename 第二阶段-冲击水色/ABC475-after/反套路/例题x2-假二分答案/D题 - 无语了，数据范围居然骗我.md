https://atcoder.jp/contests/abc482/tasks/abc482_d：

---

### D - Wannabe Zombie
**运行时间限制：** 2 秒 / **内存限制：** 1024 MiB  
**分值：** 400 分

#### 题目描述
数轴上站着 $N$ 个人，编号为 $1, 2, \dots, N$。  
人 $i$ 所在的初始坐标为 $X_i$。对于所有 $i = 1, 2, \dots, N$，$X_i$ 均为偶数，且满足 $X_1 < X_2 < \dots < X_N$。

对于每个 $k = 1, 2, \dots, N$，请解答以下问题：

> 在时刻 $0$，人 $k$ 变成了僵尸（丧尸），其他人仍保持为人类。  
> 从时刻 $0$ 开始，人类和僵尸都可以在数轴上以每单位时间不超过 $1$ 的速度自由移动。  
> 当僵尸与人类处于同一坐标时，该人类也会变成僵尸。  
> 请计算所有人全部变成僵尸的最早时刻（即最小时间）。

在本题的数据制约下，可以证明所求的答案一定是一个整数。

#### 数据范围与限制
- $1 \le N \le 2 \times 10^5$
- $0 \le X_1 < X_2 < \dots < X_N \le 10^9$
- $X_i$ 均为偶数
- 所有输入值均为整数

#### 输入格式
输入从标准输入按以下格式给出：
```text
N
X_1 X_2 … X_N
```

#### 输出格式
输出共 $N$ 行。  
第 $i$ 行（$1 \le i \le N$）输出 $k = i$ 时的答案。

---

### 样例 1

#### 输入
```text
5
2 4 10 14 18
```

#### 输出
```text
8
7
6
6
8
```

#### 解释
- 当 $k = 1$ 时：人 1 以速度 1 向数轴正方向移动，人 2, 3, 4, 5 以速度 1 向数轴负方向移动，可以在时刻 8 之前使所有人全部变成僵尸。
- 当 $k = 2$ 时：各人按如下方式移动，可以在时刻 7 之前使所有人全部变成僵尸：
  - 人 1：以速度 1 向正方向移动。
  - 人 2：在时刻 3 之前以速度 1 向正方向移动，时刻 3 之后以速度 1 向负方向移动。
  - 人 3：在时刻 3 之前以速度 1 向负方向移动，时刻 3 之后以速度 1 向正方向移动。
  - 人 4：以速度 1 向负方向移动。
  - 人 5：以速度 1 向负方向移动。

---

### 样例 2

#### 输入
```text
9
140322306 171833796 356303608 595703832 701965684 798563428 857902324 933683272 971602066
```

#### 输出
```text
415639880
399884135
307649229
280821689
280821689
329120561
358790009
396680483
415639880
```

# AC Code 简化
``` python
import math
import sys

def main():

    n = int(sys.stdin.readline().strip())

    arr: list[int]= list(map(int, sys.stdin.readline().strip().split()))

    # 第k人，答案只取决于于第K人距离最远的那个人 不对不对，还得考虑左右分别谁最远，不对不对，貌似情况有点复杂

    outs = []
    for k in range(n):

        if k == 0 or k == n - 1:
           outs.append( (arr[-1] - arr[0]) // 2)

        else:

            ans1 = max(arr[k] - arr[0], arr[-1] - arr[k - 1]) // 2

            # 反之
            ans2 = max(arr[k + 1] - arr[0], arr[-1] - arr[k]) // 2

            outs.append(min(ans1, ans2))

    print('\n'.join(map(str, outs)))

if __name__ == "__main__":
    main()
```

# AC Code 
``` python
import math
import sys

def main():

    n = int(sys.stdin.readline().strip())

    arr = list(map(int, sys.stdin.readline().strip().split()))

    # 第k人，答案只取决于于第K人距离最远的那个人 不对不对，还得考虑左右分别谁最远，不对不对，貌似情况有点复杂

    outs = []
    for k in range(n):

        if k == 0 or k == n - 1:
           outs.append( (arr[-1] - arr[0]) // 2)

        else:

            # 一开始k往left跑，感染k-1个人
            left = (arr[k] - arr[k - 1]) // 2

            # 两个人分开跑，检查最左边和最右边的人各要花多久
            x = ((arr[k] - arr[0]) - left * 2 ) // 2
            y = (arr[n - 1] - arr[k]) // 2

            ans1 = left + max(x, y)

            # 反之
            right = (arr[k + 1] - arr[k]) // 2

            x = (arr[k] - arr[0]) // 2
            y = ((arr[n - 1] - arr[k]) - right * 2) // 2

            ans2 = right + max(x, y)

            outs.append(min(ans1, ans2))

    print('\n'.join(map(str, outs)))

if __name__ == "__main__":
    main()
```