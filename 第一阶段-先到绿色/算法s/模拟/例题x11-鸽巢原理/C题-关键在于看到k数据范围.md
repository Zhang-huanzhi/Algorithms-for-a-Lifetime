https://atcoder.jp/contests/abc448/tasks/abc448_c

---

### C - Except and Min

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：300 分

#### 题目描述
袋子里有编号为 $1$ 到 $N$ 的 $N$ 个球。第 $i$ 个球上写着一个整数 $A_i$。

请处理 $Q$ 次查询。在每次查询中，会给出一个长度为 $K$ 的数列 $B_1, B_2, \dots, B_K$（其中 $B_i$ 互不相同且均在 $1$ 到 $N$ 之间），请按顺序执行以下操作：

1. 将编号为 $B_1, B_2, \dots, B_K$ 的所有球从袋子中取出。
2. 输出当前袋子中剩余球上所写整数的**最小值**。（根据数据范围，保证此时袋子不为空。）
3. 操作结束后，将取出的 $K$ 个球全部放回袋中。

#### 数据范围
- $6 \le N \le 3 \times 10^5$
- $1 \le Q \le 2 \times 10^5$
- $1 \le A_i \le 10^9$
- $1 \le K \le 5$
- $1 \le B_1 < B_2 < \dots < B_K \le N$
- 所有查询中 $K$ 的总和不超过 $4 \times 10^5$
- 输入的所有数值均为整数

---

#### 输入格式
输入从标准输入给出，格式如下。其中 $\text{query}_i$ 表示第 $i$ 次查询：

$$N \quad Q$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$
$$\text{query}_1$$
$$\text{query}_2$$
$$\vdots$$
$$\text{query}_Q$$

每次查询的格式如下：

$$K$$
$$B_1 \quad B_2 \quad \dots \quad B_K$$

#### 输出格式
输出共 $Q$ 行。  
第 $i$ 行输出第 $i$ 次查询的答案。

---

#### 样例输入 1
```plain
6 6
3 2 5 9 1 2
2
4 5
5
1 2 3 4 6
3
2 5 6
4
1 2 5 6
1
5
3
1 2 3
```

#### 样例输出 1
```plain
2
1
3
5
2
1
```

**样例解释 1**：  
例如在第 1 次查询中，从袋子中取出球 4 和球 5。此时袋中剩余以下 4 个球：
- 球 1（数值为 3）
- 球 2（数值为 2）
- 球 3（数值为 5）
- 球 6（数值为 2）

因此，此时袋中剩余球上的最小值为 $2$。


# AC Code 没有什么额外的知识点吧
``` python
import sys  
  
def main():  
  
    n, q = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    arr2 = []  
    for i in range(n):  
        arr2.append((arr[i], i + 1))  
  
    arr2.sort()  
    memo = arr2[:6]  
  
    outs = []  
    for _ in range(q):  
        k = int(sys.stdin.readline().strip())  
        brr = list(map(int, sys.stdin.readline().strip().split()))  
  
        for val, idx in memo:  
            if idx not in brr:  
                outs.append(val)  
                break  
  
    print('\n'.join(map(str, outs)))  
  
if __name__ == "__main__":  
    main()
```