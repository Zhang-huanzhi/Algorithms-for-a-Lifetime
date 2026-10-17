https://atcoder.jp/contests/abc444/tasks/abc444_d

---
**D - Many Repunit Sum** / 
时间限制: 2 秒 / 内存限制: 1024 MiB

分值 : 400 分

### 题目描述
对于 $i=1,2,\dots,N$，定义 $B_i$ 为由 $A_i$ 个 $1$ 拼接而成的整数。
更严谨地，表示为：
$$B_i = \sum_{j=0}^{A_i - 1} 10^j$$

请计算下式的值：
$$\sum_{i=1}^{N} B_i$$

### 数据范围
* $1 \le N \le 2 \times 10^5$
* $1 \le A_i \le 2 \times 10^5$
* 输入的所有值均为整数。

### 输入格式
输入通过标准输入按以下格式给出：

$$N$$  
$$A_1$$  
$$A_2$$  
$$\vdots$$
$$A_N$$   

### 输出格式
在一行中输出答案。

---

### 样例输入 1
```text
4
3 3 3 3
```

### 样例输出 1
```text
444
```
**说明：**
因为 $B_1 = B_2 = B_3 = B_4 = 111$，所以 $B_1 + B_2 + B_3 + B_4 = 444$。

---

### 样例输入 2
```text
3
30 10 20
```

### 样例输出 2
```text
111111111122222222223333333333
```
**说明：**
答案可能会非常大。

---

### 样例输入 3
```text
10
1 2 3 4 5 6 7 8 9 10
```

### 样例输出 3
```text
1234567900
```

# AC Code
``` python
import sys  
  
  
def main():  
      
    n = int(sys.stdin.readline().strip())  
    arr = list(map(int, sys.stdin.readline().strip().split()))  
  
    max_val = max(arr)  
  
    # 差分数组  
    diff = [0] * (max_val + 2)  
    for num in arr:  
        diff[1] += 1  
        diff[num + 1] -= 1  
  
    # 还原 xrr，xrr[i] 表示第 i 位的数字累加和（1-based）  
    xrr = [0] * (max_val + 1)  
    for i in range(1, max_val + 1):  
        xrr[i] = xrr[i - 1] + diff[i]  
  
    # 手动模拟进位  
    ans_digits = []  
    carry = 0  
    for i in range(1, max_val + 1):  
        total = xrr[i] + carry  
        ans_digits.append(total % 10)  # 当前位的最终数字  
        carry = total // 10  # 进位到下一位  
  
    # 处理最后剩下的进位  
    while carry > 0:  
        ans_digits.append(carry % 10)  
        carry //= 10  
  
    # 因为是从低位（个位）开始计算并添加的，输出时需要逆序  
    ans_digits.reverse()  
  
    # 输出结果  
    print(''.join(map(str, ans_digits)))  
  
  
if __name__ == "__main__":  
    main()
```

# 暴力模拟Code
``` python
import sys

def main():
    n = int(sys.stdin.readline().strip())
    arr = list(map(int, sys.stdin.readline().strip().split()))

    res = 0
    for num in arr:
        x = '1'*num
        res += int(x)

    print(res)

if __name__ == "__main__":
    main()

```