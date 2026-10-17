https://atcoder.jp/contests/abc449/tasks/abc449_d

---

### D - 制作靶心 2 (Make Target 2)

**时间限制**：2 秒 / **内存限制**：1024 MiB  
**分值**：425 分

#### 题目描述
在二维坐标平面上，每个坐标为 $(x, y)$ 的整数格点根据以下规则涂色：

* 当 $\max(|x|, |y|)$ 为**偶数**时，涂为**黑色**；
* 当 $\max(|x|, |y|)$ 为**奇数**时，涂为**白色**。

请计算满足以下所有条件的整数坐标点 $(x, y)$ 中，被涂为**黑色**的格点数量：

* $L \le x \le R$
* $D \le y \le U$

#### 制约条件
* $-10^6 \le L \le R \le 10^6$
* $-10^6 \le D \le U \le 10^6$
* 所有输入均为整数

#### 输入格式
输入通过标准输入按以下格式给出：

$$
L\ R\ D\ U
$$

#### 输出格式
输出满足条件的黑色格点数量。

---

#### 样例 1
**输入**
```text
-4 3 1 3
```
**输出**
```text
10
```
**说明**  
在满足 $-4 \le x \le 3$ 且 $1 \le y \le 3$ 的矩形区域内，黑色格点共有 $10$ 个。

---

#### 样例 2
**输入**
```text
-14 14 -14 14
```
**输出**
```text
449
```


# AC Code
``` python
import sys  
  
def main():  
    L, R, D, U = map(int, sys.stdin.readline().split())  
    ans = 0  
  
    # 1. 统计 |x| > |y| 且 |x| 为偶数的情况  
    for x in range(L, R + 1):  
        if x % 2 == 0:  
            ax = abs(x)  
            # y 范围: [-ax + 1, ax - 1] 与 [D, U] 取交集  
            y_min = max(-ax + 1, D)  
            y_max = min(ax - 1, U)  
            if y_max >= y_min:  
                ans += (y_max - y_min + 1)  
  
    # 2. 统计 |x| <= |y| 且 |y| 为偶数的情况  
    for y in range(D, U + 1):  
        if y % 2 == 0:  
            ay = abs(y)  
            # x 范围: [-ay, ay] 与 [L, R] 取交集  
            x_min = max(-ay, L)  
            x_max = min(ay, R)  
            if x_max >= x_min:  
                ans += (x_max - x_min + 1)  
  
    print(ans)  
  
if __name__ == '__main__':  
    main()
```