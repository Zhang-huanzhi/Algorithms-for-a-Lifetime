
**TLE（Time Limit Exceeded，运行超时）**

这道题给的数据范围

- 1≤N≤10 ** 12

这个N太大了，单纯的for i in range(n)的话必定超时的

暴力本身是讲1~N的每个数字都判断是不是会回文，但回文其实是很特殊的。你一个个遍历拿百分之九十九都不是。

所以要转换思路 - 主动去**构造**


---

``` python
import sys

def solve():

    A = int(sys.stdin.readline().strip())

    N = int(sys.stdin.readline().strip())

    # 回文串
    total_sum = 0
    for num in range(1, N+1):

        if str(num) != str(num)[::-1]:
            continue

        arr = []

        x = num

        while x > 0:

            y = x % A
            x = x // A

            arr.append(y)

        if arr == arr[::-1]:
            total_sum += num

    print(total_sum)

if __name__ == '__main__':
    solve()
```