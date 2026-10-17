https://atcoder.jp/contests/abc474/tasks/abc474_d

---

### D - Outweigh

**时间限制**: 2 秒 / **内存限制**: 1024 MiB  
**分值**: 400 分

#### 题目描述
有 $N$ 种石头，编号为 $1, 2, \dots, N$。同一种类的石头重量均相同。  
高桥和青木分别拥有 $A_i$ 个和 $B_i$ 个第 $i$ 种石头。

请判断是否存在满足以下条件的正整数序列 $W = (W_1, W_2, \dots, W_N)$。若存在，请构造并输出任意一个解：

- $1 \le W_i \le 10^{18}$
- 当第 $i$ 种石头的重量设为 $W_i$ 时，高桥所拥有的石头总重量**严格大于**青木所拥有的石头总重量。

#### 数据范围
- $1 \le N \le 10^5$
- $1 \le A_i \le 10^9$
- $1 \le B_i \le 10^9$
- 所有输入数据均为整数。

---

#### 输入格式
输入通过标准输入按以下格式给出：

$$N$$  
$$A_1\ A_2\ \dots\ A_N$$  
$$B_1\ B_2\ \dots\ B_N$$  

#### 输出格式
若存在满足条件的正整数序列 $W = (W_1, W_2, \dots, W_N)$，按以下格式输出：

```text
Yes
W_1 W_2 ... W_N
```

若不存在满足条件的序列，则在单行输出：

```text
No
```

---

### 样例 1

#### 输入样例 1
```text
3
4 7 4
5 5 5
```

#### 输出样例 1
```text
Yes
4 7 4
```

**样例解释 1**  
当 $W=(4,7,4)$ 时：  
- 高桥持有的石头总重量为：$4 \times 4 + 7 \times 7 + 4 \times 4 = 81$。  
- 青木持有的石头总重量为：$4 \times 5 + 7 \times 5 + 4 \times 5 = 75$。  
高桥的总重量严格大于青木的总重量，满足条件。

---

### 样例 2

#### 输入样例 2
```text
3
3 3 3
4 7 4
```

#### 输出样例 2
```text
No
```

---

### 样例 3

#### 输入样例 3
```text
1
2
2
```

#### 输出样例 3
```text
No
```


# AC Code 极端贪心

``` python
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
    brr = list(map(int, sys.stdin.readline().strip().split()))  
  
    # 构造  
    # 这wi的数值我们会这么大呀  
  
    diff = [ arr[i] - brr[i] for i in range(n) ]  
    # y = diff[i] * w[i] for i in range(n), 要确保y严格大于0  
  
    # 如何构造？贪心乱试试呗  
    crr = [1] * n  
    for i in range(n):  
        if diff[i] < 0:  
            crr[i] = 1  
        else:  
            crr[i] = 10 ** 9  
  
    y = 0  
    for num, weight in zip(diff, crr):  
        y += num * weight  
  
    if y > 0:  
        print("Yes")  
        print(*crr)  
    else:  
        print("No")  
  
if __name__ == "__main__":  
    main()
```