https://atcoder.jp/contests/abc431/tasks/abc431_c

---

### C - Robot Factory

**时间限制**: 2 秒 / **内存限制**: 1024 MiB  
**分值**: 300 分

#### 题目描述
高桥君可以通过组合 1 个头部零件和 1 个身体零件来制作 1 台机器人。如果机器人的头部零件重量大于身体零件的重量，机器人就会倒下（换言之，若头部重量小于或等于身体重量，机器人就不会倒下）。

现在，高桥君拥有 $N$ 个头部零件和 $M$ 个身体零件。他拥有的第 $i$ ($1 \le i \le N$) 个头部零件重量为 $H_i$ 克，第 $i$ ($1 \le i \le M$) 个身体零件重量为 $B_i$ 克。

高桥君希望通过合理搭配手头的零件，总共制作出 $K$ 台不会倒下的机器人。请判断高桥君是否能够达成这一目标。

注意，一个零件不能同时用于多台机器人，且制作一台机器人不能使用 2 个及以上的头部零件（或 2 个及以上的身体零件）。

#### 数据范围
* $1 \le N \le 2 \times 10^5$
* $1 \le M \le 2 \times 10^5$
* $1 \le K \le \min(N, M)$
* $1 \le H_i \le 10^9$ ($1 \le i \le N$)
* $1 \le B_i \le 10^9$ ($1 \le i \le M$)
* 所有输入均为整数。

#### 输入格式
输入按以下格式从标准输入给出：
> $N \quad M \quad K$  
> $H_1 \quad H_2 \quad \dots \quad H_N$  
> $B_1 \quad B_2 \quad \dots \quad B_M$

#### 输出格式
如果高桥君能够通过合理搭配零件制作出 $K$ 台不会倒下的机器人，输出 `Yes`；否则输出 `No`。

---

#### 样例 1
**输入**
```text
6 6 3
2 7 1 8 2 8
1 8 2 8 4 5
```
**输出**
```text
Yes
```
**样例说明 1**  
如果我们用 $(i, j)$ 表示将第 $i$ 个头部零件和第 $j$ 个身体零件进行组合，高桥君可以通过例如组合 $(1, 2)$、$(2, 4)$、$(3, 6)$ 来制作出 3 台不会倒下的机器人。因此，输出 `Yes`。

#### 样例 2
**输入**
```text
1 1 1
43
1
```
**输出**
```text
No
```
**样例说明 2**  
高桥君拥有的头部零件太重了，无法制作出不会倒下的机器人。

#### 样例 3
**输入**
```text
1 1 1
100
100
```
**输出**
```text
Yes
```
**样例说明 3**  
请注意，当头部零件与身体零件的重量相等时，机器人也不会倒下。

#### 样例 4
**输入**
```text
12 15 12
748 169 586 329 972 529 432 519 408 587 138 249
656 114 632 299 984 755 404 772 155 506 832 854 353 465 387
```
**输出**
```text
Yes
```


# AC Code
``` python
import sys  
  
def main():  
  
    n, m, k = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    arr.sort()  
    brr.sort()  
  
    cnt = 0  
    head = 0  
    body = 0  
    while head < n and body < m:  
        if arr[head] <= brr[body]:  
            cnt += 1  
            head += 1  
            body += 1  
        else:  
            body += 1  
  
        # 其实可以不写，不过算是剪枝优化  
        if cnt == k:  
            break  
  
    if cnt >= k:  
        print("Yes")  
    else:  
        print("No")  
  
if __name__ == "__main__":  
    main()
```