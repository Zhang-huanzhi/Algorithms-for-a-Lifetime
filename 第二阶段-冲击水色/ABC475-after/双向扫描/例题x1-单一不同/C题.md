https://atcoder.jp/contests/abc485/tasks/abc485_c

---

### C - Different Neighbor  
**时间限制**：2 sec / **内存限制**：1024 MiB  
**分值**：300 分

#### 题目描述
数轴上的坐标 $1, 2, \dots, N$ 处各有 $1$ 个点，位于坐标 $i$ 的点称为点 $i$。

每个点都有颜色，点 $i$ 的颜色为 $C_i$。其中，$C_1, C_2, \dots, C_N$ 中出现的值至少有 $2$ 种。

对于每个 $i = 1, 2, \dots, N$，在与点 $i$ 颜色不同的点中，求距离点 $i$ 最近的那个点到点 $i$ 的距离。

#### 约束条件
* $2 \le N \le 3 \times 10^5$
* $1 \le C_i \le N$
* $C_1, C_2, \dots, C_N$ 中出现的值至少有 $2$ 种
* 所有输入的值均为整数

#### 输入
输入通过标准输入按以下格式给出：
```
N
C_1 C_2 ... C_N
```

#### 输出
请按顺序输出 $i = 1, 2, \dots, N$ 对应的答案，中间用空格分隔。

---

#### 输入样例 1
```
6
1 1 2 3 3 3
```

#### 输出样例 1
```
2 1 1 1 2 3
```
* 在与点 $1$ 颜色不同的点中，距离点 $1$ 最近的是点 $3$，与点 $1$ 的距离为 $2$。
* 在与点 $2$ 颜色不同的点中，距离点 $2$ 最近的是点 $3$，与点 $2$ 的距离为 $1$。
* 在与点 $3$ 颜色不同的点中，距离点 $3$ 最近的是点 $2$ 和点 $4$，与点 $3$ 的距离为 $1$。
* 在与点 $4$ 颜色不同的点中，距离点 $4$ 最近的是点 $3$，与点 $4$ 的距离为 $1$。
* 在与点 $5$ 颜色不同的点中，距离点 $5$ 最近的是点 $3$，与点 $5$ 的距离为 $2$。
* 在与点 $6$ 颜色不同的点中，距离点 $6$ 最近的是点 $3$，与点 $6$ 的距离为 $3$。

因此，按此顺序以空格分隔输出 $2, 1, 1, 1, 2, 3$。

#### 输入样例 2
```
7
4 2 3 3 3 4 4
```

#### 输出样例 2
```
1 1 1 2 1 1 2
```

# AC 双向扫描
``` python
import sys  
  
def main():  
  
    n = int(sys.stdin.readline().strip())  
  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    ans = [n] * n  
    last_diff = -1  
    for i in range(1, n):  
        if arr[i] != arr[i - 1]:  
            last_diff = i - 1  
  
        if last_diff != -1:  
            ans[i] = min(ans[i], i - last_diff)  
  
    next_diff = -1  
    for i in range(n - 2, -1, -1):  
        if arr[i] != arr[i + 1]:  
            next_diff = i + 1  
        if next_diff != -1:  
            ans[i] = min(ans[i], next_diff - i)  
  
    print(*ans)  
  
if __name__ == "__main__":  
    main()
```