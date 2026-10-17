https://atcoder.jp/contests/abc474/tasks/abc474_e

---

### E - One Time Coupon
**时间限制**: 2 秒 / **内存限制**: 1024 MiB  
**分值**: 450 分

#### 题目描述
某家商店出售 $N$ 种商品。每种商品都可以购买任意多次。

第 $i$ 种商品 $(1 \le i \le N)$ 可以通过以下两种方式之一购买：
1. **不使用优惠券**，以 $A_i$ 日元的价格购买，并获得 $1$ 张优惠券。
2. **使用 $1$ 张优惠券**，以 $B_i$ 日元的价格购买。

最初，你手里没有任何优惠券（即优惠券数量为 $0$）。

请求出购买**每种商品各至少一次**所需的最小总金额。

每个测试点包含 $T$ 组测试用例，请分别求解。

#### 制约条件
* $1 \le T \le 2 \times 10^5$
* $1 \le N \le 2 \times 10^5$
* $1 \le B_i < A_i \le 10^9$
* 所有测试用例中 $N$ 的总和不超过 $2 \times 10^5$
* 输入的所有值均为整数

#### 输入格式
输入从标准输入中给出，格式如下：

$T$  
$\text{case}_1$  
$\text{case}_2$  
$\vdots$  
$\text{case}_T$  

每个测试用例 $\text{case}_i$ 的格式如下：

$N$  
$A_1$ $B_1$  
$A_2$ $B_2$  
$\vdots$  
$A_N$ $B_N$  

#### 输出格式
依次输出每个测试用例的答案，每个答案占一行。

---

### 样例输入 1
```text
3
5
11 6
6 5
2 1
8 3
7 4
4
5 1
5 2
5 3
5 4
6
24 13
24 2
50 12
35 25
28 26
10 1
```

### 样例输出 1
```text
23
13
100
```

#### 样例说明 1
对于第 $1$ 个测试用例：

你可以按照以下步骤操作，总共花费 $23$ 日元买到所有种类的商品至少一次：
1. 不使用优惠券，以 $6$ 日元购买第 $2$ 种商品。此时持有 $1$ 张优惠券。
2. 不使用优惠券，以 $2$ 日元购买第 $3$ 种商品。此时持有 $2$ 张优惠券。
3. 不使用优惠券，以 $2$ 日元再次购买第 $3$ 种商品。此时持有 $3$ 张优惠券。
4. 使用 $1$ 张优惠券，以 $6$ 日元购买第 $1$ 种商品。此时持有 $2$ 张优惠券。
5. 使用 $1$ 张优惠券，以 $3$ 日元购买第 $4$ 种商品。此时持有 $1$ 张优惠券。
6. 使用 $1$ 张优惠券，以 $4$ 日元购买第 $5$ 种商品。此时持有 $0$ 张优惠券。

总花费为 $6 + 2 + 2 + 6 + 3 + 4 = 23$ 日元。不存在花费少于 $23$ 日元的购买方案，因此第 1 行输出 `23`。


# AC Code 枚举贪心
``` python
import math  
import sys  
  
def main():  
    t = int(sys.stdin.readline().strip())  
    outs = []  
    for _ in range(t):  
        n = int(sys.stdin.readline().strip())  
  
        arr = []  
        diff = []  
        total = 0  
        cost = math.inf  
        for _ in range(n):  
            a, b = map(int, sys.stdin.readline().strip().split())  
            arr.append((a, b))  
            diff.append(a - b)  
            total += a  
            # 刷一个卷的最小成本  
            if a < cost:  
                cost = a  
  
        # 降序排序，省钱最多的在前面  
        diff.sort(reverse=True)  
  
        ans = total  # 初始值：一张券都不用时的总花费  
        y = 0  # 记录前 k 个商品用券累计节省的金额  
  
        # 枚举使用了 k 张优惠券 (k 从 1 到 n)        for k in range(1, n + 1):  
            y += diff[k - 1]  
            extra = max(0, 2 * k - n) * cost  # 刷券的额外成本  
            ans = min(ans, total - y + extra)  
  
        outs.append(ans)  
  
    print('\n'.join(map(str, outs)))  
  
if __name__ == "__main__":  
    main()
```