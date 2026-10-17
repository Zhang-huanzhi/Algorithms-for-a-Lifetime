https://atcoder.jp/contests/abc472/tasks/abc472_c

---

### C - On a Diet (节食)

时间限制：2 秒 / 内存限制：1024 MiB

分值：300 分

#### 题目描述
高桥君回老家探亲，一共会停留 $N$ 天。

在老家，每天都会准备点心，第 $i$ 天点心的卡路里为 $A_i$。

为了进行健康管理，高桥君决定：只要在最近 $M$ 天内吃掉的点心总卡路里不超过 $K$，他就吃掉当天的点心。

具体而言，对于 $i = 1, 2, \dots, N$，他按顺序遵循以下规则来决定是否吃第 $i$ 天的点心：

* 假设他吃了第 $i$ 天的点心，如果在第 $\max(i - M + 1, 1)$ 天到第 $i$ 天之间（包含首尾）所吃点心的总卡路里**小于等于** $K$，那么他就实际吃掉第 $i$ 天的点心；否则，他不吃第 $i$ 天的点心。

请对每一个 $i = 1, 2, \dots, N$，判断高桥君是否会吃第 $i$ 天的点心。

#### 数据范围
* $1 \le M \le N \le 2 \times 10^5$
* $1 \le K \le 10^{15}$
* $1 \le A_i \le 10^9$
* 所有输入均为整数

#### 输入格式
输入通过标准输入按以下格式给出：

$$N \quad M \quad K$$
$$A_1 \quad A_2 \quad \dots \quad A_N$$

#### 输出格式
输出 $N$ 行。
第 $i$ 行：如果高桥君吃第 $i$ 天的点心，输出 `Yes`；否则输出 `No`。

---

### 样例 1
#### 输入
```text
5 3 83
48 73 59 90 21
```
#### 输出
```text
Yes
No
No
No
Yes
```
#### 样例解释 1
在每一天，假设吃掉当天点心后，最近 3 天内所吃点心的总卡路里如下：
* 第 1 天：$48 \le 83 \implies$ **吃**（总卡路里 48）
* 第 2 天：$48 + 73 = 121 > 83 \implies$ **不吃**
* 第 3 天：$48 + 59 = 107 > 83 \implies$ **不吃**
* 第 4 天：$90 > 83$（第 1 天已超出最近 3 天范围，最近 3 天为第 2~4 天）$\implies$ **不吃**
* 第 5 天：$21 \le 83 \implies$ **吃**

---

### 样例 2
#### 输入
```text
7 4 728
187 816 349 609 255 308 175
```
#### 输出
```text
Yes
No
Yes
No
Yes
No
Yes
```

---

### 样例 3
#### 输入
```text
10 3 1368290936
216519459 804733999 297250023 775422599 287963235 999315644 354987425 974810607 653940822 117157941
```
#### 输出
```text
Yes
Yes
Yes
No
Yes
Yes
No
No
Yes
Yes
```

# AC Code

``` python
import sys  
  
def main():  
  
    n, m, k = map(int, sys.stdin.readline().strip().split())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    s = 0  
    for i in range(n):  
  
        if i >= m:  
            s -= arr[i - m]  
  
        if s + arr[i] <= k:  
            s += arr[i]  
            print("Yes")  
        else:  
            arr[i] = 0  
            print("No")  
  
if __name__ == "__main__":  
    main()
```